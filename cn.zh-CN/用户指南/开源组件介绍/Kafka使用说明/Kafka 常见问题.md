# Kafka 常见问题 {#concept_u2m_rgd_z2b .concept}

-   `Error while executing topic command : Replication factor: 1 larger than available brokers: 0.`

    常见原因：

    -   Kafka服务异常，集群Broker进程都退出了，需要结合日志排查问题。
    -   Kafka服务的Zookeeper地址使用错误，请注意查看并使用集群配置管理中Kafka组件的Zookeeper连接地址。
-   `java.net.BindException: Address already in use (Bind failed)`

    当您使用kafka命令行工具时，有时会碰到 `java.net.BindException: Address already in use (Bind failed)` 异常，这一般由JMX端口被占用导致，您可以在命令行前手动指定一个JMX端口即可。例如：

    ```
    JMX_PORT=10101 kafka-topics --zookeeper emr-header-1:2181/kafka-1.0.0 --list
    ```


