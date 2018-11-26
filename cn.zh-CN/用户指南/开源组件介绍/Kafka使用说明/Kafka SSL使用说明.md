# Kafka SSL使用说明 {#concept_t33_3gx_y2b .concept}

从 EMR-3.12.0 版本开始，E-MapReduce Kafka支持SSL功能。

## 创建集群 {#section_akj_trc_z2b .section}

具体的创建集群操作，请参见[创建集群](intl.zh-CN/用户指南/集群/创建集群.md#)。

## 开启SSL服务 {#section_bkj_trc_z2b .section}

默认Kafka集群没有开启SSL功能，您可以在Kafka服务的配置页面开启SSL。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17900/153829571710846_zh-CN.png)

如图，修改配置项kafka.ssl.enable为true，部署配置并重启组件。

## 客户端访问Kafka {#section_bh5_vrc_z2b .section}

客户端通过SSL访问Kafka时需要设置 security.protocol、truststore 和 keystore 的相关配置。以非安全集群为例，如果是在kafka集群运行作业，可以配置如下：

```
security.protocol=SSL
ssl.truststore.location=/etc/ecm/kafka-conf/truststore
ssl.truststore.password=${password}
ssl.keystore.location=/etc/ecm/kafka-conf/keystore
ssl.keystore.password=${password}
```

如果是在Kafka集群以外的环境运行作业，可将 Kafka 集群中的 truststore 和 keystore 文件（位于集群任意一个节点的/etc/ecm/kafka-conf/目录中）拷贝至运行环境作相应配置。

以Kafka自带的producer和consumer程序，在 Kafka 集群运行为例：

1.  创建配置文件 ssl.properties， 添加配置项。

    ```
    security.protocol=SSL
     ssl.truststore.location=/etc/ecm/kafka-conf/truststore
     ssl.truststore.password=${password}
     ssl.keystore.location=/etc/ecm/kafka-conf/keystore
     ssl.keystore.password=${password}
    ```

2.  创建topic。

    ```
    kafka-topics.sh --zookeeper emr-header-1:2181/kafka-1.0.1 --replication-factor 2 --
    partitions 100 --topic test --create
    ```

3.  使用SSL配置文件产生数据。

    ```
    kafka-producer-perf-test.sh --topic test --num-records 123456 --throughput 10000 --record-size 1024 --producer-props bootstrap.servers=emr-worker-1:9092 --producer.config ssl.properties
    ```

4.  使用SSL配置文件消费数据。

    ```
    kafka-consumer-perf-test.sh --broker-list emr-worker-1:9092 --messages 100000000 --topic test --consumer.config ssl.properties
    ```


