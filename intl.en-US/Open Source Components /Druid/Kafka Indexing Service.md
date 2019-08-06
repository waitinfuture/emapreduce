# Kafka Indexing Service {#concept_qzt_cqd_z2b .concept}

This section describes how to use Apache Druid Kafka Indexing Service in E-MapReduce to ingest Kafka data in real time.

The Kafka Indexing Service is an extension launched by Apache Druid to ingest Kafka data in real time using Apache Druid's indexing service. The extension enables supervisors in Overlord which start some indexing tasks in Middlemanager. These tasks connect to the Kafka cluster to ingest the topic data and complete the index creation. You need to prepare a data ingestion format file and manually start the supervisor through the RESTful API.

## Interaction with the Kafka cluster {#section_kmb_4td_z2b .section}

See the introduction in [Tranquility](intl.en-US/Open Source Components /Druid/Tranquility.md#).

## Use Apache Druid's Kafka Indexing Service to ingest Kafka data in real time {#section_fdk_4td_z2b .section}

1.  Run the following command on the Kafka cluster \(or gateway\) to create a topic named metrics.

    ``` {#codeblock_h54_2i2_2hz}
    --If the Kafka high-security mode is enabled:
     export KAFKA_OPTS="-Djava.security.auth.login.config=/etc/ecm/kafka-conf/kafka_client_jaas.conf"
     --
     kafka-topics.sh --create --zookeeper emr-header-1:2181,emr-header-2,emr-header-3/kafka-1.0.0 --partitions 1 --replication-factor 1 --topic metrics
    ```

    You can adjust the parameters based on your needs. The /kafka-1.0.0 section of the - -zookeeper parameter is path, and you can see the value of the zookeeper.connect on the Kafka service Configuration page of the Kafka cluster. If you build your own Kafka cluster, the parmname —zookeeper parameter can be changed according to your actual configuration.

2.  Define the data format description file for the data source. Name it metrics-kafka.json and place it in the current directory \(or another directory that you have specified\).

    ``` {#codeblock_hbg_s3f_ek3}
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
                 "bootstrap.servers": "emr-worker-1.cluster-xxxxxxxx:9092 (the bootstrap.servers of your Kafka clusters)",
                 "group.id": "kafka-indexing-service",
                 "security.protocol": "SASL_PLAINTEXT",
                 "sasl.mechanism": "GSSAPI"
             },
             "taskCount": 1,
             replicas: 1
             "taskDuration": "PT1H"
         },
         "tuningConfig": {
             "type": "kafka",
             "maxRowsInMemory": "100000"
         }
     }
    ```

    **Note:** ioConfig.consumerProperties.security.protocol and ioConfig.consumerProperties.sasl.mechanism are security-related options and are not required for standard mode Kafka clusters.

3.  Run the following command to add a Kafka supervisor.

    ``` {#codeblock_tjv_f2l_ipg}
    curl --negotiate -u:druid -b ~/cookies -c ~/cookies -XPOST -H 'Content-Type: application/json' -d @metrics-kafka.json http://emr-header-1.cluster-1234:18090/druid/indexer/v1/supervisor
    ```

    The `—negotiate`, `-u`, `-b`, and `-c`options are for high-security mode Druid clusters.

4.  Enable a console producer on the Kafka cluster.

    ``` {#codeblock_yw7_lgf_6z5}
    --If the high-security mode of Kafka is enabled:
     export KAFKA_OPTS="-Djava.security.auth.login.config=/etc/ecm/kafka-conf/kafka_client_jaas.conf"
     echo -e "security.protocol=SASL_PLAINTEXT\nsasl.mechanism=GSSAPI" > /tmp/Kafka/producer.conf
     --
     Kafka-console-producer.sh --producer.config /tmp/kafka/producer.conf --broker-list emr-worker-1:9092，emr-worker-2:9092，emr-worker-3:9092 --topic metrics
     >
    ```

    The —producer.config /tmp/Kafka/producer.confoption is for high-security mode Kafka clusters.

5.  Enter data at the command prompt of kafka\_console\_producer.

    ``` {#codeblock_u2l_ofn_0h5}
    {"time": "2018-03-06T09:57:58Z", "url": "/foo/bar", "user": "alice", "latencyMs": 32}
     {"time": "2018-03-06T09:57:59Z", "url": "/", "user": "bob", "latencyMs": 11}
     {"time": "2018-03-06T09:58:00Z", "url": "/foo/bar", "user": "bob", "latencyMs": 45}
    ```

    The timestamp can be generated with the following Python command:

    ``` {#codeblock_vdu_6ij_dtm}
    python -c 'import datetime; print(datetime.datetime.utcnow().strftime("%Y-%m-%dT%H:%M:%SZ"))'
    ```

6.  Prepare a query file named metrics-search.json.

    ``` {#codeblock_9cz_a6o_bip}
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

7.  Execute the query on the master node of the Druid cluster.

    ``` {#codeblock_vyz_2id_tu0}
    curl --negotiate -u:Druid -b ~/cookies -c ~/cookies -XPOST -H 'Content-Type: application/json' -d @metrics-search.json http://emr-header-1.cluster-1234:8082/druid/v2/?pretty
    ```

    The `—negotiate`, `-u`, `-b`, and `-c`options are for high-security mode Druid clusters.

8.  You will see a query result similar to the following:

    ``` {#codeblock_xfs_enb_myd}
    [ {
       "timestamp" : "2018-03-06T09:00:00.000Z",
       "result": {
         "dimension" : "user",
         "value" : "bob",
         "count": 2,
       } ]
     } ]
    ```


