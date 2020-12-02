# Release notes of EMR V4.4.1

This topic describes the release notes of E-MapReduce \(EMR\) V4.4.1, including the release date and updates.

## Release date

September 15, 2020 for EMR V4.4.1

## Updates

|Component|Description|
|---------|-----------|
|YARN|-   The hadoop/tools/lib directory is deleted from the value of the yarn.application.classpath parameter.
-   Default parameter settings for MapReduce jobs are optimized. |
|Hive|Default parameter settings are optimized.|
|Tez|
|Ranger|-   Impala-based access control is supported.
-   The jackson-databind version is updated. |
|Impala|-   Integration with Ranger is supported.
-   Shiro is updated to 1.6.0. |
|SmartData|SmartData is updated to 2.7.301.|
|Bigboot|
|Knox|-   The web UI of Tez can be independently viewed. Knox is compatible with Tez on the web UI of YARN.
-   Shiro is updated to 1.6.0. |
|EMRDOCTOR|The following issue is fixed: Job information is not collected when a time-based configuration file is empty.|
|Ganglia|The detection feature of the service RPC port for Hadoop Distributed File System \(HDFS\) is enabled.|
|Oozie|-   The issue that the web UI cannot be accessed is fixed.
-   The jackson-databind version is updated. |
|ZooKeeper|Internal IP addresses can be bound to Elastic Compute Service \(ECS\) instances to start service ports.|
|Superset|The startup script is repaired.|
|Livy|The versions of jackson-databind and Fastjson are updated.|
|Zepplin|The versions of jackson-databind and Shiro are updated.|
|Has|The versions of jackson-databind and Fastjson are updated.|
|Flume|The Fastjson version is updated.|

