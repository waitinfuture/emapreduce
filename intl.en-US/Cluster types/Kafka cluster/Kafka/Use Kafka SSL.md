# Use Kafka SSL

This topic describes how to use the SSL feature of an EMR Kafka cluster.

An EMR Kafka cluster is created. For more information, see [Create a cluster](/intl.en-US/Cluster Management/Configure clusters/Create a cluster.md).

## Enable SSL

SSL is disabled for Kafka clusters by default. You can perform the following steps to enable SSL:

1.  Log on to the [EMR console](https://emr.console.aliyun.com/).

2.  In the top navigation bar, select the region where your cluster resides. Select the resource group as required. By default, all resources of the account appear.

3.  Click the **Cluster Management** tab.

4.  On the **Cluster Management** page that appears, find the target cluster and click **Details** in the Actions column.

5.  In the left-side navigation pane, click **Cluster Service** and then **Kafka**.

6.  Click the **Configure** tab.

    1.  In the Service Configuration section, set **kafka.ssl.enable** to **true**.

    2.  Click **Save**.

    3.  In the **Confirm Changes** dialog box, specify **Description**, turn on the **Auto-update Configuration** switch, and then click **OK**.

7.  Restart the Kafka service.

    1.  In the upper-right corner of the page, select **Restart All Components** from the **Actions** drop-down list.

    2.  In the **Cluster Activities** dialog box, specify **Description** and click **OK**.


## Access Kafka from an EMR client

To access Kafka over SSL, you must configure security.protocol, truststore, and keystore. For example, if you run jobs in a non-security Kafka cluster, configure the following information:

```
security.protocol=SSL
ssl.truststore.location=/etc/ecm/kafka-conf/truststore
ssl.truststore.password=${password}
ssl.keystore.location=/etc/ecm/kafka-conf/keystore
ssl.keystore.password=${password}
```

If you run jobs outside your Kafka cluster, copy the truststore and keystore files in the /etc/ecm/kafka-conf/ directory for a node of the Kafka cluster to the runtime environment. Then, configure the required information.

The following example shows how to use the Producer and Consumer programs of Kafka to run jobs in a Kafka cluster.

1.  Create a configuration file named ssl.properties and add the following configuration items:

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

3.  Use the SSL configuration file to generate data.

    ```
    kafka-producer-perf-test.sh --topic test --num-records 123456 --throughput 10000 --record-size 1024 --producer-props bootstrap.servers=emr-worker-1:9092 --producer.config ssl.properties
    ```

4.  Use the SSL configuration file to consume data.

    ```
    kafka-consumer-perf-test.sh --broker-list emr-worker-1:9092 --messages 100000000 --topic test --consumer.config ssl.properties
    ```


