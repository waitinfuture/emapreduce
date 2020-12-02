# Release notes of EMR V4.4.0

This topic describes the release notes of E-MapReduce \(EMR\) V4.4.0, including the release date and updates.

## Release date

July 8, 2020 for EMR V4.4.0

## Updates

|Component|Description|
|---------|-----------|
|Spark|-   A Java agent is added for the Thrift Server process.
-   The NOT IN subquery performance of Spark SQL is optimized.
-   The following issue is fixed: A stack overflow occurs when a large number of empty files are read.
-   Explicit dependencies on Has are removed by using code or Has dependencies are updated. |
|Hive|-   Hive is updated to 3.1.2.
-   JindoFS is optimized.
-   Metastore consistency check \(MSCK\) is optimized.
-   The Jindo Job Committer in an HCatalog table is supported.
-   Has dependencies are updated. |
|Knox|-   Knox is compatible with Impala.
-   Knox is compatible with later Flink versions. |
|Impala|Impala is updated to 3.4 and can be integrated with Ranger.|
|SmartData|SmartData is updated to 2.7.202. Bigboot is updated to 2.7.202.|
|Bigboot|
|Superset|Superset is updated to 0.36.0.|
|Has|Has is updated to 1.2.0. The issue that Has is incompatible with later OpenJDK versions is fixed.|
|Livy|Has dependencies are updated.|
|YARN|
|HDFS|-   Configuration defects in Object Storage Service \(OSS\) are fixed.
-   Has dependencies are updated. |
|HBase|Has dependencies are updated.|
|Tez|
|Oozie|

