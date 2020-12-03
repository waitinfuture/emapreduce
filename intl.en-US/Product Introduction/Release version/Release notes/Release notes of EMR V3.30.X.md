# Release notes of EMR V3.30.X

This topic describes the release notes of E-MapReduce \(EMR\) V3.30.X, including the release date and updates.

## Release date

October 26, 2020 for EMR V3.30.0

## Updates

|Component|Description|
|---------|-----------|
|SmartData|SmartData is updated to 3.0.0.|
|Spark|-   Metadata from Alibaba Cloud Data Lake Formation \(DLF\) is supported.
-   Has dependencies are updated to 2.0.1.
-   Issues caused by backticks \(\`\) in Streaming SQL are fixed.
-   The JAR package of Delta is removed. Delta is separately deployed.
-   Logs are stored in Hadoop Distributed File System \(HDFS\) directories. |
|Hive|-   Metadata from Alibaba Cloud DLF is supported.
-   The issues caused when you read an empty Delta table storage directory and when you write data into a DUMMY file are fixed.
-   Has dependencies are updated to 2.0.1. |
|Presto|-   Metadata from Alibaba Cloud DLF is supported.
-   The limits on reading a Delta table are eliminated.
-   The issue of no Java Virtual Machine \(JVM\) configurations in high security mode is fixed.
-   Has dependencies are updated to 2.0.1. |
|HDFS|-   The Hot swap Disk Module mode is supported.
-   Has dependencies are updated to 2.0.1. |
|YARN|-   Issues related to YARN RMZKStateStore are fixed.
-   Snappy files that are generated from Log Service are supported.
-   Directory configurations in Local mode of MapReduce are modified to resolve the problems that occur in a directory permission check.
-   The Hot swap Disk Module mode is supported.
-   Logs are stored in HDFS directories.
-   Has dependencies are updated to 2.0.1. |
|ZooKeeper|-   Internal IP addresses can be bound to Elastic Compute Service \(ECS\) instances to start service ports.
-   Has dependencies are updated to 2.0.1. |
|Flink-VVP|-   Flink-VVP is updated to 1.11-2.2.2.
-   SQL and Autopilot features are supported. |
|Flink|-   Data can be written into Object Storage Service \(OSS\) in cache mode. The exactly-once semantics are supported by combining the Flink checkpoint and the source that can be retransmitted.
-   Features of open source Flink 1.11.1 are supported. An SQL statement is used to insert data into different destination tables or partitions to generate multiple outputs.
-   Has dependencies are updated to 2.0.1. |
|Impala|-   Custom configuration of parameters on the catalogd.flgs, impalad.flgs, and statestored.flgs tabs is supported.
-   Shiro is updated to 1.6.0.
-   Has dependencies are updated to 2.0.1. |
|Tez|-   Default settings of AM memory parameters are optimized.
-   Has dependencies are updated to 2.0.1. |
|Has|Has dependencies are updated to 2.0.1.|
|Storm|
|Zeppelin|
|Ranger|
|OpenLDAP|
|Oozie|
|Knox|
|Kafka|
|Hue|
|HBase|
|Druid|

