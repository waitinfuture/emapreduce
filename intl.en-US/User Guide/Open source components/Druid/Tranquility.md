# Tranquility {#concept_t2g_jqd_z2b .concept}

Tranquility is an application that sends data to Druid in real-time in push mode. It solves many issues, such as multiple partitions, multiple copies, service discovery, and data loss. It simplifies the usage of Druid for users. It supports a wide range of data sources, including Samza, Spark, Storm, Kafka, and Fink. This section uses Kafka as an example, and describes how to use Tranquility in the EMR to capture data from the Kafka cluster and push the data to the Druid cluster in real time.

## Interaction with the Kafka cluster {#section_xbp_tsd_z2b .section}

The first is the interaction between the Druid cluster and Kafka cluster. The interaction configuration of the two clusters is similar to that of the Hadoop cluster. You have to set the connectivity and hosts. For standard mode Kafka clusters, perform the following steps:

1.  Ensure the communication between clusters. \(The two clusters are in the same security group. Or each cluster is associated with a different security group, and access rules are configured for the two security groups.\)
2.  Write the hosts of the Kafka cluster to the hosts list of each node on the Druid cluster. Note that the hostname of the Kafka cluster should be in the form of a long name, such as emr-header-1.cluster-xxxxxxxx.

For high-security mode Kafka clusters, you need to perform the following operations \(the first two steps are the same as those for standard mode clusters\):

1.  Ensure the communication between the two clusters \(The two clusters are in the same security group. Or each cluster is associated with a different security group, and access rules are configured for the two security groups\).
2.  Write the hosts of the Kafka cluster to the hosts list of each node on the Druid cluster. Note that the hostname of the Kafka cluster should be in the form of a long name, such as emr-header-1.cluster-xxxxxxxx.
3.  Set Kerberos cross-domain mutual trust between the two clusters. \(For details, see [Cross-region access](intl.en-US/User Guide/Kerberos authentication/Cross-region access.md#)here. The bidirectional mutual trust is preferred.
4.  Prepare a client security configuration file:

    ```
    KafkaClient {
          com.sun.security.auth.module.Krb5LoginModule required
          useKeyTab=true
          storeKey=true
          keyTab="/etc/ecm/druid-conf/druid.keytab"
          principal="druid@EMR. 1234. COM";
      };
    ```

    Synchronize the configuration file to all nodes in the Druid cluster, and place it to a specific directory such as /tmp/kafka/kafka\_client\_jaas.conf.

5.  In overlord.jvm of the Druid configuration page:

    ```
    Add Djava.security.auth.login.config=/tmp/kafka/kafka_client_jaas.conf
    ```

    .

6.  Configure the following option in middleManager.runtime on the Druid configuration page: `druid.indexer.runner.javaOpts=-Djava.security.auth.login.confi=/tmp/kafka/kafka_client_jaas.conf` and other jvm startup parameters.
7.  Restart the Druid service.

## Use Tranquility Kafka {#section_q4h_ctd_z2b .section}

Because Tranquility is a service, it is a consumer for Kafka and a client for Druid. You can use a neutral machine to run Tranquility, as long as this machine is able to connect to the Kafka cluster and the Druid cluster simultaneously.

1.  Create a topic named pageViews on the Kafka side.

    ```
    --If the Kafka high-security mode is enabled:
     export KAFKA_OPTS="-Djava.security.auth.login.config=/etc/ecm/kafka-conf/kafka_client_jaas.conf"
     --
     ./bin/kafka-topics.sh --create --zookeeper emr-header-1:2181,emr-header-2:2181,emr-header-3:2181/kafka-1.0.1 --partitions 1 --replication-factor 1 --topic pageViews
    ```

2.  Download the Tranquility installation package and decompress it to a path.
3.  Configure the dataSource.

    It is assumed that your topic name is pageViews, and each topic is a JSON file.

    ```
    {"time": "2018-05-23T11:59:43Z", "url": "/foo/bar", "user": "alice", "latencyMs": 32}
     {"time": "2018-05-23T11:59:44Z", "url": "/", "user": "bob", "latencyMs": 11}
     {"time": "2018-05-23T11:59:45Z", "url": "/foo/bar", "user": "bob", "latencyMs": 45}
    ```

    The configuration of the corresponding dataSource is as follows:

    ```
    {
       "dataSources" : {
         "pageViews-kafka" : {
           "spec" : {
             "dataSchema" : {
               "dataSource" : "pageViews-kafka",
               "parser" : {
                 "type" : "string",
                 "parseSpec" : {
                   "timestampSpec" : {
                     "column" : "time",
                     "format" : "auto"
                   },
                   "dimensionsSpec" : {
                     "dimensions" : ["url", "user"],
                     "dimensionExclusions" : [
                       "timestamp",
                       "value"
                     ]
                   },
                   "format" : "json"
                 }
               },
               "granularitySpec" : {
                 "type" : "uniform",
                 "segmentGranularity" : "hour",
                 "queryGranularity" : "none"
               },
               "metricsSpec" : [
                 {"name": "views", "type": "count"},
                 {"name": "latencyMs", "type": "doubleSum", "fieldName": "latencyMs"}
               ]
             },
             "ioConfig" : {
               "type" : "realtime"
             },
             "tuningConfig" : {
               "type" : "realtime",
               "maxRowsInMemory" : "100000",
               "intermediatePersistPeriod" : "PT10M",
               "windowPeriod" : "PT10M"
             }
           },
           "properties" : {
             "task.partitions" : "1",
             "task.replicants" : "1",
             "topicPattern" : "pageViews"
           }
         }
       },
       "properties" : {
         "zookeeper.connect" : "localhost",
         "druid.discovery.curator.path" : "/druid/discovery",
         "druid.selectors.indexing.serviceName" : "druid/overlord",
         "commit.periodMillis" : "15000",
         "consumer.numThreads" : "2",
         "kafka.zookeeper.connect" : "emr-header-1.cluster-500148518:2181,emr-header-2.cluster-500148518:2181,   emr-header-3.cluster-500148518:2181/kafka-1.0.1",
         "kafka.group.id" : "tranquility-kafka",
       }
     }
    ```

4.  Run the following command to start Tranquility.

    ```
    ./bin/tranquility kafka -configFile 
    ```

5.  Start the producer and configure it to send some data.

    ```
    ./bin/kafka-console-producer.sh --broker-list emr-worker-1:9092,emr-worker-2:9092,emr-worker-3:9092 --topic pageViews
    ```

    Enter the following codes:

    ```
    {"time": "2018-05-24T09:26:12Z", "url": "/foo/bar", "user": "alice", "latencyMs": 32}
     {"time": "2018-05-24T09:26:13Z", "url": "/", "user": "bob", "latencyMs": 11}
     {"time": "2018-05-24T09:26:14Z", "url": "/foo/bar", "user": "bob", "latencyMs": 45}
    ```

    You can view specific information in Tranquility log. At the same time, the corresponding real-time indexing task has been started on the Druid side.


