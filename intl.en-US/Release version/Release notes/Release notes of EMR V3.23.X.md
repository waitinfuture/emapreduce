# Release notes of EMR V3.23.X

This topic describes the release notes of E-MapReduce V3.23.X, including the release date and updates.

## Release date

September 18, 2019

## Updates

|Component|Description|
|---------|-----------|
|Druid|-   Upgraded Druid to version 0.15.1.
-   Added the Router component.
-   Upgraded Fastjson to version 1.2.60. |
|Spark|-   Updated the code for Spark SQL Thrift Server to fix the issue where IsolatedClassLoader cannot load classes in certain cases.
-   Refactored the code related to Spark transactions to improve stability.
-   Fixed the issue where optimized row columnar \(ORC\) files cannot be read or written after the built-in Hive is upgraded to version 2.3.
-   Supports the MERGE INTO syntax.
-   Supports the SCAN and STREAM syntaxes.
-   Supports exactly-once semantics \(EOS\) for the Structured Streaming Kafka sink.
-   Upgraded Delta to version 0.4.0. |
|Hive|-   Removed Hive hooks configured in earlier versions of Hive.
-   Supports using multiple COUNT\(DISTINCT\) for hive.groupby.skew in data optimization.
-   Fixed the issue of data loss when joining tables with different bucket versions. |
|Flink|Upgraded Flink to version 1.8.2.|
|Bigboot|-   Upgraded the HDFS FsImage analysis tool.
-   Updated the JAR package for Object Storage Service \(OSS\) to fix the issue where the non-Daemon thread keeps alive after the main thread is terminated. |
|Kafka|-   Supports the DepolymentSet-Aware replication distribution strategy in Kafka.
-   Removed the Fastjson dependency. |
|HDFS|-   Optimized the deployment logic of the JAR package for SmartData OSS.
-   Updated the JAR package for SmartData OSS. |
|Flume|Upgraded Fastjson to version 1.2.60.|
|Tensorflow on Spark|This is a new component.|
|Has|Upgraded Fastjson to version 1.2.60.|
|Livy|Upgraded Fastjson to version 1.2.60.|

