# 使用E-MapReduce提交Storm作业处理Kafka数据 {#concept_wnl_wkj_gfb .concept}

本文演示如何在E-MapReduce上部署Storm集群和Kafka集群，并运行Storm作业消费Kafka数据。

## 环境准备 {#section_rnx_xkj_gfb .section}

本文选择在杭州Region进行测试，版本选择EMR-3.8.0，本次测试需要的组件版本有：

-   Kafka：2.11\_1.0.0
-   Storm: 1.0.1

本文使用阿里云EMR服务自动化搭建Kafka集群，详细过程请参考[创建集群](../../../../../intl.zh-CN/快速入门/创建 E-MapReduce/创建集群.md#)。

-   创建Hadoop集群

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21765/154743236612655_zh-CN.png)

-   创建Kafka集群

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21765/154743236612657_zh-CN.png)

    **说明：** 

    -   如果使用经典网络，请注意将Hadoop集群和Kafka集群放置在同一个安全组下面，这样可以省去配置安全组，避免网络不通的问题。
    -   如果使用VPC网络，请注意将Hadoop集群和Kafka集群放置在同一个VPC/VSwitch以及安全组下面，这样同样省去配置网路和安全组，避免网络不通。
    -   如果你熟悉ECS的网络和安全组，可以按需配置。
