# Kafka Indexing Service {#concept_qzt_cqd_z2b .concept}

本文将介绍在 E-MapReduce 中如何使用 Apache Druid Kafka Indexing Service 实时消费 Kafka 数据。

Kafka Indexing Service 是 Apache Druid 推出的利用 Apache Druid 的 Indexing Service 服务实时消费 Kafka 数据的插件。该插件会在 Overlord 中启动一个 supervisor，supervisor 启动之后会在 Middlemanager 中启动一些 indexing task，这些 task 会连接到 Kafka 集群消费 topic 数据，并完成索引创建。您需要做的，就是准备一个数据消费格式文件，之后通过 REST API 手动启动 supervisor。

## 与 Kafka集群交互 {#section_kmb_4td_z2b .section}

请参考[Druid 使用 Tranquility Kafka](intl.zh-CN/开源组件介绍/Druid 使用说明/Tranquility.md#)一节的介绍。

## 使用 Apache Druid Kafka Indexing Service 实时消费Kafka数据 {#section_fdk_4td_z2b .section}

1.  在 Kafka 集群上（或者 gateway 上）执行下述命令创建一个名为 “metrics” 的 topic。

    ``` {#codeblock_g8e_4at_laf}
    -- 如果开启了 Kafka 高安全：
     export KAFKA_OPTS="-Djava.security.auth.login.config=/etc/ecm/kafka-conf/kafka_client_jaas.conf"
     --
     kafka-topics.sh --create --zookeeper emr-header-1:2181,emr-header-2,emr-header-3/kafka-1.0.0 --partitions 1 --replication-factor 1 --topic metrics
    ```

    其中各个参数可根据需要进行调整。—zookeeper 参数中 /kafka-1.0.0 部分为 path，其具体值您可以登录 EMR 控制台，在 Kafka 集群的 Kafka 服务配置页面查看 zookeeper.connect 配置项的值。如果是您自己搭建的 Kafka 集群，—zookeeper 参数可根据您的实际配置进行改变。

2.  定义数据源的数据格式描述文件，我们将其命名为 metrics-kafka.json，并置于当前目录下面（或者放置于其他您指定的目录）。

    ``` {#codeblock_w2h_0qg_v7r}
    {
         "type": "kafka",
         "dataSchema": {
             "dataSource": "metrics-kafka",
             "parser": {
                 "type": "string",
                 "parseSpec": {
                     "timestampSpec": {
                         "column": "time",
                         "format": "auto"
                     },
                     "dimensionsSpec": {
                         "dimensions": ["url", "user"]
                     },
                     "format": "json"
                 }
             },
             "granularitySpec": {
                 "type": "uniform",
                 "segmentGranularity": "hour",
                 "queryGranularity": "none"
             },
             "metricsSpec": [{
                     "type": "count",
                     "name": "views"
                 },
                 {
                     "name": "latencyMs",
                     "type": "doubleSum",
                     "fieldName": "latencyMs"
                 }
             ]
         },
         "ioConfig": {
             "topic": "metrics",
             "consumerProperties": {
                 "bootstrap.servers": "emr-worker-1.cluster-xxxxxxxx:9092(您 Kafka 集群的 bootstrap.servers)",
                 "group.id": "kafka-indexing-service",
                 "security.protocol": "SASL_PLAINTEXT",
                 "sasl.mechanism": "GSSAPI"
             },
             "taskCount": 1,
             "replicas": 1,
             "taskDuration": "PT1H"
         },
         "tuningConfig": {
             "type": "kafka",
             "maxRowsInMemory": "100000"
         }
     }
    ```

    **说明：** ioConfig.consumerProperties.security.protocol 和 ioConfig.consumerProperties.sasl.mechanism 为安全相关选项（非安全 Kafka 集群不需要）。

3.  执行下述命令添加 Kafka supervisor。

    ``` {#codeblock_gyc_39j_mq2}
    curl --negotiate -u:druid -b ~/cookies -c ~/cookies -XPOST -H 'Content-Type: application/json' -d @metrics-kafka.json http://emr-header-1.cluster-1234:18090/druid/indexer/v1/supervisor
    ```

    其中 `—negotiate`、`-u`、`-b`、`-c` 等选项是针对安全 E-MapReduce Druid 集群。

4.  在 Kafka 集群上开启一个 console producer。

    ``` {#codeblock_0s8_17j_ubm}
    -- 如果开启了 Kafka 高安全：
     export KAFKA_OPTS="-Djava.security.auth.login.config=/etc/ecm/kafka-conf/kafka_client_jaas.conf"
     echo -e "security.protocol=SASL_PLAINTEXT\nsasl.mechanism=GSSAPI" > /tmp/Kafka/producer.conf
     --
     Kafka-console-producer.sh --producer.config /tmp/kafka/producer.conf --broker-list emr-worker-1:9092，emr-worker-2:9092，emr-worker-3:9092 --topic metrics
     >
    ```

    其中 —producer.config /tmp/Kafka/producer.conf 是针对安全 Kafka 集群的选项。

5.  在 kafka\_console\_producer 的命令提示符下输入一些数据。

    ``` {#codeblock_2bl_cn3_3ma}
    {"time": "2018-03-06T09:57:58Z", "url": "/foo/bar", "user": "alice", "latencyMs": 32}
     {"time": "2018-03-06T09:57:59Z", "url": "/", "user": "bob", "latencyMs": 11}
     {"time": "2018-03-06T09:58:00Z", "url": "/foo/bar", "user": "bob", "latencyMs": 45}
    ```

    其中时间戳可用如下 python 命令生成：

    ``` {#codeblock_qtd_4qc_bvd}
    python -c 'import datetime; print(datetime.datetime.utcnow().strftime("%Y-%m-%dT%H:%M:%SZ"))'
    ```

6.  准备一个查询文件，命名为 metrics-search.json。

    ``` {#codeblock_8qp_1el_8e0}
    {
         "queryType" : "search",
         "dataSource" : "metrics-kafka",
         "intervals" : ["2018-03-02T00:00:00.000/2018-03-08T00:00:00.000"],
         "granularity" : "all",
         "searchDimensions": [
             "url",
             "user"
         ],
         "query": {
             "type": "insensitive_contains",
             "value": "bob"
         }
     }
    ```

7.  在 E-MapReduce Druid 集群 master 上执行查询。

    ``` {#codeblock_2b9_88l_adg}
    curl --negotiate -u:Druid -b ~/cookies -c ~/cookies -XPOST -H 'Content-Type: application/json' -d @metrics-search.json http://emr-header-1.cluster-1234:8082/druid/v2/?pretty
    ```

    其中`—negotiate`、`-u`、`-b`、`-c` 等选项是针对安全 E-MapReduce Druid 集群。

8.  如果一切正常，您将看到类似如下的查询结果。

    ``` {#codeblock_2yg_ogx_27a}
    [ {
       "timestamp" : "2018-03-06T09:00:00.000Z",
       "result" : [ {
         "dimension" : "user",
         "value" : "bob",
         "count" : 2
       } ]
     } ]
    ```


