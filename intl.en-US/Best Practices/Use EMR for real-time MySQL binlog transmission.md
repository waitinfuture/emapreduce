# Use EMR for real-time MySQL binlog transmission {#concept_mzl_txz_cfb .concept}

This section describes how to use the SLS plug-in function of Alibaba Cloud and the E-MapReduce cluster to implement quasi-real-time transmission of MySQL binlog.

## Basic architecture {#section_wyp_5xz_cfb .section}

RDS -\> SLS -\> Spark Streaming -\> Spark HDFS

The preceding links contain three processes:

1.  How to collect RDS binlog to SLS.
2.  How to read and analyze the logs in SLS through Spark Streaming.
3.  How to save the logs read and processed in the second link to Spark HDFS.

## Prepare the environment {#section_kzp_byz_cfb .section}

1.  Install a MySQL database \(using MySQL protocol, such as RDS and DRDS\), and enable the log-bin function. Configure the binlog type to ROW mode. \(RDS is enabled by default.\)
2.  Enable the SLS service.

## Procedure {#section_oxt_3yz_cfb .section}

1.  Check the MySQL database environment.
    1.  View whether the log-bin function is enabled.

        ```
        mysql> show variables like "log_bin";
        +---------------+-------+
        | Variable_name | Value |
        +---------------+-------+
        | log_bin       | ON    |
        +---------------+-------+
        1 row in set (0.02 sec)
        ```

    2.  View the binlog type.

        ```
        mysql> show variables like "binlog_format";
        +---------------+-------+
        | Variable_name | Value |
        +---------------+-------+
        | binlog_format | ROW   |
        +---------------+-------+
        1 row in set (0.03 sec)
        ```

2.  Add user permissions. You can also add user permissions directly from the RDS console.

    ```
    CREATE USER canal IDENTIFIED BY 'canal';
    GRANT SELECT, REPLICATION SLAVE, REPLICATION CLIENT ON *. * TO 'canal'@'%';
    FLUSH PRIVILEGES;
    ```

3.  Add the corresponding configuration file for the SLS service, and check if the data is collected properly.
    1.  Add the corresponding project and logstore in the SLS console. For example, create a project named canaltest and a logstore named canal.
    2.  Configure SLS: create a file named user\_local\_config.json under the directory of /etc/ilogtail.

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

        The information such as host and password in detail is MySQL database information, and the user information is the user name authorized previously. AliUid, defaultEndpoint, project\_name, and category are information related with users and SLS. Fill in the information according to your actual situation.

    3.  Wait about 2 minutes to see if the log data has been uploaded successfully in the SLS console.

        If the log data acquisition is not successful, view the acquisition log of SLS based on its prompt for troubleshooting.

4.  Prepare and compile the code to jar package, and upload it to OSS.
    1.  Copy the example code of EMR using Git and modify the code. The command is as follows: `git clone https://github.com/aliyun/aliyun-emapreduce-demo.git`. The example code includes the LoghubSample class, which is primarily used to capture and print data from SLS. The modified code is as below:

        ```
        package com.aliyun.emr.example
        import org.apache.spark.SparkConf
        import org.apache.spark.storage.StorageLevel
        import org.apache.spark.streaming.aliyun.logservice.LoghubUtils
        import org.apache.spark.streaming.{ Milliseconds, StreamingContext}
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
         qendpoint,
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

        The main change is as follows: `loghubStream.foreachRDD(rdd => rdd.saveAsObjectFile("/mysqlbinlog") )`. When the example code is run in the EMR cluster, the data that flows out of Spark Streaming will be saved in HDFS of EMR.

        **Note:** 

        -   To run the example code locally, create a Hadoop cluster in the local environment in advance.
        -   Because the Spark SDK of EMR is updated, its example code is old and cannot directly transfer the AccessKey ID and AccessKey Secret of OSS in the parameter. You need to set the Spark SDK with the SparkConf constructor, as shown in the following figure:

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

        -   During local debugging, you need to change /mysqlbinlogloghubStream.foreachRDD\(rdd =\> in rdd.saveAsObjectFile\("/mysqlbinlog"\) \) to the local HDFS address.
    2.  Compile code.

        After local debugging is complete, you can run the following command to package and compile the code:

        ```
        mvn clean install
        ```

    3.  Upload the jar package.

        Create a directory on an OSS instance where the bucket is qiaozhou-EMR/jar, and upload examples-1.1-shaded.jar under the directory of /target/shaded to the OSS directory through the OSS console or the SDK of OSS. The uploaded jar package address is oss://qiaozhou-EMR/jar/examples-1.1-shaded.jar. This address will be used later.

5.  Create an EMR cluster and tasks, and run the execution plans.
    1.  Create an EMR cluster in the EMR console, which takes about 10 minutes.
    2.  Create a job of the Spark type.

        Replace `SLS_endpoint` `$SLS_access_id` `$SLS_secret_key` with your actual values. Make sure that the order of the parameters is correct. Otherwise, errors may be reported.

        ```
        --master yarn --deploy-mode client --driver-memory 4g --executor-memory 2g --executor-cores 2 --class com.aliyun.EMR.example.LoghubSample ossref://EMR-test/jar/examples-1.1-shaded.jar canaltest canal sparkstreaming $SLS_endpoint $SLS_access_id $SLS_secret_key 1
        ```

    3.  After the execution plan is created, bind jobs to the EMR cluster. Start to run the jobs.
    4.  Search for the IP address of the master node.

        After you login through SSH, run the following command:

        ```
        hadoop fs -ls /
        ```

        You can see the directory at the beginning of mysqlbinlog, and view the mysqlbinlog file with the following command:

        ```
        hadoop fs -ls /mysqlbinlog
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17964/154815067611868_en-US.png)

        You can also run `hadoop fs -cat /mysqlbinlog/part-00000` command to view the file content.

6.  Troubleshoot.

    If you don’t see the normal results, you can troubleshoot problems in the running records of EMR.


