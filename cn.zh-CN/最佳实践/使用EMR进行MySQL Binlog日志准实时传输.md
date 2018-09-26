# 使用EMR进行MySQL Binlog日志准实时传输 {#concept_mzl_txz_cfb .concept}

本文介绍如何利用阿里云的SLS插件功能和EMR集群进行MySQL binlog的准实时传输。

## 基本架构 {#section_wyp_5xz_cfb .section}

RDS -\> SLS -\> Spark Streaming -\> Spark HDFS

上述链路主要包含3个过程：

1.  如何把RDS的binlog收集到SLS。
2.  如何通过Spark Streaming将SLS中的日志读取出来，进行分析。
3.  如何把链路2中读取和处理过的日志，保存到Spark HDFS中。

## 环境准备 {#section_kzp_byz_cfb .section}

1.  安装一个MySQL类型的数据库（使用MySQL协议，例如RDS、DRDS等\)，开启log-bin功能，且配置binlog类型为ROW模式（RDS默认开启）。
2.  开通SLS服务。

## 操作步骤 {#section_oxt_3yz_cfb .section}

1.  检查MySQL数据库环境。
    1.  查看是否开启log-bin功能。

        ```
        mysql> show variables like "log_bin";
        +---------------+-------+
        | Variable_name | Value |
        +---------------+-------+
        | log_bin       | ON    |
        +---------------+-------+
        1 row in set (0.02 sec)
        ```

    2.  查看binlog类型。

        ```
        mysql> show variables like "binlog_format";
        +---------------+-------+
        | Variable_name | Value |
        +---------------+-------+
        | binlog_format | ROW   |
        +---------------+-------+
        1 row in set (0.03 sec)
        ```

2.  添加用户权限。（也可以直接通过RDS控制台添加）

    ```
    CREATE USER canal IDENTIFIED BY 'canal';
    GRANT SELECT, REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'canal'@'%';
    FLUSH PRIVILEGES;
    ```

3.  为SLS服务添加对应的配置文件，并检查数据是否正常采集。
    1.  在SLS控制台添加对应的project和logstore，例如：创建一个名称为canaltest的project，然后创建一个名称为canal的logstore。
    2.  对SLS进行配置：在/etc/ilogtail目录下创建文件user\_local\_config.json，具体配置如下：

        ```
        {
        "metrics": {
           "##1.0##canaltest$plugin-local": {
               "aliuid": "****",
               "enable": true,
               "category": "canal",
               "defaultEndpoint": "*******",
               "project_name": "canaltest",
               "region": "cn-hangzhou",
               "version": 2
               "log_type": "plugin",
               "plugin": {
                   "inputs": [
                       {
                           "type": "service_canal",
                           "detail": {
                               "Host": "*****",
                               "Password": "****",
                               "ServerID": ****,
                               "User" : "***",
                               "DataBases": [
                                   "yourdb"
                               ],
                               "IgnoreTables": [
                                   "\\S+_inner"
                               ],
                                "TextToString" : true
                           }
                       }
                   ],
                   "flushers": [
                       {
                           "type": "flusher_sls",
                           "detail": {}
                       }
                   ]
               }
           }
        }
        }
        ```

        其中detail中的Host和Password等信息为MySQL数据库信息，User信息为之前授权过的用户名。aliUid、defaultEndpoint、project\_name、category请根据自己的实际情况填写对应的用户和SLS信息。

    3.  等待约2分钟，通过SLS控制台查看日志数据是否上传成功，具体如图所示。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17964/153795221711863_zh-CN.png)

        如果日志数据没有采集成功，请根据SLS的提示，查看SLS的采集日志进行排查。

