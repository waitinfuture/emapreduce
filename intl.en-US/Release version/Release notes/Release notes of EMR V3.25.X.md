# Release notes of EMR V3.25.X

This topic describes the release notes of E-MapReduce \(EMR\) V3.25.X, including the release date, new feature, and updates.

## Release date

January 13, 2020 for EMR V3.25.0

## New feature

Operations related to Ranger Presto are supported.

## Updates

|Component|Description|
|---------|-----------|
|Ranger|-   The database of Ranger Admin is initialized in a high-availability cluster.
-   The security issues that are caused when you run scripts on Ranger UserSync are fixed. |
|Spark|-   Parameters related to Delta, such as `spark.sql.extensions`, can be configured in the EMR console.
-   Data from a Delta table can be read by using Hive to avoid manual configuration of InputFormat.
-   The ALTER TABLE SET TBLPROPERTIES and UNSET TBLPROPERTIES statements are supported. |
|Delta|
|Hive|The issue that causes execution failures of MapReduce tasks in automatic local mode is fixed.|
|Presto|-   Presto is updated to 310.
-   Joda-Time is updated to 2.10.5. |
|Tez|-   Tez is updated to 0.9.2.
-   The issue that the application running progress cannot be properly displayed on the web UI of Tez is fixed.
-   The issue that you cannot view the application running history on the web UI of Tez is fixed. |
|Impala|The issue that an LZO table cannot be accessed by using Impala is fixed.|
|HDFS|JAR packages related to Mongo-Hadoop are removed.|
|ZooKeeper|ZooKeeper is updated to 3.5.6.|
|YARN|Information on the web UI of Tez can be viewed. You can set **yarn.resourcemanager.system-metrics-publisher.enabled** to true on the **yarn-site** tab.|
|Bigboot|-   Bigboot is updated to 2.2.3.
-   The renaming operation is supported in OSS cache mode. |
|SmartData|
|Knox|Dependencies are updated.|
|Oozie|Dependencies are updated.|

