# Deployment of service components

This topic describes the deployment details of service components on each node in different types of EMR clusters.

When you create an EMR cluster, different service components are deployed on different categories of nodes. For example, the NameNode component of Hadoop HDFS is deployed on the master node.

## View deployment details of service components on a cluster

1.  Log on to the [EMR console](https://emr.console.aliyun.com/).

2.  In the top navigation bar, select the region where your cluster residesand select a resource group based on your business requirements.

3.  Click the **Cluster Management** tab.

4.  On the **Cluster Management** page, find your cluster and click **Details** in the Actions column.

5.  In the left-side navigation pane, click **Cluster Service** and choose the service for which you want to view the deployment details of components.

6.  On the service page that appears, click the **Component Deployment** tab.

    You can view specific deployment information of service components.


## Hadoop clusters

Deployment details of the service components that run on a Hadoop cluster of EMR V3.29.0:

-   Required services

    |Service name|Master node|Core node|
    |------------|-----------|---------|
    |HDFS|    -   KMS
    -   SecondaryNameNode
    -   HttpFS
    -   HDFS Client
    -   NameNode
|    -   DataNode
    -   HDFS Client |
    |YARN|    -   ResourceManager
    -   App Timeline Server
    -   JobHistory
    -   WebAppProxyServer
    -   Yarn Client
|    -   Yarn Client
    -   NodeManager |
    |Hive|    -   Hive MetaStore
    -   HiveServer2
    -   Hive Client
|Hive Client|
    |Spark|    -   Spark Client
    -   SparkHistory
    -   ThriftServer
|Spark Client|
    |Knox|Knox|N/A|
    |Tez|    -   Tomcat
    -   Tez Client
|Tez Client|
    |Ganglia|    -   Gmond
    -   Httpd
    -   Gmetad
    -   Ganglia Client
|    -   Gmond
    -   Ganglia Client |
    |Sqoop|Sqoop Client|Sqoop Client|
    |Bigboot|    -   Bigboot Client
    -   Bigboot Monitor
|    -   Bigboot Client
    -   Bigboot Monitor |
    |OpenLDAP|OpenLDAP|N/A|
    |Hue|Hue|N/A|
    |SmartData|    -   Jindo Namespace Service
    -   Jindo Storage Service
    -   Jindo Client
|    -   Jindo Storage Service
    -   Jindo Client |

-   Optional services

    |Service name|Master node|Core node|
    |------------|-----------|---------|
    |Livy|Livy|N/A|
    |Superset|Superset|N/A|
    |Flink|    -   FlinkHistoryServer
    -   Flink Client
|Flink Client|
    |Ranger|    -   RangerPlugin
    -   RangerAdmin
    -   RangerUserSync
    -   Solr
|RangerPlugin|
    |Storm|    -   Storm Client
    -   UI
    -   Nimbus
    -   Logviewer
|    -   Storm Client
    -   Supervisor
    -   Logviewer |
    |Phoenix|Phoenix Client|Phoenix Client|
    |Kudu|    -   Kudu Master
    -   Kudu Client
|    -   Kudu Tserver
    -   Kudu Master
    -   Kudu Client |
    |HBase|    -   HMaster
    -   HBase Client
    -   ThriftServer
|    -   HBase Client
    -   HRegionServer |
    |ZooKeeper|    -   ZooKeeper follower
    -   ZooKeeper Client
|    -   ZooKeeper follower
    -   ZooKeeper leader
    -   ZooKeeper Client |
    |Oozie|Oozie|N/A|
    |Presto|    -   Presto Client
    -   PrestoMaster
|    -   Presto Client
    -   PrestoWorker |
    |Impala|    -   Impala Runtime and Shell
    -   Impala Catalog Server
    -   Impala StateStore Server
|    -   Impala Runtime and Shell
    -   Impala Daemon Server |
    |Pig|Pig Client|Pig Client|
    |Zeppelin|Zeppelin|N/A|
    |Flume|    -   Flume Agent
    -   Flume Client
|    -   Flume Agent
    -   Flume Client |


## Druid clusters

Deployment details of the service components that run on a Druid cluster of EMR V3.29.0:

-   Required services

    |Service name|Master node|Core node|
    |------------|-----------|---------|
    |Druid|    -   Druid Client
    -   Coordinator
    -   Overlord
    -   Broker
    -   Router
|    -   MiddleManager
    -   Historical
    -   Druid Client |
    |HDFS|    -   KMS
    -   SecondaryNameNode
    -   HttpFS
    -   HDFS Client
    -   NameNode
|    -   DataNode
    -   HDFS Client |
    |Ganglia|    -   Gmond
    -   Httpd
    -   Gmetad
    -   Ganglia Client
|    -   Gmond
    -   Ganglia Client |
    |ZooKeeper|    -   ZooKeeper follower
    -   ZooKeeper Client
|    -   ZooKeeper leader
    -   ZooKeeper follower
    -   ZooKeeper Client |
    |OpenLDAP|OpenLDAP|N/A|
    |Bigboot|    -   Bigboot Client
    -   Bigboot Monitor
|    -   Bigboot Client
    -   Bigboot Monitor |
    |SmartData|    -   Jindo Namespace Service
    -   Jindo Storage Service
    -   Jindo Client
|    -   Jindo Storage Service
    -   Jindo Client |

-   Optional services

    |Service name|Master node|Core node|
    |------------|-----------|---------|
    |YARN|    -   ResourceManager
    -   App Timeline Server
    -   JobHistory
    -   WebAppProxyServer
    -   Yarn Client
|    -   Yarn Client
    -   NodeManager |
    |Superset|Superset|N/A|


## Kafka clusters

Deployment details of the service components that run on a Kafka cluster of EMR V3.29.0:

-   Required services

    |Service name|Master node|Core node|
    |------------|-----------|---------|
    |Kafka-Manager|Kafka Manager|N/A|
    |Kafka|    -   Kafka Client
    -   KafkaMetadataMonitor
    -   Kafka Rest Proxy
    -   Kafka Broker broker
    -   Kafka Schema Registry
|    -   Kafka Broker broker
    -   Kafka Client |
    |Ganglia|    -   Gmond
    -   Httpd
    -   Gmetad
    -   Ganglia Client
|    -   Gmond
    -   Ganglia Client |
    |ZooKeeper|    -   ZooKeeper follower
    -   ZooKeeper Client
|    -   ZooKeeper leader
    -   ZooKeeper follower
    -   ZooKeeper Client |
    |OpenLDAP|OpenLDAP|N/A|

-   Optional services

    |Service name|Master node|Core node|
    |------------|-----------|---------|
    |Ranger|    -   RangerPlugin
    -   RangerUserSync
    -   RangerAdmin
    -   Solr
|RangerPlugin|
    |Knox|Knox|N/A|


## ZooKeeper clusters

Deployment details of the service components that run on a ZooKeeper cluster of EMR 3.29.0:

|Service name|Master node|Core node|
|------------|-----------|---------|
|Ganglia|N/A|-   Gmond
-   Httpd
-   Gmetad
-   Ganglia Client |
|ZooKeeper|N/A|-   ZooKeeper leader
-   ZooKeeper Client
-   ZooKeeper follower |

## Dataflow clusters

Deployment details of the service components that run on a Dataflow cluster of EMR V3.28.2:

-   Required services

|Service name|Master node|Core node|
|------------|-----------|---------|
|HDFS|-   KMS
-   SecondaryNameNode
-   HttpFS
-   HDFS Client
-   NameNode
|-   DataNode
-   HDFS Client |
|YARN|-   ResourceManager
-   App Timeline Server
-   JobHistory
-   WebAppProxyServer
-   Yarn Client
|-   Yarn Client
-   NodeManager |
|Ganglia|-   Gmond
-   Httpd
-   Gmetad
-   Ganglia Client
|-   Gmond
-   Ganglia Client |
|ZooKeeper|-   ZooKeeper follower
-   ZooKeeper Client
|-   ZooKeeper leader
-   ZooKeeper follower
-   ZooKeeper Client |
|Flink-Vvp|Flink-Vvp|N/A|
|SmartData|-   JindoFS Namespace Service
-   JindoFS Storage Service
-   SmartData Client
|-   JindoFS Storage Service
-   SmartData Client |
|Bigboot|-   Bigboot Client
-   Bigboot Monitor
|-   Bigboot Client
-   Bigboot Monitor |
|OpenLDAP|OpenLDAP|N/A|

-   Optional services

    PAI-Alink: Alink


## Data Science clusters

Deployment details of the service components that run on a Data Science cluster of EMR V3.29.1:

-   Required services

    |Service name|Master node|Core node|
    |------------|-----------|---------|
    |HDFS|    -   HDFS Client
    -   KMS
    -   HttpFS
    -   NameNode
    -   SecondaryNameNode
|    -   HDFS Client
    -   DataNode |
    |YARN|    -   WebAppProxyServer
    -   JobHistory
    -   App Timeline Server
    -   ResourceManager
    -   Yarn Client
|    -   NodeManager
    -   Yarn Client |
    |ZooKeeper|    -   ZooKeeper Client
    -   ZooKeeper follower
|    -   ZooKeeper Client
    -   ZooKeeper leader
    -   ZooKeeper follower |
    |Knox|Knox|N/A|
    |TensorFlow on YARN|    -   TensorFlow-On-YARN-Gateway
    -   TensorFlow-On-YARN-History-Server
    -   TensorFlow-On-YARN
|    -   TensorFlow-On-YARN-Client
    -   TensorFlow-On-YARN-Gateway |
    |SmartData|    -   Jindo Namespace Service
    -   Jindo Storage Service
    -   Jindo Client
|    -   Jindo Storage Service
    -   Jindo Client |
    |Bigoot|    -   Bigboot Monitor
    -   Bigboot Client
|    -   Bigboot Monitor
    -   Bigboot Client |
    |PAI-EASYREC|Easyrec|Easyrec|
    |PAI-EAS|PAIEAS|PAIEAS|
    |PAI-Faiss|Faiss|Faiss|
    |PAI-Redis|Redis|Redis|
    |PAI-Alink|Alink|N/A|
    |Flink-Vvp|Flink-Vvp|N/A|
    |OpenLDAP|OpenLDAP|N/A|
    |Jindo SDK|Jindo SDK|Jindo SDK|

-   Optional services

    |Service name|Master node|Core node|
    |------------|-----------|---------|
    |Zeppelin|Zeppelin|N/A|
    |PAI-REC|Rec|N/A|
    |AUTOML|AUTOML|AUTOML|
    |TensorFlow|TensorFlow|TensorFlow|