-   配置Storm环境

    如果我们想在Storm上运行作业消费Kafka的话，集群初始环境下是会失败的，因为Storm运行环境缺少了不少必须的依赖包，如下：

    -   [curator-client](http://central.maven.org/maven2/org/apache/curator/curator-client/2.10.0/curator-client-2.10.0.jar)
    -   [curator-framework](http://central.maven.org/maven2/org/apache/curator/curator-framework/2.10.0/curator-framework-2.10.0.jar)
    -   [curator-recipes](http://central.maven.org/maven2/org/apache/curator/curator-recipes/2.10.0/curator-recipes-2.10.0.jar)
    -   [json-simple](http://central.maven.org/maven2/com/googlecode/json-simple/json-simple/1.1/json-simple-1.1.jar)
    -   [metrics-core](http://central.maven.org/maven2/com/yammer/metrics/metrics-core/2.2.0/metrics-core-2.2.0.jar)
    -   [scala-library](http://central.maven.org/maven2/org/scala-lang/scala-library/2.11.7/scala-library-2.11.7.jar)
    -   [zookeeper](http://central.maven.org/maven2/org/apache/zookeeper/zookeeper/3.4.6/zookeeper-3.4.6.jar)
    -   [commons-cli](http://central.maven.org/maven2/commons-cli/commons-cli/1.3.1/commons-cli-1.3.1.jar)
    -   [commons-collections](http://central.maven.org/maven2/commons-collections/commons-collections/3.2.2/commons-collections-3.2.2.jar)
    -   [commons-configuration](http://central.maven.org/maven2/commons-configuration/commons-configuration/1.6/commons-configuration-1.6.jar)
    -   [htrace-core](http://central.maven.org/maven2/org/htrace/htrace-core/3.0.4/htrace-core-3.0.4.jar)
    -   [jcl-over-slf4j](http://central.maven.org/maven2/org/slf4j/jcl-over-slf4j/1.6.6/jcl-over-slf4j-1.6.6.jar)
    -   [protobuf-java](http://central.maven.org/maven2/com/google/protobuf/protobuf-java/2.5.0/protobuf-java-2.5.0.jar)
    -   [guava](http://search.maven.org/remotecontent?filepath=com/google/guava/guava/23.0/guava-23.0.jar)
    -   [hadoop-common](http://central.maven.org/maven2/org/apache/hadoop/hadoop-common/3.0.0/hadoop-common-3.0.0.jar)
    -   [kafka-clients](http://central.maven.org/maven2/org/apache/kafka/kafka-clients/1.0.0/kafka-clients-1.0.0.jar)
    -   [kafka](http://central.maven.org/maven2/org/apache/kafka/kafka_2.10/0.10.0.1/kafka_2.10-0.10.0.1.jar)
    -   [storm-hdfs](http://central.maven.org/maven2/org/apache/storm/storm-hdfs/1.1.2/storm-hdfs-1.1.2.jar)
    -   [storm-kafka](http://central.maven.org/maven2/org/apache/storm/storm-kafka/1.1.2/storm-kafka-1.1.2.jar)
    以上版本依赖包经过测试可用，如果你再测试过程中引入了其他依赖，也一同添加在Storm lib中，具体操作如下：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21765/154743236612659_zh-CN.png)

    上述操作需要在Hadoop集群的每台机器执行一遍。执行完在E-MapReduce控制台重启Storm服务，如下：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21765/154743236612660_zh-CN.png)

    查看操作历史，待Storm重启完毕：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21765/154743236612661_zh-CN.png)


## 开发Storm和Kafka作业 {#section_x4k_k4j_gfb .section}

-   E-MapReduce已经提供了现成的示例代码，直接使用即可，地址如下：

-   [e-mapreduce-demo](https://github.com/aliyun/aliyun-emapreduce-demo)
-   [e-mapreduce-sdk](https://github.com/aliyun/aliyun-emapreduce-sdk)
-   Topic数据准备

    1.  登录到Kafka集群
    2.  创建一个test topic，分区数10，副本数2

        ```
        /usr/lib/kafka-current/bin/kafka-topics.sh --partitions 10 --replication-factor 2 --zookeeper emr-header-1:/kafka-1.0.0 --topic test --create
        ```

    3.  向test topic写入100条数据

        ```
        /usr/lib/kafka-current/bin/kafka-producer-perf-test.sh --num-records 100 --throughput 10000 --record-size 1024 --producer-props bootstrap.servers=emr-worker-1:9092 --topic test
        ```

    **说明：** 以上命令在kafka集群的emr-header-1节点执行，当然也可以客户端机器上执行。

-   运行Storm作业

    登录到Hadoop集群，将编译得到的/target/shaded目录下的examples-1.1-shaded.jar拷贝到集群emr-header-1上，这里我放在root根目录下面。提交作业：

    ```
    /usr/lib/storm-current/bin/storm jar examples-1.1-shaded.jar com.aliyun.emr.example.storm.StormKafkaSample test aaa.bbb.ccc.ddd hdfs://emr-header-1:9000 sample
    ```

-   查看作业运行
    -   查看Storm运行状态

        查看集群上服务的WebUI有2种方式:

        -   通过Knox方式，参考文档[Knox 使用说明](../../../../../intl.zh-CN/用户指南/开源组件介绍/Knox 使用说明.md#)
        -   SSH隧道，参考文档[SSH 登录集群](../../../../../intl.zh-CN/用户指南/SSH 登录集群.md#)
        本文选择使用SSH隧道方式，访问地址：http://localhost:9999/index.html。可以看到我们刚刚提交的Topology。点进去可以看到执行详情：

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21765/154743236712663_zh-CN.png)

    -   查看HDFS输出
        -   查看HDFS文件输出

            ```
            [root@emr-header-1 ~]# hadoop fs -ls /foo/
            -rw-r--r--   3 root hadoop     615000 2018-02-11 13:37 /foo/bolt-2-0-1518327393692.txt
            -rw-r--r--   3 root hadoop     205000 2018-02-11 13:37 /foo/bolt-2-0-1518327441777.txt
            [root@emr-header-1 ~]# hadoop fs -cat /foo/bolt-2-0-1518327441777.txt | wc -l
            200
            ```

        -   向kafka写120条数据

            ```
            [root@emr-header-1 ~]# /usr/lib/kafka-current/bin/kafka-producer-perf-test.sh --num-records 120 --throughput 10000 --record-size 1024 --producer-props bootstrap.servers=emr-worker-1:9092 --topic test
            120 records sent, 816.326531 records/sec (0.80 MB/sec), 35.37 ms avg latency, 134.00 ms max latency, 35 ms 50th, 39 ms 95th, 41 ms 99th, 134 ms 99.9th.
            ```

        -   查看HDFS文件输出

            ```
            [root@emr-header-1 ~]# hadoop fs -cat /foo/bolt-2-0-1518327441777.txt | wc -l
            320
            ```


## 总结 {#section_bxc_lqj_gfb .section}

至此，我们成功实现了在E-MapReduce上部署一套Storm集群和一套Kafka集群，并运行Storm作业消费Kafka数据。当然，E-MapReduce也支持Spark Streaming和Flink组件，同样可以方便在Hadoop集群上运行，处理Kafka数据。

**说明：** 

由于E-MapReduce没有单独的Storm集群类别，所以我们是创建的Hadoop集群，并安装了Storm组件。如果你在使用过程中用不到其他组件，可以很方便地在E-MapReduce管理控制台将那些组件停掉。这样，可以将Hadoop集群作为一个纯粹的Storm集群使用。

