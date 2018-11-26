# Kafka SSL {#concept_t33_3gx_y2b .concept}

E-MapReduce Kafka supports the SSL function in EMR-3.12.0 and later versions.

## Create a cluster {#section_akj_trc_z2b .section}

For details about how to create a cluster, see [Create a cluster](intl.en-US/User Guide/Cluster/Create a cluster.md#)Create a cluster.

## Enable the SSL service {#section_bkj_trc_z2b .section}

The SSL function is not enabled for the Kafka cluster by default. You can enable SSL on the configuration page of the Kafka service.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17900/154148571210846_en-US.png)

As shown in the preceding figure, change kafka.ssl.enable to true and then restart the component.

## Access Kafka from the client {#section_bh5_vrc_z2b .section}

You need to configure security.protocol, truststore, and keystore when you access Kafka through SSL. Use a standard mode cluster as an example. To run a job in a Kafka cluster, you can configure the cluster as follows:

```
security.protocol=SSL
ssl.truststore.location=/etc/ecm/kafka-conf/truststore
ssl.truststore.password=${password}
ssl.keystore.location=/etc/ecm/kafka-conf/keystore
ssl.keystore.password=${password}
```

If you are running a job in an environment other than a Kafka cluster, copy the truststore and keystore files \(in the /etc/ecm/kafka-conf/ directory on any node of the cluster\) in the Kafka cluster to the running environment and add configurations accordingly.

Use the producer and consumer programs in the Kafka as an example.

1.  Create the configuration file ssl.properties, and add configuration items.

    ```
    security.protocol=SSL
     ssl.truststore.location=/etc/ecm/kafka-conf/truststore
     ssl.truststore.password=${password}
     ssl.keystore.location=/etc/ecm/kafka-conf/keystore
     ssl.keystore.password=${password}
    ```

2.  Create a topic.

    ```
    kafka-topics.sh --zookeeper emr-header-1:2181/kafka-1.0.1 --replication-factor 2 --
    partitions 100 --topic test --create
    ```

3.  Use an SSL configuration file to generate data.

    ```
    kafka-producer-perf-test.sh --topic test --num-records 123456 --throughput 10000 --record-size 1024 --producer-props bootstrap.servers=emr-worker-1:9092 --producer.config ssl.properties
    ```

4.  Use an SSL configuration file to consume data.

    ```
    kafka-consumer-perf-test.sh --broker-list emr-worker-1:9092 --messages 100000000 --topic test --consumer.config ssl.properties
    ```


