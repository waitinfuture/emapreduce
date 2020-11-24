# Submit Storm jobs in EMR to process Kafka data

This topic describes how to create a Storm cluster and a Kafka cluster in the E-MapReduce \(EMR\) console, and how to run Storm jobs to process Kafka data.

## Prepare the environment

Select the China \(Hangzhou\) region and use EMR V3.8.0 for a test. The following components are required for this test:

-   Kafka: 2.11\_1.0.0
-   Storm: 1.0.1

Create a Kafka cluster in the Alibaba Cloud EMR console. For more information, see [Create a cluster](/intl.en-US/Quick Start/Create a cluster.md).

-   Create a Hadoop cluster.

    ![hadoop](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6298794061/p111847.png)

-   Create a Kafka cluster.

    ![kafka](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7495326061/p12657.png)

    **Note:**

    -   If you use the classic network, you must place the Hadoop cluster and Kafka cluster in the same security group. In this case, you do not need to configure two different security groups, and network connection errors can be avoided.
    -   If you use VPCs, you must place the Hadoop cluster and Kafka cluster in the same VPC or VSwitch and the same security group. In this case, network and security group configurations are simplified, and network connection errors are avoided.
    -   If you are familiar with network connections and security group configurations on ECS instances, you can configure your clusters based on your business requirements.
-   Configure the Storm environment.

    Operations that run Storm jobs to process Kafka data fail in the initial Storm environment. This is because the required dependencies are missing. The required dependencies include curator-client, curator-framework, curator-recipes, json-simple, metrics-core, scala-library, zookeeper, commons-cli, commons-collections, commons-configuration, htrace-core, jcl-over-slf4j, protobuf-java, guava, hadoop-common, kafka-clients, kafka, storm-hdfs, and storm-kafka.

    If other dependencies are introduced in the test, add these dependencies to the lib directory of Storm, as shown in the following figure.

    ![Storm lib](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8064879751/p12659.png)

    You need to perform the preceding operations on each node in the Hadoop cluster. After the operations are completed, restart Storm in the EMR console, as shown in the following figure.

    ![restart](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1497593061/p12660.png)

    View operational logs to check the status of Storm.

    ![result](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9064879751/p12661.png)


## Develop a Storm job and a Kafka job

-   EMR provides sample code that you can use directly. For more information, see the following topics:

-   [e-mapreduce-demo](https://github.com/aliyun/aliyun-emapreduce-demo)
-   [e-mapreduce-sdk](https://github.com/aliyun/aliyun-emapreduce-sdk)
-   Prepare topic data.

    1.  Log on to the Kafka cluster.
    2.  Create a test topic that has 10 partitions and 2 replicas.

        ```
        /usr/lib/kafka-current/bin/kafka-topics.sh --partitions 10 --replication-factor 2 --zookeeper emr-header-1:/kafka-1.0.0 --topic test --create
        ```

    3.  Write 100 data records to the test topic.

        ```
        /usr/lib/kafka-current/bin/kafka-producer-perf-test.sh --num-records 100 --throughput 10000 --record-size 1024 --producer-props bootstrap.servers=emr-worker-1:9092 --topic test
        ```

    **Note:** The preceding commands are executed on the emr-header-1 node in the Kafka cluster. You can also run the commands on your client.

-   Run a Storm job.

    Log on to the Hadoop cluster. Copy examples-1.1-shaded.jar that is stored in the /target/shaded directory to the emr-header-1 node. In this example, copy examples-1.1-shaded.jar to the root directory of the emr-header-1 node.

    ```
    /usr/lib/storm-current/bin/storm jar examples-1.1-shaded.jar com.aliyun.emr.example.storm.StormKafkaSample test aaa.bbb.ccc.ddd hdfs://emr-header-1:9000 sample
    ```

-   View job running details.
    -   View the running status of the Storm job.

        You can use one of the following methods to view the running status of the Storm job on the web UI:

        -   Use Knox. For more information, see [Knox](/intl.en-US/Cluster types/Hadoop cluster/Knox.md).
        -   Use an SSH tunnel. For more information, see [Create an SSH tunnel to access web UIs of open source components](/intl.en-US/Cluster Management/Configure clusters/Connect to a cluster/Create an SSH tunnel to access web UIs of open source components.md).
        In this topic, an SSH tunnel is used to access the web UI. You can view the submitted Storm job at the following URL: http://localhost:9999/index.html. You can click this job to view running details.

        ![information](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9064879751/p12663.png)

    -   View the output in HDFS.
        -   View the output files in HDFS.

            ```
            [root@emr-header-1 ~]# hadoop fs -ls /foo/
            -rw-r--r--   3 root hadoop     615000 2018-02-11 13:37 /foo/bolt-2-0-1518327393692.txt
            -rw-r--r--   3 root hadoop     205000 2018-02-11 13:37 /foo/bolt-2-0-1518327441777.txt
            [root@emr-header-1 ~]# hadoop fs -cat /foo/bolt-2-0-1518327441777.txt | wc -l
            200
            ```

        -   Write 120 data records to the test topic in the Kafka cluster.

            ```
            [root@emr-header-1 ~]# /usr/lib/kafka-current/bin/kafka-producer-perf-test.sh --num-records 120 --throughput 10000 --record-size 1024 --producer-props bootstrap.servers=emr-worker-1:9092 --topic test
            120 records sent, 816.326531 records/sec (0.80 MB/sec), 35.37 ms avg latency, 134.00 ms max latency, 35 ms 50th, 39 ms 95th, 41 ms 99th, 134 ms 99.9th.
            ```

        -   View the output files in HDFS again.

            ```
            [root@emr-header-1 ~]# hadoop fs -cat /foo/bolt-2-0-1518327441777.txt | wc -l
            320
            ```


## Summary

A Storm cluster and a Kafka cluster are created in the EMR console, and a Storm job is run to process Kafka data. EMR also supports Spark Streaming and Flink. You can run Spark Streaming and Flink jobs in a Hadoop cluster to process Kafka data.

**Note:**

The EMR console does not provide the Storm cluster type. Therefore, a Hadoop cluster is created and a Storm component is deployed on the Hadoop cluster. If you do not require other components in the Hadoop cluster, you can disable them in the EMR console. Then, the Hadoop cluster can serve as a Storm cluster.

