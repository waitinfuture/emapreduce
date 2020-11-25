# Use Kafka Connect to migrate data

When streaming data is processed, data synchronization between Kafka and other systems or data migration between Kafka clusters is often required. This topic describes how to use Kafka Connect in E-MapReduce \(EMR\) to migrate data between Kafka clusters.

-   An Alibaba Cloud account is created.
-   EMR is activated.
-   The Alibaba Cloud account is authorized. For more information, see [Authorize roles](/intl.en-US/Cluster Management/Cluster planning/Authorize roles.md).

Kafka Connect is a scalable and reliable tool used to synchronize data between Kafka and other systems and to transmit streaming data between Kafka clusters. For example, you can use Kafka Connect to obtain binlog data from a database and migrate the data of the database to a Kafka cluster. This also indirectly connects the database to the downstream streaming data processing system of the Kafka cluster. Kafka Connect also provides a RESTful API to help you create and manage Kafka Connect connectors.

Kafka Connect can run in standalone or distributed mode. In standalone mode, all workers run in the same process. The distributed mode is more scalable and fault-tolerant than the standalone mode. The distributed mode is the most commonly used mode and the recommended mode for the production environment.

This topic describes how to call the RESTful API of Kafka Connect to migrate data between Kafka clusters, where Kafka Connect runs in distributed mode.

## Step 1: Create Kafka clusters

Create a source Kafka cluster and a destination Kafka cluster in the EMR console. Kafka Connect is installed on a task node. To install Kafka Connect, you must create a task node in the destination Kafka cluster. Kafka Connect is started on the task node after the destination cluster is created. The port number is 8083.

