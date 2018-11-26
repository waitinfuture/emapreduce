# Kafka Indexing Service {#concept_qzt_cqd_z2b .concept}

Kafka Indexing Service是Druid推出的利用Druid的Indexing Service服务实时消费Kafka数据的插件。该插件会在Overlord中启动一个supervisor，supervisor启动之后会在 Middlemanager中启动一些indexing tasks，这些tasks会连接到Kafka集群消费topic数据，并完成索引创建。您需要做的，就是准备一个数据消费格式文件，之后通过REST API手动启动supervisor。

## 与Kafka集群交互 {#section_kmb_4td_z2b .section}

请参考[Druid 使用 Tranquility Kafka](intl.zh-CN/用户指南/开源组件介绍/Druid使用说明/Tranquility.md#)一节的介绍。

## 使用Druid Kafka Indexing Service实时消费Kafka数据 {#section_fdk_4td_z2b .section}

1.  在Kafka集群上（或者gateway上）执行下述命令创建一个名为“metrics” 的topic。

    ```
    -- 如果开启了Kafka 高安全：
     export KAFKA_OPTS="-Djava.security.auth.login.config=/etc/ecm/kafka-conf/kafka_client_jaas.conf"
     --
     kafka-topics.sh --create --zookeeper emr-header-1:2181,emr-header-2,emr-header-3/kafka-1.0.0 --partitions 1 --replication-factor 1 --topic metrics
    ```

    其中各个参数可根据需要进行调整。—zookeeper 参数中 /kafka-1.0.0 部分为path，其具体值您可以登录EMR控制台，在Kafka集群的Kafka服务配置页面查看 zookeeper.connect配置项的值。如果是您自己搭建的Kafka集群，—zookeeper 参数可根据您的实际配置进行改变。

2.  定义数据源的数据格式描述文件，我们将其命名为 metrics-kafka.json，并置于当前目录下面（或者放置于其他您指定的目录）。

    ```
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
             "type": "Kafka",
             "maxRowsInMemory": "100000"
         }
     }
    ```

    **说明：** ioConfig.consumerProperties.security.protocol 和 ioConfig.consumerProperties.sasl.mechanism 为安全相关选项（非安全 Kafka 集群不需要）。

3.  执行下述命令添加Kafka supervisor。

    ```
    curl --negotiate -u:druid -b ~/cookies -c ~/cookies -XPOST -H 'Content-Type: application/json' -d @metrics-kafka.json http://emr-header-1.cluster-1234:18090/druid/indexer/v1/supervisor
    ```

    其中`—negotiate`、`-u`、`-b`、`-c` 等选项是针对安全Druid集群。

4.  在Kafka集群上开启一个console producer。

    ```
    -- 如果开启了Kafka高安全：
     export KAFKA_OPTS="-Djava.security.auth.login.config=/etc/ecm/kafka-conf/kafka_client_jaas.conf"
     echo -e "security.protocol=SASL_PLAINTEXT\nsasl.mechanism=GSSAPI" > /tmp/Kafka/producer.conf
     --
     Kafka-console-producer.sh --producer.config /tmp/kafka/producer.conf --broker-list emr-worker-1:9092，emr-worker-2:9092，emr-worker-3:9092 --topic metrics
     >
    ```

    其中 —producer.config /tmp/Kafka/producer.conf 是针对安全 Kafka 集群的选项。

5.  在 kafka\_console\_producer 的命令提示符下输入一些数据。

    ```
    {"time": "2018-03-06T09:57:58Z", "url": "/foo/bar", "user": "alice", "latencyMs": 32}
     {"time": "2018-03-06T09:57:59Z", "url": "/", "user": "bob", "latencyMs": 11}
     {"time": "2018-03-06T09:58:00Z", "url": "/foo/bar", "user": "bob", "latencyMs": 45}
    ```

    其中时间戳可用如下 python 命令生成：

    ```
    python -c 'import datetime; print(datetime.datetime.utcnow().strftime("%Y-%m-%dT%H:%M:%SZ"))'
    ```

6.  准备一个查询文件，命名为 metrics-search.json。

    ```
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

7.  在 Druid 集群 master 上执行查询。

    ```
    curl --negotiate -u:Druid -b ~/cookies -c ~/cookies -XPOST -H 'Content-Type: application/json' -d @metrics-search.json http://emr-header-1.cluster-1234:8082/druid/v2/?pretty
    ```

    其中`—negotiate`、`-u`、`-b`、`-c` 等选项是针对安全 druid 集群。

8.  如果一切正常，您将看到类似如下的查询结果。

    ```
    [ {
       "timestamp" : "2018-03-06T09:00:00.000Z",
       "result" : [ {
         "dimension" : "user",
         "value" : "bob",
         "count" : 2
       } ]
     } ]
    ```


