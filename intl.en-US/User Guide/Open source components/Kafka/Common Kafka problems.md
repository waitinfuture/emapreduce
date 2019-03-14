# Common Kafka problems {#concept_u2m_rgd_z2b .concept}

This section describes two common issues with Kafka.

-   `Error while executing topic command : Replication factor: 1 larger than available brokers: 0.`

    Common causes:

    -   A fault occurs in the Kafka service and the cluster broker process exits. You need to use logs to troubleshoot the fault.
    -   The ZooKeeper address of the Kafka service is incorrect. View and use the Zookeeper.connect configuration item on the Kafka configuration management page.
-   `java.net.BindException: Address already in use (Bind failed)`

    You may encounter this exception when you use Kafka command line tools. This is typically caused by the unavailability of the JMX port. You can specify a JMX port manually before using the command line. For example:

    ```
    JMX_PORT=10101 kafka-topics --zookeeper emr-header-1:2181/kafka-1.0.0 --list
    ```


