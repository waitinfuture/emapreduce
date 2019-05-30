# Components deployed on different instance nodes {#concept_266225 .concept}

This topic describes the deployment details for the components on different instance nodes in an EMR cluster.

When you create an EMR cluster, components are deployed on instance nodes based on the [instance node types](reseller.en-US/Cluster Planning and Configurations/Cluster planning/Instance types.md#). For example, the NameNode component of Hadoop HDFS is deployed on the master instance node. For large-scale clusters, you can use bootstrap actions to complete custom deployment and allocation. For example, a stand-alone ZooKeeper cluster uses an Alibaba Cloud RDS instance as the Hive metastore. The following tables describe the service components that run on different instance nodes.

## Hadoop cluster {#section_a58_4ri_e6v .section}

Deployment details for service components that run on a Hadoop cluster

-   Required services

    |Service name|Master instance node|Core instance node|
    |------------|--------------------|------------------|
    |HDFS|     -   KMS
    -   SecondaryNameNode
    -   HttpFS
    -   HDFS Client
    -   NameNode
 |     -   DataNode
    -   HDFS Client
 |
    |YARN|     -   ResourceManager
    -   App Timeline Server
    -   JobHistory
    -   WebAppProxyServer
    -   Yarn Client
 |     -   Yarn Client
    -   NodeManager
 |
    |Hive|     -   Hive MetaStore
    -   HiveServer2
    -   Hive Client
 |Hive Client|
    |GANGLIA|     -   Gmond
    -   Httpd
    -   Gmetad
    -   Ganglia Client
 |     -   Gmond
    -   Ganglia Client
 |
    |HUE|Hue|N/A|
    |SPARK|     -   Spark Client
    -   SparkHistory
    -   ThriftServer
 |Spark Client|
    |ZEPPLIN|Zeppelin|N/A|
    |TEZ|     -   Tomcat
    -   Tez Client
 |Tez Client|
    |SQOOP|Sqoop Client|Sqoop Client|
    |PIG|Pig Client|Pig Client|
    |HAPROXY|HAProxy|N/A|
    |APACHEDS|ApacheDS|N/A|
    |KNOX|Knox|N/A|

-   Optional services

    |Service name|Master instance node|Core instance node|
    |------------|--------------------|------------------|
    |Flume|     -   Flume Agent
    -   Flume Client
 |     -   Flume Agent
    -   Flume Client
 |
    |LIVY|Livy|N/A|
    |SUPERSET|Superset|N/A|
    |FLINK|     -   FlinkHistoryServer
    -   Flink Client
 |Flink Client|
    |RANGER|     -   RangerPlugin
    -   RangerAdmin
    -   RangerUserSync
 |RangerPlugin|
    |STORM|     -   Storm Client
    -   UI
    -   Nimbus
    -   Logviewer
 |     -   Storm Client
    -   Supervisor
    -   Logviewer
 |
    |PHOENIX|Phoneix Client|Phoneix Client|
    |HBASE|     -   HMaster
    -   HBase Client
    -   ThriftServer
 |     -   HBase Client
    -   HRegionServer
 |
    |ZOOKEEPER|     -   ZooKeeper follower
    -   ZooKeeper Client
 |     -   ZooKeeper leader
    -   ZooKeeper follower
    -   ZooKeeper Client
 |
    |OOZIE|Oozie|N/A|
    |PRESTO|     -   Presto Client
    -   PrestoMaster
 |     -   Presto Client
    -   PrestoWorker
 |
    |IMPALA|     -   Impala Runtime and Shell
    -   Impala Catalog Server
    -   Impala StateStore Server
 |     -   Impala Runtime and Shell
    -   Impala Daemon Server
    -   Impala StateStore Server
 |


## Druid cluster {#section_pu9_mq6_3co .section}

Deployment details for service components on a Druid cluster

-   Required services

    |Service name|Master instance node|Core instance node|
    |------------|--------------------|------------------|
    |DRUID|     -   Druid Client
    -   Coordinator
    -   Overlord
    -   Broker
 |     -   MiddleManager
    -   Historical
    -   Druid Client
 |
    |HDFS|     -   KMS
    -   SecondaryNameNode
    -   HttpFS
    -   HDFS Client
    -   NameNode
 |     -   DataNode
    -   HDFS Client
 |
    |GANGLIA|     -   Gmond
    -   Httpd
    -   Gmetad
    -   Ganglia Client
 |     -   Gmond
    -   Ganglia Client
 |
    |ZOOKEEPER|     -   ZooKeeper follower
    -   ZooKeeper Client
 |     -   ZooKeeper leader
    -   ZooKeeper follower
    -   ZooKeeper Client
 |

-   Optional services

    |Service name|Master instance node|Core instance node|
    |------------|--------------------|------------------|
    |YARN|     -   ResourceManager
    -   App Timeline Server
    -   JobHistory
    -   WebAppProxyServer
    -   Yarn Client
 |     -   Yarn Client
    -   NodeManager
 |
    |SUPERSET|Superset|N/A|


## Kafka cluster {#section_tv8_y13_rjj .section}

Deployment details for service components on a Kafka cluster

-   Required services

    |Service name|Master instance node|Core instance node|
    |------------|--------------------|------------------|
    |KAFKA-MANAGER|Kafka Manager|N/A|
    |KAFKA|     -   Kafka Client
    -   KafkaMetadataMonitor
    -   Kafka Rest Proxy
    -   Kafka Broker controller
    -   Kafka Schema Registry
 |     -   Kafka Broker broker
    -   Kafka Client
 |
    |GANGLIA|     -   Gmond
    -   Httpd
    -   Gmetad
    -   Ganglia Client
 |     -   Gmond
    -   Ganglia Client
 |
    |ZOOKEEPER|     -   ZooKeeper follower
    -   ZooKeeper Client
 |     -   ZooKeeper leader
    -   ZooKeeper follower
    -   ZooKeeper Client
 |

-   Optional services

    |Service name|Master instance node|Core instance node|
    |------------|--------------------|------------------|
    |RANGER|     -   RangerPlugin
    -   RangerUserSync
    -   RangerAdmin
 |RangerPlugin|
    |KNOX|Knox|N/A|
    |APACHEDS|ApacheDS|N/A|


## Zookeeper cluster {#section_bc3_dpo_p87 .section}

Deployment details for service components on a ZooKeeper cluster

|Service name|Master instance node|Core instance node|
|------------|--------------------|------------------|
|GANGLIA|N/A| -   Gmond
-   Httpd
-   Gmetad
-   Ganglia Client

 |
|ZOOKEEPER|N/A| -   ZooKeeper leader
-   ZooKeeper Client
-   ZooKeeper follower

 |

## Data Science cluster {#section_wf4_k7d_4ap .section}

Deployment details for service components on a Data Science cluster

-   Required services

    |Service name|Master instance node|Core instance node|
    |------------|--------------------|------------------|
    |HDFS|     -   KMS
    -   SecondaryNameNode
    -   HttpFS
    -   HDFS Client
    -   NameNode
 |     -   DataNode
    -   HDFS Client
 |
    |JUPYTER|Jupyter|Jupyter|
    |GANGLIA|     -   Gmond
    -   Httpd
    -   Gmetad
    -   Ganglia Client
 |     -   Gmond
    -   Ganglia Client
 |
    |ZOOKEEPER|     -   ZooKeeper follower
    -   ZooKeeper Client
 |     -   ZooKeeper leader
    -   ZooKeeper follower
    -   ZooKeeper Client
 |
    |ANALYTICS-ZOO|     -   Analytics-Zoo-Python
    -   Analytics-Zoo-Scala
 |     -   Analytics-Zoo-Python
    -   Analytics-Zoo-Scala
 |
    |KNOX|Knox|N/A|
    |APACHEDS|ApacheDS|N/A|
    |TENSORFLOW-ON-YARN|     -   TensorFlow-On-YARN
    -   TensorFlow-On-YARN-Gateway
    -   TensorFlow-On-YARN-History-Server
 |     -   TensorFlow-On-YARN-Client
    -   TensorFlow-On-YARN-Gateway
 |
    |TENSORFLOW|TensorFlow|TensorFlow|
    |ZEPPELIN|Zeppelin|N/A|
    |SPARK|     -   Spark Client
    -   ThriftServer
    -   SparkHistory
 |Spark Client|
    |YARN|     -   ResourceManager
    -   App Timeline Server
    -   JobHistory
    -   WebAppProxyServer
    -   Yarn Client
 |     -   Yarn Client
    -   NodeManager
 |

-   Optional services

    |Service name|Master instance node|Core instance node|
    |------------|--------------------|------------------|
    |HUE|Hue|RangerPlugin|
    |HIVE|     -   HiveServer2
    -   Hive MetaStore
    -   Hive Client
 |Hive Client|


