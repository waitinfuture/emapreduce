# Kafka SSL使用说明 {#concept_t33_3gx_y2b .concept}

E-MapReduce Kafka 从 EMR-3.12.0 版本开始，开始支持 SSL 功能。

## 创建集群 {#section_akj_trc_z2b .section}

具体的创建集群操作，请参见[创建集群](../../../../intl.zh-CN/集群规划与配置/集群配置/创建集群.md#)。

## 开启 SSL 服务 {#section_bkj_trc_z2b .section}

Kafka 集群默认没有开启 SSL 功能，您可以在 Kafka 服务的配置页面开启 SSL。

![开启SSL](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17900/155963097010846_zh-CN.png)

如图，修改配置项 kafka.ssl.enable 为 true，部署配置并重启组件。

## 客户端访问 Kafka {#section_bh5_vrc_z2b .section}

客户端通过 SSL 访问 Kafka 时需要设置 security.protocol、truststore 和 keystore 的相关配置。以非安全集群为例，如果是在 Kafka 集群运行作业，可以配置如下：

```
security.protocol=SSL
ssl.truststore.location=/etc/ecm/kafka-conf/truststore
ssl.truststore.password=${password}
ssl.keystore.location=/etc/ecm/kafka-conf/keystore
ssl.keystore.password=${password}
```

如果是在 Kafka 集群以外的环境运行作业，可将 Kafka 集群中的 truststore 和 keystore 文件（位于集群任意一个节点的 /etc/ecm/kafka-conf/目录中）拷贝至运行环境作相应配置。

以 Kafka 自带的 producer 和 consumer 程序，在 Kafka 集群运行为例：

1.  创建配置文件 ssl.properties， 添加配置项。

    ```
    security.protocol=SSL
     ssl.truststore.location=/etc/ecm/kafka-conf/truststore
     ssl.truststore.password=${password}
     ssl.keystore.location=/etc/ecm/kafka-conf/keystore
     ssl.keystore.password=${password}
    ```

2.  创建 topic。

    ```
    kafka-topics.sh --zookeeper emr-header-1:2181/kafka-1.0.1 --replication-factor 2 --
    partitions 100 --topic test --create
    ```

3.  使用 SSL 配置文件产生数据。

    ```
    kafka-producer-perf-test.sh --topic test --num-records 123456 --throughput 10000 --record-size 1024 --producer-props bootstrap.servers=emr-worker-1:9092 --producer.config ssl.properties
    ```

4.  使用 SSL 配置文件消费数据。

    ```
    kafka-consumer-perf-test.sh --broker-list emr-worker-1:9092 --messages 100000000 --topic test --consumer.config ssl.properties
    ```


