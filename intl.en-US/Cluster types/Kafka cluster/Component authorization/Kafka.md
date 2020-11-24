# Kafka

If Kerberos authentication or simple username-password cryptography is not enabled, users can use a forged identity to access cluster services. This is the case even if Kafka authorization is enabled. We recommend that you create a high-security Kafka cluster with Kerberos authentication enabled.

This topic describes only permission configurations for high-security Kafka clusters in E-MapReduce \(EMR\). In a high-security Kafka cluster, Kafka is started in Kerberos mode. For more information, see [Introduction to Kerberos](/intl.en-US/Cluster types/Hadoop cluster/Kerberos/Introduction to Kerberos.md).

## Go to the Configure tab for Kafka

1.  Log on to the [EMR console](https://emr.console.aliyun.com/).

2.  In the top navigation bar, select the region where your cluster residesand select a resource group based on your business requirements.

3.  Click the **Cluster Management** tab.

4.  On the **Cluster Management** page, find your cluster and click **Details** in the Actions column.

5.  In the left-side navigation pane, choose **Cluster Service** \> **Kafka**.

6.  Click the **Configure** tab.


## Configure parameters

1.  In the **Service Configuration** section, click the **server.properties** tab.

2.  Click **Custom Configuration** in the upper-right corner and configure the parameters listed in the following table.

    |key|value|Remarks|
    |---|-----|-------|
    |**authorizer.class.name**|**kafka.security.auth.SimpleAclAuthorizer**|N/A.|
    |**super.users**|**User:kafka**|User:kafka is required. If you want to add other users, separate them with semicolons \(;\).|

    **Note:** The zookeeper.set.acl parameter specifies whether Kafka has operation permissions on data in ZooKeeper. This is a built-in parameter and is set to true by default. You do not need to add this parameter in this step. If the zookeeper.set.acl parameter is set to true, only user kafka who has passed Kerberos authentication can run the kafka-topics.sh command. This command is used to read data from, write data to, and modify data in ZooKeeper.


## Restart Kafka

1.  In the upper-right corner of the **Kafka** page, choose **Actions** \> **Restart All Components**.

2.  In the Cluster Activities dialog box that appears, set related parameters and click **OK**.

    Click **History** in the upper-right corner to view the task progress.


## Authorization \(ACL\)

-   Basic concepts

    Definition of ACL in official Kafka documentation:

    ```
    Kafka acls are defined in the general format of "Principal P is [Allowed/Denied] Operation O From Host H On Resource R"
    ```

    The following concepts are involved in the definition: Principal, Allowed/Denied, Operation, Host, and Resource.

    -   Principal: the username.

        |Security protocol|Value|
        |-----------------|-----|
        |PLAINTEXT|ANONYMOUS|
        |SSL|ANONYMOUS|
        |SASL\_PLAINTEXT|If the PLAIN mechanism is used, Principal refers to the username specified in the client\_jaas.conf file. If the GSSAPI mechanism is used, Principal refers to the principal specified in the client\_jaas.conf file.|
        |SASL\_SSL|-|

    -   Allowed/Denied: specifies whether to allow or deny an operation.
    -   Operation: supported operations, including Read, Write, Create, DeleteAlter, Describe, ClusterAction, AlterConfigs, DescribeConfigs, IdempotentWrite, and All.
    -   Host: the machine.
    -   Resource: the resources on which permissions are granted. The resources include topics, groups, clusters, and transactional IDs.
    For the mappings between operations and resources, see [KIP-11 - Authorization Interface](https://cwiki.apache.org/confluence/display/KAFKA/KIP-11+-+Authorization+Interface).

-   Authorization commands

    The kafka-acls.sh script in the /usr/lib/kafka-current/bin/ directory is used to implement Kafka authorization. You can run the `kafka-acls.sh --help` command to learn how to use this script.


## Example

Perform the operations in this section on the master node of the high-security Kafka cluster.

1.  Run the following command to create user test:

    `useradd test`

2.  Create a topic.

    Only user kafka who has passed Kerberos authentication can run the kafka-topics.sh command, as described in the "Configure parameters" section.

    ```
    # The Kerberos authentication information of user kafka is configured in the kafka_client_jaas.conf file.
    export KAFKA_HEAP_OPTS="-Djava.security.auth.login.config=/etc/ecm/kafka-conf/kafka_client_jaas.conf" 
    # Replace the ZooKeeper information with the actual hostname of the Kafka cluster. You can run the hostname command to obtain the hostname.
    kafka-topics.sh --create --zookeeper emr-header-1:2181/kafka-1.0.0 --replication-factor 3 --partitions 1 --topic test
    ```

3.  Run the kafka-console-producer.sh command as user test.

    1.  Create a keytab file for user test to implement ZooKeeper and Kafka authentication.

        ```
        su root
        sh /usr/lib/has-current/bin/hadmin-local.sh /etc/ecm/has-conf -k /etc/ecm/has-conf/admin.keytab
        HadminLocalTool.local: # Press Enter to view a list of commands and the usage of each command.
        HadminLocalTool.local: addprinc # Press Enter to view the usage of the command.
        HadminLocalTool.local: addprinc -pw 123456 test # Add a principal named test and specify 123456 as the password.
        HadminLocalTool.local: ktadd -k /home/test/test.keytab test # Export the keytab file for later use.
        ```

    2.  Add the kafka\_client\_test.conf file.

        For example, place this file in the /home/test/ directory. The file contains the following content:

        ```
        KafkaClient {
        com.sun.security.auth.module.Krb5LoginModule required
        useKeyTab=true
        storeKey=true
        serviceName="kafka"
        keyTab="/home/test/test.keytab"
        principal="test";
        };
        // Zookeeper client authentication
        Client {
        com.sun.security.auth.module.Krb5LoginModule required
        useKeyTab=true
        useTicketCache=false
        serviceName="zookeeper"
        keyTab="/home/test/test.keytab"
        principal="test";
        };
        ```

    3.  Add the producer.conf file.

        For example, place this file in the `/home/test/` directory. The file contains the following content:

        ```
        security.protocol=SASL_PLAINTEXT
        sasl.mechanism=GSSAPI
        ```

    4.  Run the kafka-console-producer.sh command as user test.

        ```
        su test
        export KAFKA_HEAP_OPTS="-Djava.security.auth.login.config=/home/test/kafka_client_test.conf"
        kafka-console-producer.sh --producer.config /home/test/producer.conf --topic test --broker-list emr-worker-1:9092
        ```

        The following error is returned because no ACL is configured:

        ```
        org.apache.kafka.common.errors.TopicAuthorizationException: Not authorized to access topics: [test]
        ```

    5.  Configure an ACL.

        Run the `kafka-acls.sh` command as user kafka. Other users are not allowed to run this command.

        ```
        su kafka
        export KAFKA_HEAP_OPTS="-Djava.security.auth.login.config=/etc/ecm/kafka-conf/kafka_client_jaas.conf" 
        kafka-acls.sh --authorizer-properties zookeeper.connect=emr-header-1:2181/kafka-1.0.0 --add --allow-principal User:test --operation Write --topic test
        ```

    6.  Run the kafka-console-producer.sh command as user test.

        ```
        su test
        export KAFKA_HEAP_OPTS="-Djava.security.auth.login.config=/home/test/kafka_client_test.conf"
        kafka-console-producer.sh --producer.config /home/test/producer.conf --topic test --broker-list emr-worker-1:9092
        ```

        If the command succeeds, information similar to the following output is returned:

        ```
        [2018-02-28 22:25:36,178] INFO Kafka commitId : aaa7af6d4a11b29d (org.apache.kafka.common.utils.AppInfoParser)
        >alibaba
        >E-MapReduce
        >
        ```

4.  Run the `kafka-console-consumer.sh` command as user test.

    After you run the kafka-console-producer.sh command and add data to the topic, you can run the `kafka-console-consumer.sh` command to perform a consumption test.

    1.  Add the consumer.conf file.

        For example, place this file in the /home/test/ directory. The file contains the following content:

        ```
        security.protocol=SASL_PLAINTEXT
        sasl.mechanism=GSSAPI
        ```

    2.  Run the kafka-console-consumer.sh command as user test.

        ```
        su test
        # kafka_client_test.conf is used in the same way as producer.conf.
        export KAFKA_HEAP_OPTS="-Djava.security.auth.login.config=/home/test/kafka_client_test.conf"
        kafka-console-consumer.sh --consumer.config consumer.conf  --topic test --bootstrap-server emr-worker-1:9092 --group test-group  --from-beginning
        ```

        The following error is reported because no permissions are configured:

        ```
        org.apache.kafka.common.errors.GroupAuthorizationException: Not authorized to access group: test-group
        ```

    3.  Configure an ACL.

        ```
        su kafka
        export KAFKA_HEAP_OPTS="-Djava.security.auth.login.config=/etc/ecm/kafka-conf/kafka_client_jaas.conf" 
        # Permissions on test-group
        kafka-acls.sh --authorizer-properties zookeeper.connect=emr-header-1:2181/kafka-1.0.0 --add --allow-principal User:test --operation Read --group test-group
        # Permissions on topic
        kafka-acls.sh --authorizer-properties zookeeper.connect=emr-header-1:2181/kafka-1.0.0 --add --allow-principal User:test --operation Read --topic test
        ```

    4.  Run the `kafka-console-consumer.sh` command as user test again.

        ```
        su test
        # kafka_client_test.conf is used in the same way as producer.conf.
        export KAFKA_HEAP_OPTS="-Djava.security.auth.login.config=/home/test/kafka_client_test.conf"
        kafka-console-consumer.sh --consumer.config consumer.conf  --topic test --bootstrap-server emr-worker-1:9092 --group test-group  --from-beginning
        ```

        The following normal output is returned:

        ```
        alibaba
        E-MapReduce
        ```


