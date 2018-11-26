# Spark + Kafka {#concept_gh4_zfh_hfb .concept}

本文介绍如何在E-MapReduce的Hadoop集群运行Spark Streaming作业处理Kafka集群的数据。

## 编程参考 {#section_twr_bgh_hfb .section}

由于E-MapReduce上的Hadoop集群和Kafka集群都是基于纯开源版本软件，所以在编程使用上参考相应官方文档即可。

-   Spark官方文档：[streaming-kafka-integration](http://spark.apache.org/docs/latest/streaming-kafka-integration.html)
-   Spark官方文档：[structured-streaming-kafka-integration](http://spark.apache.org/docs/latest/structured-streaming-kafka-integration.html)
-   E-MapReduce-demo: [github地址](https://github.com/aliyun/aliyun-emapreduce-demo)

## 访问Kerberos Kafka集群 {#section_vwr_bgh_hfb .section}

E-MapReduce支持创建基于Kerberos认证的Kafka集群。当Hadoop集群作业需要访问Kerberos Kafka集群时，有两种使用方式：

-   非kerberos Hadoop集群：提供用于Kafka集群的Kerberos认证的kafka\_client\_jaas.conf文件。
-   kerberos Hadoop集群: 基于kerberos集群跨域互信，提供用于Hadoop集群的kerberos认证的kafka\_client\_jaas.conf文件。

以上两种方式都需要运行作业时提供kafka\_client\_jaas.conf文件用于kerberos认证。

kafka\_client\_jaas.conf文件格式：

```
KafkaClient {
    com.sun.security.auth.module.Krb5LoginModule required
    useKeyTab=true
    storeKey=true
    serviceName="kafka"
    keyTab="/path/to/kafka.keytab"
    principal="kafka/emr-header-1.cluster-12345@EMR.12345.COM";
};
```

如何获取keytab文件，请参考**用户指南-\>Kerberos认证**。

## Spark Streaming访问Kerberos Kafka集群 {#section_zwr_bgh_hfb .section}

当我们运行Spark Streaming作业访问kerberos kafka时，可以在spark-submit命令行参数中提供所需的kafka\_client\_jaas.conf和kafka.keytab文件。

```
spark-submit --conf spark.driver.extraJavaOptions=-Djava.security.auth.login.config={{PWD}}/kafka_client_jaas.conf --conf spark.executor.extraJavaOptions=-Djava.security.auth.login.config={{PWD}}/kafka_client_jaas.conf --files /local/path/to/kafka_client_jaas.conf,/local/path/to/kafka.keytab --class xx.xx.xx.KafkaSample --num-executors 2 --executor-cores 2 --executor-memory 1g --master yarn-cluster xxx.jar arg1 arg2 arg3
```

需要注意的是，kafka\_client\_jaas.conf文件中，keytab文件路径需要写相对路径，请严格按照如下keyTab配置项写法：

```
KafkaClient {
    com.sun.security.auth.module.Krb5LoginModule required
    useKeyTab=true
    storeKey=true
    serviceName="kafka"
    keyTab="kafka.keytab"
    principal="kafka/emr-header-1.cluster-12345@EMR.12345.COM";
};
```

