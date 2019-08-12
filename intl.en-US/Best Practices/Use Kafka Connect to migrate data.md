# Use Kafka Connect to migrate data {#task_1443351 .task}

During streaming data processing, data synchronization between Kafka and other systems or data migration between Kafka clusters is often required. This topic describes how to use Kafka Connect to migrate data between Kafka clusters.

-   You have registered an Alibaba Cloud account. For more information, see [Create an Alibaba Cloud account](https://partners-intl.aliyun.com/help/doc-detail/50482.htm).
-   You have activated E-MapReduce.
-   You have authorized the Alibaba Cloud account. For more information, see [Role authorization](../../../../reseller.en-US/Cluster Planning and Configurations/Cluster planning/Role authorization.md#).

Kafka Connect is a scalable and reliable tool for fast transmitting streaming data between Kafka and other systems. For example, you can use Kafka Connect to obtain binlog data from a database and migrate the data of the database to a Kafka cluster. In this way, you can migrate the data of the database and indirectly connect the database to a downstream streaming data processing system. Kafka Connect also provides a Representational State Transfer \(REST\) application programming interface \(API\) to help you create and manage Kafka Connect connectors.

Kafka Connect can run in standalone or distributed mode. In standalone mode, all workers run in the same process. Compared with the standalone mode, the distributed mode is more scalable and fault-tolerant. It is the most commonly used mode and the recommended mode for the production environment.

This topic describes how to call the REST API of Kafka Connect to migrate data between Kafka clusters, where Kafka Connect runs in distributed mode.

## Step 1: Create Kafka clusters {#section_jky_y72_92b .section}

Create a source Kafka cluster and a target Kafka cluster in E-MapReduce. Kafka Connect is installed on the task node. Therefore, a task node must be created in the target Kafka cluster. Kafka Connect is started on the task node by default after the cluster is created. The port number is 8083.

We recommend that you add the source Kafka cluster and the target Kafka cluster to the same security group. If the source Kafka cluster and the target Kafka cluster belong to different security groups, the two clusters are not accessible to each other by default. You must modify the required settings of the security groups to allow mutual access.

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://partners-intl.console.aliyun.com/#/emr).
2.  Create the source Kafka cluster and the target Kafka cluster. For more information, see [Create a cluster](../../../../reseller.en-US/Cluster Planning and Configurations/Configure clusters/Create a cluster.md#). 

    **Note:** When creating the target Kafka cluster, you must configure a **task instance**, that is, a task node.

    ![Create a Kafka cluster](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1068351/156560098352756_en-US.png)


## Step 2: Create a topic for storing the data to be migrated {#section_qgf_nnt_g9d .section}

Create a topic named **connect** in the source Kafka cluster.

1.  Use Secure Shell \(SSH\) to log on to the header node of the source Kafka cluster. In this example, the header node is emr-header-1.
2.  Run the following command as the root user to create a topic named **connect**: 

    ``` {#codeblock_mk0_gig_nfk}
    kafka-topics.sh --create --zookeeper emr-header-1:2181 --replication-factor 2 --partitions 10 --topic connect
    ```

    ![Create a topic](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1148224/156560098453860_en-US.png)

    **Note:** After performing the preceding operations, keep the logon window for later use.


## Step 3: Create a Kafka Connect connector {#section_2ks_6mz_rym .section}

On the task node of the target Kafka cluster, run the curl command to create a Kafka Connect connector by using JavaScript Object Notation \(JSON\) data.

1.  Use SSH to log on to the task node of the target Kafka cluster. In this example, the task node is **emr-worker-3**.
2.  Customize Kafka Connect configuration. 

    Go to the Configuration page of the **Kafka** service under the target Kafka cluster. Customize the **offset.storage.topic**, **config.storage.topic**, and **status.storage.topic** parameters in **connect-distributed.properties**. For more information, see [Configure parameters](../../../../reseller.en-US/Cluster Planning and Configurations/Third-party software/Configure parameters.md#).

    Kafka Connect saves the offsets, configurations, and task status in the topics specified by the **offset.storage.topic**, **config.storage.topic**, and **status.storage.topic** parameters, respectively. Kafka Connect automatically creates these topics by using the default partition and replication factor that are saved in /etc/ecm/kafka-conf/connect-distributed.properties.

3.  Run the following command as the root user to create a Kafka Connect connector: 

    ``` {#codeblock_pzk_uyf_q97}
    curl -X POST -H "Content-Type: application/json" --data '{"name": "connect-test", "config": { "connector.class": "EMRReplicatorSourceConnector", "key.converter": "org.apache.kafka.connect.converters.ByteArrayConverter", "value.converter": "org.apache.kafka.connect.converters.ByteArrayConverter", "src.kafka.bootstrap.servers": "${src-kafka-ip}:9092", "src.zookeeper.connect": "${src-kafka-curator-ip}:2181", "dest.zookeeper.connect": "${dest-kafka-curator-ip}:2181", "topic.whitelist": "${source-topic}", "topic.rename.format": "${dest-topic}", "src.kafka.max.poll.records": "300" } }' http://emr-worker-3:8083/connectors
    ```

    In the JSON data, the name field indicates the name of the Kafka Connect connector to create, which is connect-test in this example. The config field needs to be configured based on your actual requirements. The following table describes the key variables of the config field.

    |Variable|Description|
    |--------|-----------|
    |**$\{source-topic\}**|The topics for storing the data to be migrated in the source Kafka cluster. For example, **connect**. Separate multiple topics with commas \(,\).|
    |**$\{dest-topic\}**|The topics to which the data is migrated in the target Kafka cluster. For example, **connect.replica**.|
    |**$\{src-kafka-curator-hostname\}**|The internal IP address of the node where the ZooKeeper service is installed in the source Kafka cluster.|
    |**$\{dest-kafka-curator-hostname\}**|The internal IP address of the node where the ZooKeeper service is installed in the target Kafka cluster.|

    **Note:** After performing the preceding operations, keep the logon window for later use.


## Step 4: View the status of the Kafka Connect connector and task node {#section_lo0_pff_1qq .section}

View the status of the Kafka Connect connector and task node and make sure that they are in normal status.

1.  Return to the logon window on the task node of the target Kafka cluster. In the example, the task node is **emr-worker-3**.
2.  Run the following command as the root user to view all Kafka Connect connectors: 

    ``` {#codeblock_vjq_att_cp8}
    curl emr-worker-3:8083/connectors
    ```

    ![View all Kafka Connect connectors](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1148224/156560098453871_en-US.png)

3.  Run the following command as the root user to view the status of the Kafka Connect connector created in this example, that is, **connect-test**: 

    ``` {#codeblock_gb3_i3t_cmf}
    curl emr-worker-3:8083/connectors/connect-test/status
    ```

    ![View the status of connect-test](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1148224/156560098453874_en-US.png)

    Make sure that the Kafka Connect connector \(**connect-test** in this example\) is in the **RUNNING** status.

4.  Run the following command as the root user to view the details of the task node: 

    ``` {#codeblock_zst_gq6_q73}
    curl emr-worker-3:8083/connectors/connect-test/tasks
    ```

    ![View task node details](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1148224/156560098553876_en-US.png)

    Make sure that no error message about the task node is returned.


## Step 5: Generate the data to be migrated {#section_hz9_ok8_10n .section}

Send the data to be migrated to the **connect** topic in the source Kafka cluster.

1.  Return to the logon window on the header node of the source Kafka cluster. In this example, the header node is emr-header-1.
2.  Run the following command as the root user to send data to the **connect** topic: 

    ``` {#codeblock_uph_1g2_7zf}
    kafka-producer-perf-test.sh --topic connect --num-records 100000 --throughput 5000 --record-size 1000 --producer-props bootstrap.servers=emr-header-1:9092
    ```

    ![Generate the data to be migrated](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1148224/156560098553877_en-US.png)


## Step 6: View the result of data migration {#section_4ot_7td_mgr .section}

After the data to be migrated is generated, Kafka Connect automatically migrates the data to the corresponding topic in the target Kafka cluster. In this example, the topic is **connect.replica**.

1.  Return to the logon window on the task node of the target Kafka cluster. In this example, the task node is **emr-worker-3**.
2.  Run the following command as the root user to check whether the data is migrated: 

    ``` {#codeblock_q9p_czo_yxi}
    kafka-consumer-perf-test.sh --topic connect.replica --broker-list emr-header-1:9092 --messages 100000
    ```

    ![View the result of data migration](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1148224/156560098553881_en-US.png)

    According to the command output in the preceding figure, the 100,000 messages sent to the source Kafka cluster are migrated to the target Kafka cluster.


## Summary {#section_taj_fni_lf8 .section}

This topic describes and demonstrates how to use Kafka Connect to migrate data between Kafka clusters. For more information about how to use Kafka Connect, see [Kafka official website](https://kafka.apache.org/documentation/#connect) and [REST API](https://docs.confluent.io/current/connect/references/restapi.html).

