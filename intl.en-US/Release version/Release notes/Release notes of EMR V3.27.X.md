# Release notes of EMR V3.27.X

This topic describes the release notes of E-MapReduce \(EMR\) V3.27.X, including release dates, new features, and updates.

## Release dates

|Version|Release date|
|-------|------------|
|EMR V3.27.0|April 29, 2020|
|EMR V3.27.1|May 8, 2020|
|EMR V3.27.2|May 20, 2020|

## New features

|Feature|Description|
|-------|-----------|
|Custom component deployment|Components on the master node can be customized. The following components are supported: -   Hadoop
-   Spark
-   Hive
-   Zookeeper
-   Presto |
|Graceful release during auto scaling|Graceful release during auto scaling is supported. After you enable graceful release, the nodes that are removed during a scale-in are not immediately released. Instead, they are released after tasks are completed within the specified period of time.|

## Updates

|Component|Description|
|---------|-----------|
|Spark|-   Partition fields of the date type are supported in the cube.
-   The stack depth in the spark-submit scripts is increased. |
|Delta|-   DDL statements, including CREATE, SHOW, and DESCRIBE, are enhanced.
-   The OPTIMIZE statement for Z-order is supported. |
|Knox|-   Information on the web UI of Druid can be viewed.
-   Deployment on multiple master nodes is supported. |
|Hive|-   The magic committer in an HCatalog table is supported.
-   Some outdated default configurations are removed. |
|Bigboot|-   Bigboot is updated to 2.6.3.
-   Deployment on multiple master nodes is supported. |
|SmartData|-   SmartData is updated to 2.6.3.
-   Deployment on multiple master nodes is supported. |
|Ranger|-   Solr is supported.
-   PrestoSQL 311 is supported. |
|Tez|The ScratchDir option can be used to specify a temporary directory in OSS.|
|Presto|Presto is updated to 331.|
|Druid|Druid is updated to 0.17.1.|
|Superset|Superset is updated to 0.35.2.|
|Sqoop|-   The MySQL JDBC JAR package is updated to 5.1.48.
-   Custom encoding is supported by using `--mysql-charset` in MySQL direct export mode. |

