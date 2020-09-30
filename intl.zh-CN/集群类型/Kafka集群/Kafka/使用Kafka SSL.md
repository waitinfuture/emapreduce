# 使用Kafka SSL

本文介绍如何使用E-MapReduce Kafka的SSL功能。

已创建Kafka类型的集群，创建详情请参见[创建集群](/intl.zh-CN/集群管理/集群配置/创建集群.md)。

## 开启SSL服务

Kafka集群SSL功能默认关闭，您可以执行以下步骤开启SSL功能。

1.  登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com/)。

2.  在顶部菜单栏处，根据实际情况选择地域（Region）和资源组。

3.  单击上方的**集群管理**页签。

4.  在**集群管理**页面，单击相应集群所在行的**详情**。

5.  在左侧导航栏单击**集群服务** \> **Kafka**。

6.  单击**配置**页签，修改服务配置。

    1.  将**kafka.ssl.enable**修改为**true**。

    2.  单击**保存**。

    3.  在**确认修改**页面，输入**执行原因**，开启**自动更新配置**，单击**确定**。

7.  重启配置。

    1.  单击右上角的**操作** \> **重启 All Componens**。

    2.  在**执行集群操作**页面，输入**执行原因**，单击**确定**。


## 客户端访问Kafka

客户端通过SSL访问Kafka时需要配置security.protocol、truststore和keystore。以非安全集群为例，如果是在Kafka集群执行作业，可以配置以下信息。

```
security.protocol=SSL
ssl.truststore.location=/etc/ecm/kafka-conf/truststore
ssl.truststore.password=${password}
ssl.keystore.location=/etc/ecm/kafka-conf/keystore
ssl.keystore.password=${password}
```

如果是在Kafka集群以外的环境执行作业，可以将Kafka集群中任意节点/etc/ecm/kafka-conf/目录下的truststore和keystore文件，拷贝至运行环境做相应配置。

例如，在Kafka集群中，以Kafka自带的Producer和Consumer程序执行作业。

1.  创建配置文件ssl.properties，添加配置项。

    ```
    security.protocol=SSL
     ssl.truststore.location=/etc/ecm/kafka-conf/truststore
     ssl.truststore.password=${password}
     ssl.keystore.location=/etc/ecm/kafka-conf/keystore
     ssl.keystore.password=${password}
    ```

2.  创建Topic。

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