1.  Log on to the [EMR console](https://emr.console.aliyun.com/).

2.  Create a source Kafka cluster and a destination Kafka cluster. For more information, see [Create a cluster](/intl.en-US/Cluster Management/Configure clusters/Create a cluster.md).

    **Note:**

    -   We recommend that you add the source and destination Kafka clusters to the same security group.

        If the two clusters belong to different security groups, they are not accessible to each other. You must modify the settings of the security groups to allow mutual access between the two clusters.

    -   After the destination Kakfa cluster is created, you must add a task node.
    ![Create a Kafka cluster](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4307136061/p52756.png)


## Step 2: Create a topic used to store the data you want to migrate

Create a topic named **connect** in the source Kafka cluster.

1.  Log on to the master node of the source Kafka cluster by using SSH. In this example, the master node is **emr-header-1**.

2.  Run the following command as the root user to create a topic named **connect**:

    ```
    kafka-topics.sh --create --zookeeper emr-header-1:2181 --replication-factor 2 --partitions 10 --topic connect
    ```

    ![Create a topic](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9627457851/p53860.png)

    **Note:** After you complete the preceding operations, do not close the logon window. The logon window is required in a later step.


## Step 3: Create a Kafka Connect connector

On the task node of the destination Kafka cluster, run the `curl` command to create a Kafka Connect connector by using JSON data.

1.  Customize Kafka Connect configurations.

    Go to the **Configure** tab of the **Kafka** service under the destination Kafka cluster in the [Alibaba Cloud EMR console](https://emr.console.aliyun.com/). Specify the **offset.storage.topic**, **config.storage.topic**, and **status.storage.topic** parameters on the **connect-distributed.properties** subtab. For more information, see [Configure parameters](/intl.en-US/Cluster Management/Configure clusters/Configure parameters.md).

    Kafka Connect saves the offsets, configurations, and task status in the topics specified by the **offset.storage.topic**, **config.storage.topic**, and **status.storage.topic** parameters, respectively. Kafka Connect automatically creates these topics by using the default settings of partitions and replication-factor that are saved in /etc/ecm/kafka-conf/connect-distributed.properties.

2.  Log on to the master node of the destination Kafka cluster. In this example, the master node is **emr-header-1**.

3.  Switch to the task node named **emr-worker-3** in this example.

    ![task](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4839794061/p167634.png)

4.  Run the following command as the root user to create a Kafka Connect connector:

    ```
    curl -X POST -H "Content-Type: application/json" --data '{"name": "connect-test", "config": { "connector.class": "EMRReplicatorSourceConnector", "key.converter": "org.apache.kafka.connect.converters.ByteArrayConverter", "value.converter": "org.apache.kafka.connect.converters.ByteArrayConverter", "src.kafka.bootstrap.servers": "${src-kafka-ip}:9092", "src.zookeeper.connect": "${src-kafka-curator-ip}:2181", "dest.zookeeper.connect": "${dest-kafka-curator-ip}:2181", "topic.whitelist": "${source-topic}", "topic.rename.format": "${dest-topic}", "src.kafka.max.poll.records": "300" } }' http://emr-worker-3:8083/connectors
    ```

    In the JSON data, the name field specifies the name of the Kafka Connect connector that you want to create. In this example, the name is connect-test. Configure the config field based on your needs. The following table describes the key variables of the config field.

    |Variable|Description|
    |--------|-----------|
    |**$\{source-topic\}**|The topics that store the data to be migrated in the source Kafka cluster, for example, **connect**. Separate multiple topics with commas \(,\).|
    |**$\{dest-topic\}**|The topics to which data is migrated in the destination Kafka cluster, for example, **connect.replica**.|
    |**$\{src-kafka-curator-hostname\}**|The internal IP address of the node where the ZooKeeper service is deployed in the source Kafka cluster.|
    |**$\{dest-kafka-curator-hostname\}**|The internal IP address of the node where the ZooKeeper service is deployed in the destination Kafka cluster.|

    **Note:** After you complete the preceding operations, do not close the logon window. The logon window is required in a later step.


## Step 4: View the status of the Kafka Connect connector and task node

View the status of the Kafka Connect connector and task node and make sure that they are normal.

1.  Return to the logon window of the task node of the destination Kafka cluster. In this example, the task node is **emr-worker-3**.

2.  Run the following command as the root user to view all Kafka Connect connectors:

    ```
    curl emr-worker-3:8083/connectors
    ```

    ![View all Kafka Connect connectors](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9627457851/p53871.png)

3.  Run the following command as the root user to view the status of the Kafka Connect connector created in this example, that is, **connect-test**:

    ```
    curl emr-worker-3:8083/connectors/connect-test/status
    ```

    ![View the status of connect-test](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9627457851/p53874.png)

    Make sure that the Kafka Connect connector, **connect-test** in this example, is in the **RUNNING** state.

4.  Run the following command as the root user to view the details of the task node:

    ```
    curl emr-worker-3:8083/connectors/connect-test/tasks
    ```

    ![View the details of the task node](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9627457851/p53876.png)

    Make sure that no error message about the task node is returned.


## Step 5: Generate the data to be migrated

Send the data to be migrated to the **connect** topic in the source Kafka cluster.

1.  Return to the logon window of the master node of the source Kafka cluster. In this example, the master node is emr-header-1.

2.  Run the following command as the root user to send data to the **connect** topic:

    ```
    kafka-producer-perf-test.sh --topic connect --num-records 100000 --throughput 5000 --record-size 1000 --producer-props bootstrap.servers=emr-header-1:9092
    ```

    When the information shown in the following figure appears, the data to be migrated is generated.

    ![Generate the data to be migrated](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3597593061/p53877.png)


## Step 6: View the data migration results

After the data to be migrated is generated, Kafka Connect automatically migrates the data to the corresponding topic in the destination Kafka cluster. In this example, the topic is **connect.replica**.

1.  Return to the logon window of the task node of the destination Kafka cluster. In this example, the task node is **emr-worker-3**.

2.  Run the following command as the root user to check whether the data is migrated:

    ```
    kafka-consumer-perf-test.sh --topic connect.replica --broker-list emr-header-1:9092 --messages 100000
    ```

    ![View the data migration results](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0727457851/p53881.png)

    Based on the command output in the preceding figure, the 100,000 messages sent to the source Kafka cluster are migrated to the destination Kafka cluster.


## Summary

This topic describes how to use Kafka Connect to migrate data between Kafka clusters. For more information about how to use Kafka Connect, visit the [Kafka official website](https://kafka.apache.org/documentation/#connect) and see [RESTful API](https://docs.confluent.io/current/connect/references/restapi.html).

