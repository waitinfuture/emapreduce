# Use E-MapReduce to collect metrics from a Kafka client {#concept_ach_3gj_gfb .concept}

This section describes how to use E-MapReduce to collect metrics from a Kafka client to conduct effective performance monitoring.

## Background {#section_knb_vgj_gfb .section}

Kafka provides a collection of metrics that are used to measure the performance of Broker, Consumer, Producer, Stream, and Connect. E-MapReduce collects metrics for Kafka Broker by using Ganglia to monitor the running status of this Kafka Broker. A Kafka system consists of two roles: a Kafka Broker and multiple Kafka clients. When an issue of read/write performance occurs, you must perform an analysis on the both Kafka Broker and clients. Metrics from Kafka clients are important for performing the analysis.

## Principle {#section_nqd_xgj_gfb .section}

-   Collect Metrics for Kafka performance

    Kafka supports multiple external Metrics Reporters. JMX Reporter is built in to Kafka by default. You can use the JMX tool to view metrics of Kafka. You can implement your own Metrics Reporter such as org.apache.kafka.common.metrics.MetricsReporter to collect custom metrics.

-   Store Metrics

    You can customize Kafka metrics. In addition, you need a data store to keep these metrics for later use and analysis. You can store metrics to Kafka without using a third-party data store as Kafka itself is a data store. In addition, Kafka can be easily integrated with other services. You can collect metrics from a client as the following figure shows:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21764/154824025412649_en-US.png)

    E-MapReduce provides a sample emr-kafka-client-metrics. You can download the source code from the link: [source code](https://github.com/aliyun/aliyun-emapreduce-sdk/tree/master-2.x/external/emr-kafka).


## Prerequisites {#section_f4s_yzr_ngb .section}

-   Restrictions
    -   Support for only Java applications;
    -   Support for only clients of Kafka 0.10 or later;
-   Without compiling code by yourself, E-MapReduce has published the jar package in Maven. You can download the latest version from the [download link](http://mvnrepository.com/artifact/com.aliyun.emr/emr-kafka-client-metrics?spm=a2c4e.11153940.blogcont624050.20.24d04bcauktP9S).

-   In this section, we use E-MapReduce to automatically create a Kafka cluster. For more information, see [Create a cluster](../DNemapreduce1883011/EN-US_TP_17840.dita#concept_nrp_154_y2b).

    We use the following versions of E-MapReduce and Kafka:

    -   EMR Version: EMR-3.12.1
    -   Cluster Type: Kafka
    -   Software: Kafka-Manager \(1.3.3.16\), Kafka \(2.11-1.0.1\), ZooKeeper \(3.4.12\), and Ganglia \(3.7.2\)
    -   The network type of this Kafka cluster is VPC in the China \(Hangzhou\) region. The master instance group is configured with a public IP and an internal network IP. The following figure shows the details.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21764/154824025412651_en-US.png)

-   Configure metrics

    |Metric|Description|
    |------|-----------|
    |metric.reporters|The sample Metrics Reporter: org.apache.kafka.clients.reporter.EMRClientMetricsReporter|
    |emr.metrics.reporter.bootstrap.servers|The metrics that stores bootstrap.servers of a Kafka cluster.|
    |emr.metrics.reporter.zookeeper.connect|The metrics that stores Zookeeper addresses of a Kafka cluster.|

-   Load metrics
    -   Place the emr-kafka-client-metrics jar package on a client. Add the path of the jar package to the classpath of a client-side application.
    -   Install the emr-kafka-client-metrics dependency on the jar package of a client-side application.

## Procedures {#section_q2p_shj_gfb .section}

1.  Download the latest emr-kafka-client-metrics package.

    ```
    wget http://central.maven.org/maven2/com/aliyun/emr/emr-kafka-client-metrics/1.4.3/emr-kafka-client-metrics-1.4.3.jar
    ```

2.  Create a test topic.

    ```
    kafka-topics.sh --zookeeper emr-header-1:2181/kafka-1.0.1 --partitions 10 --replication-factor 2 --topic test-metrics  --create
    ```

3.  Copy theemr-kafka-client-metrics package to the lib directory of a Kafka client.

    ```
    cp emr-kafka-client-metrics-1.4.3.jar /usr/lib/kafka-current/libs/
    ```

4.  Write data to a test topic. You can write the configurations of a Kafka Producer to the local client.conf file.

    ```
    ## client.conf:
    metric.reporters=org.apache.kafka.clients.reporter.EMRClientMetricsReporter
    emr.metrics.reporter.bootstrap.servers=emr-worker-1:9092
    emr.metrics.reporter.zookeeper.connect=emr-header-1:2181/kafka-1.0.1
    bootstrap.servers=emr-worker-1:9092
    ## Commnad:
    kafka-producer-perf-test.sh --topic test-metrics --throughput 1000 --num-records 100000 
    --record-size 1024 --producer.config client.conf
    ```

5.  View the current metrics from a client. The default metrics topic is \_emr-client-metrics.

    ```
    Kafka-console-consumer.sh -- Topic _ emr-client-metrics -- Bootstrap-server emr-worker-1: 9092 
    --from-beginning
    ```

    The returned message is shown as follows.

    ```
    {prefix=kafka.producer, client.ip=192.168.xxx.xxx, client.process=25536@emr-header-1.cluster-xxxx, 
    attribute=request-rate, value=894.4685104965012, timestamp=1533805225045, group=producer-metrics, 
    tag.client-id=producer-1}
    ```


**Note:** 

|Field name|Description:|
|----------|------------|
|client.ip|The IP address of a client host.|
|client.process|The process ID of a client-side application.|
|attribute|The attribute name of a metric.|
|value|The value of a metric.|
|timestamp|The timestamp when you collect a metric.|
|tag.xxx|Other tag information of a metric.|

