# Submit Storm topologies to process data in Kafka on E-MapReduce {#concept_wnl_wkj_gfb .concept}

This topic describes how to deploy Storm clusters and Kafka clusters on E-MapReduce and run Storm topologies to consume data in Kafka.

## Prepare the environment {#section_rnx_xkj_gfb .section}

The test is performed using EMR that is deployed in the China East 1 \(Hangzhou\) region. The version of EMR is 3.8.0. The component versions required for this test are as follows.

-   Kafka: 2.11\_1.0.0
-   Storm: 1.0.1

In this topic, we use Alibaba Cloud E-MapReduce to create a Kafka cluster automatically. For more information, see [Create a cluster](../DNemapreduce1883011/EN-US_TP_17840.dita#concept_nrp_154_y2b).

-   Create a Hadoop cluster

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21765/154708875012655_en-US.png)

-   Create a Kafka cluster

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21765/154708875012657_en-US.png)

    **Note:** 

    -   If you choose classic network as the network type, put the Hadoop cluster and the Kafka cluster in the same security group to save time for configuring connections between instances.
    -   If you choose VPC as the network type, put the Hadoop cluster and the Kafka cluster in the same VPC and the same security group to save time for configuring a VPC peering connection.
    -   If you are familiar with networking and security groups for ECS, you can create configurations as needed.
-   Configure the environment for Storm

    Consuming Kafka data fails if you run Storm topologies in the initial environment. To avoid such failures, you need to install the following dependencies for the Storm environment:

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
    These dependencies have been tested. If you need additional dependencies, perform the following operations to add them to the lib folder of Storm.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21765/154708875012659_en-US.png)

    You need to perform the preceding operations on each node in the Hadoop cluster. After the operations are complete, restart Storm in the E-MapReduce console as shown in the following figure.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21765/154708875012660_en-US.png)

    You can view operation logs to check the status of Storm:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21765/154708875112661_en-US.png)


## Create Storm topologies and Kafka topics {#section_x4k_k4j_gfb .section}

-   E-MapReduce provides sample code that you can use directly. The links are as follows:

-   [e-mapreduce-demo](https://github.com/aliyun/aliyun-emapreduce-demo)
-   [e-mapreduce-sdk](https://github.com/aliyun/aliyun-emapreduce-sdk)
-   Write data to topics

    1.  Log on to the Kafka cluster.
    2.  Create a test topic with 10 partitions and 2 replicas.

        ```
        /usr/lib/kafka-current/bin/kafka-topics.sh --partitions 10 --replication-factor 2 --zookeeper emr-header-1:/kafka-1.0.0 --topic test --create
        ```

    3.  Write 100 records of data to the test topic.

        ```
        /usr/lib/kafka-current/bin/kafka-producer-perf-test.sh --num-records 100 --throughput 10000 --record-size 1024 --producer-props bootstrap.servers=emr-worker-1:9092 --topic test
        ```

    **Note:** The preceding command is run on the emr-header-1 node in the Kafka cluster. You can also run the command on client nodes.

-   Run a Storm topology.

    Log on to the Hadoop cluster, copy the examples-1.1-shaded.jar file \(the test topic data is compiled to this file in step 2\) to the emr-header-1 node. In this example, the file is stored in the HDFS root directory. Run the following command to submit the topology:

    ```
    /usr/lib/storm-current/bin/storm jar examples-1.1-shaded.jar com.aliyun.emr.example.storm.StormKafkaSample test aaa.bbb.ccc.ddd hdfs://emr-header-1:9000 sample
    ```

-   View the running status of a topology
    -   View the running status of Storm

        You can use the Web UI to view the services on a cluster in the following ways:

        -   With Knox. For more information, see [Knox instructions](../DNemapreduce1876943/EN-US_TP_17921.dita#concept_knp_s1x_y2b).
        -   Use SSH. For more information, see [Use SSH to log on to a cluster](../DNemapreduce1876943/EN-US_TP_17923.dita#concept_sns_sww_y2b).
        In this topic, we use SSH to access the Web UI. The endpoint is http://localhost:9999/index.html. You can see the topology that we have submitted. Click the topology to view the running logs:

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21765/154708875112663_en-US.png)

    -   View the output files in HDFS
        -   View the output files in HDFS.

            ```
            [root@emr-header-1 ~]# hadoop fs -ls /foo/
            -rw-r--r--   3 root hadoop     615000 2018-02-11 13:37 /foo/bolt-2-0-1518327393692.txt
            -rw-r--r--   3 root hadoop     205000 2018-02-11 13:37 /foo/bolt-2-0-1518327441777.txt
            [root@emr-header-1 ~]# hadoop fs -cat /foo/bolt-2-0-1518327441777.txt | wc -l
            200
            ```

        -   Write 120 records of data to the test topic in Kafka.

            ```
            [root@emr-header-1 ~]# /usr/lib/kafka-current/bin/kafka-producer-perf-test.sh --num-records 120 --throughput 10000 --record-size 1024 --producer-props bootstrap.servers=emr-worker-1:9092 --topic test
            120 records sent, 816.326531 records/sec (0.80 MB/sec), 35.37 ms avg latency, 134.00 ms max latency, 35 ms 50th, 39 ms 95th, 41 ms 99th, 134 ms 99.9th.
            ```

        -   Output the line number of the HDFS file.

            ```
            [root@emr-header-1 ~]# hadoop fs -cat /foo/bolt-2-0-1518327441777.txt | wc -l
            320
            ```


## Summary {#section_bxc_lqj_gfb .section}

We have successfully deployed a Storm cluster and a Kafka cluster on E-MapReduce, run a Storm topology and consumed Kafka data. E-MapReduce also supports the Spark streaming and the Flink components, which can run in Hadoop clusters and process Kafka data.

**Note:** 

E-MapReduce does not provide the Storm cluster option. Therefore, we have created a Hadoop cluster and have installed the Storm components. If you do not need to use other components, you can easily disable them in the E-MapReduce console. Then a Hadoop cluster is equivalent to a Storm cluster.

