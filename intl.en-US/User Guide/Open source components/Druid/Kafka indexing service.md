# Kafka indexing service {#concept_qzt_cqd_z2b .concept}

Kafka indexing service is a plug-in launched by Druid to consume Kafka data in real time using druid’s indexing service. The plug-in will enable a supervisor in Overlord. The supervisor starts some indexing tasks in Middlemanager after it starts. These tasks will connect to the Kafka cluster to consume the topic data and complete the index creation. You need to prepare a data consumption format file and manually start the supervisor through the REST API.

## Interaction with the Kafka cluster {#section_kmb_4td_z2b .section}

See the introduction in [Tranquility](intl.en-US/User Guide/Open source components/Druid/Tranquility.md#).

## Use Druid Kafka indexing service to consume Kafka data in real time {#section_fdk_4td_z2b .section}

1.  Run the following command on the Kafka cluster \(or gateway\) to create a topic named metrics.

    ```
    --If the Kafka high-security mode is enabled:
     export KAFKA_OPTS="-Djava.security.auth.login.config=/etc/ecm/kafka-conf/kafka_client_jaas.conf"
     --
     kafka-topics.sh --create --zookeeper emr-header-1:2181,emr-header-2,emr-header-3/kafka-1.0.0 --partitions 1 --replication-factor 1 --topic metrics
    ```

    You can adjust the parameters based on your needs. The /kafka-1.0.0 section of the - -zookeeper parameter is path, and you can see the value of the zookeeper.connect on the Kafka service Configuration page of the Kafka cluster. If you build your own Kafka cluster, the parmname —zookeeper parameter can be changed according to your actual configuration.

2.  Define the data format description file for the data source. You can name it as metrics-kafka.json and place it in the current directory \(or another directory that you specified\).

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
             "type": "Kafka",
             "maxRowsInMemory": "100000"
         }
     }
    ```

    **Note:** ioConfig.consumerProperties.security.protocol and ioConfig.consumerProperties.sasl.mechanism are security-related options \(not required for standard mode Kafka clusters\).

3.  Run the following command to add a Kafka supervisor.

    ```
    curl --negotiate -u:druid -b ~/cookies -c ~/cookies -XPOST -H 'Content-Type: application/json' -d @metrics-kafka.json http://emr-header-1.cluster-1234:18090/druid/indexer/v1/supervisor
    ```

    The options `—negotiate`, `-u`, `-b`, and `-c` are for high-security mode Druid clusters.

4.  Enable a console producer on the Kafka cluster.

    ```
    --If the high-security mode of Kafka is enabled:
     export KAFKA_OPTS="-Djava.security.auth.login.config=/etc/ecm/kafka-conf/kafka_client_jaas.conf"
     echo -e "security.protocol=SASL_PLAINTEXT\nsasl.mechanism=GSSAPI" > /tmp/Kafka/producer.conf
     --
     Kafka-console-producer.sh --producer.config /tmp/kafka/producer.conf --broker-list emr-worker-1:9092，emr-worker-2:9092，emr-worker-3:9092 --topic metrics
     >
    ```

    —producer.config /tmp/Kafka/producer.conf is an option for high-security mode Kafka clusters.

5.  Enter some data at the command prompt of kafka\_console\_producer.

    ```
    {"time": "2018-03-06T09:57:58Z", "url": "/foo/bar", "user": "alice", "latencyMs": 32}
     {"time": "2018-03-06T09:57:59Z", "url": "/", "user": "bob", "latencyMs": 11}
     {"time": "2018-03-06T09:58:00Z", "url": "/foo/bar", "user": "bob", "latencyMs": 45}
    ```

    The timestamp can be generated with the following Python command:

    ```
    python -c 'import datetime; print(datetime.datetime.utcnow().strftime("%Y-%m-%dT%H:%M:%SZ"))'
    ```

6.  Prepare a query file named metrics-search.json.

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

7.  Execute the query on the master node of Druid cluster.

    ```
    curl --negotiate -u:Druid -b ~/cookies -c ~/cookies -XPOST -H 'Content-Type: application/json' -d @metrics-search.json http://emr-header-1.cluster-1234:8082/druid/v2/?pretty
    ```

    The options `—negotiate`, `-u`, `-b`, and `-c` are for high-security mode Druid clusters.

8.  You will see a query result similar to the following in normal cases.

    ```
    [ {
       "timestamp" : "2018-03-06T09:00:00.000Z",
       "result": {
         "dimension" : "user",
         "value" : "bob",
         "count": 2,
       } ]
     } ]
    ```


