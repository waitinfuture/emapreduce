# Kafka 常见问题 {#concept_u2m_rgd_z2b .concept}

本文介绍 Kafka 常见问题的一些问题以及解决方法。

-   `Error while executing topic command : Replication factor: 1 larger than available brokers: 0.`

    常见原因：

    -   Kafka 服务异常，集群 Broker 进程都退出了，需要结合日志排查问题。
    -   Kafka 服务的 Zookeeper 地址使用错误，请注意查看并使用集群配置管理中 Kafka 组件的 Zookeeper 连接地址。
-   `java.net.BindException: Address already in use (Bind failed)`

    当您使用 Kafka 命令行工具时，有时会碰到 `java.net.BindException: Address already in use (Bind failed)` 异常，这一般是由 JMX 端口被占用导致，您可以在命令行前手动指定一个 JMX 端口即可。例如：

    ```
    JMX_PORT=10101 kafka-topics --zookeeper emr-header-1:2181/kafka-1.0.0 --list
    ```