4.  准备代码，将代码编译成jar包，然后上传到OSS。
    1.  将EMR的example代码通过git复制下来，然后进行修改，具体命令为：`git clone https://github.com/aliyun/aliyun-emapreduce-demo.git`。example代码中已经有LoghubSample类，该类主要用于从SLS采集数据并打印。以下是修改后的代码，供参考：

        ```
        package com.aliyun.emr.example
        import org.apache.spark.SparkConf
        import org.apache.spark.storage.StorageLevel
        import org.apache.spark.streaming.aliyun.logservice.LoghubUtils
        import org.apache.spark.streaming.{Milliseconds, StreamingContext}
        object LoghubSample {
        def main(args: Array[String]): Unit = {
        if (args.length < 7) {
         System.err.println(
           """Usage: bin/spark-submit --class LoghubSample examples-1.0-SNAPSHOT-shaded.jar
             |            
             |           
           """.stripMargin)
         System.exit(1)
        }
        val loghubProject = args(0)
        val logStore = args(1)
        val loghubGroupName = args(2)
        val endpoint = args(3)
        val accessKeyId = args(4)
        val accessKeySecret = args(5)
        val batchInterval = Milliseconds(args(6).toInt * 1000)
        val conf = new SparkConf().setAppName("Mysql Sync")
        //    conf.setMaster("local[4]");
        val ssc = new StreamingContext(conf, batchInterval)
        val loghubStream = LoghubUtils.createStream(
         ssc,
         loghubProject,
         logStore,
         loghubGroupName,
         endpoint,
         1,
         accessKeyId,
         accessKeySecret,
         StorageLevel.MEMORY_AND_DISK)
        loghubStream.foreachRDD(rdd =>
           rdd.saveAsTextFile("/mysqlbinlog")
        )
        ssc.start()
        ssc.awaitTermination()
        }
        }
        ```

        其中的主要改动是：`loghubStream.foreachRDD(rdd => rdd.saveAsObjectFile("/mysqlbinlog") )`。这样在EMR集群中运行时，就会把Spark Streaming中流出来的数据，保存到EMR的HDFS中。

        **说明：** 

        -   由于如果要在本地运行，请在本地环境提前搭建Hadoop集群。
        -   由于EMR的Spark SDK做了升级，其example code比较旧，不能直接在参数中传递OSS的AccessKeyId、AccessKeySecret， 而是需要通过SparkConf进行设置，如下所示。

            ```
            trait RunLocally {
            val conf = new SparkConf().setAppName(getAppName).setMaster("local[4]")
            conf.set("spark.hadoop.fs.oss.impl", "com.aliyun.fs.oss.nat.NativeOssFileSystem")
            conf.set("spark.hadoop.mapreduce.job.run-local", "true")
            conf.set("spark.hadoop.fs.oss.endpoint", "YourEndpoint")
            conf.set("spark.hadoop.fs.oss.accessKeyId", "YourId")
            conf.set("spark.hadoop.fs.oss.accessKeySecret", "YourSecret")
            conf.set("spark.hadoop.job.runlocal", "true")
            conf.set("spark.hadoop.fs.oss.impl", "com.aliyun.fs.oss.nat.NativeOssFileSystem")
            conf.set("spark.hadoop.fs.oss.buffer.dirs", "/mnt/disk1")
            val sc = new SparkContext(conf)
            def getAppName: String
            }
            ```

        -   在本地调试时，需要把loghubStream.foreachRDD\(rdd =\> rdd.saveAsObjectFile\("/mysqlbinlog"\) \)中的/mysqlbinlog修改成本地HDFS的地址。
    2.  代码编译。

        在本地调试完成后，我们可以通过如下命令进行打包编译：

        ```
        mvn clean install
        ```

    3.  上传jar包。

        请先在OSS上建立bucket为qiaozhou-EMR/jar的目录，然后通过OSS控制台或OSS的SDK将/target/shaded目录下的examples-1.1-shaded.jar上传到oss的这个目录下。上传后的jar包地址为oss://qiaozhou-EMR/jar/examples-1.1-shaded.jar，这个地址在后面会用上，如下图所示：

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17964/153795221711865_zh-CN.png)

5.  搭建EMR集群，创建任务并运行执行计划。
    1.  通过EMR控制台创建一个EMR集群，大约需要10分钟左右，请耐心等待。
    2.  创建一个类型为Spark的作业。

        请根据您具体的配置将SLS\_endpoint $SLS\_access\_id $SLS\_secret\_key替换成真实值。请注意参数的顺序，否则可能会报错。

        ```
        --master yarn --deploy-mode client --driver-memory 4g --executor-memory 2g --executor-cores 2 --class com.aliyun.EMR.example.LoghubSample ossref://EMR-test/jar/examples-1.1-shaded.jar canaltest canal sparkstreaming $SLS_endpoint $SLS_access_id $SLS_secret_key 1
        ```

    3.  创建执行计划，将作业和EMR集群绑定后，开始运行。
    4.  查询Master节点的IP，如图所示：

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17964/153795221711867_zh-CN.png)

        通过SSH登录后，执行以下命令：

        ```
        hadoop fs -ls /
        ```

        可以看到mysqlbinlog开头的目录，再通过以下命令查看mysqlbinlog文件:

        ```
        hadoop fs -ls /mysqlbinlog
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17964/153795221711868_zh-CN.png)

        还可以通过`hadoop fs -cat /mysqlbinlog/part-00000`命令查看文件内容。

6.  错误排查。

    如果没有看到正常的结果，可以通过EMR的运行记录，来进行问题排查，如图所示：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17964/153795221711869_zh-CN.png)


