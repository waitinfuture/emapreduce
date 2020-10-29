# Release notes of EMR V3.22.X

This topic describes the release notes of E-MapReduce V3.22.X, including the release date, new features, and updates.

## Release date

July 28, 2019

## New features

|Component|Description|
|---------|-----------|
|Kudu|-   The new component, Kudu, is added. Kudu fills in the gaps in the Hadoop ecosystem. Similar to HBase, you can use Kudu to quickly read, write, and modify data. In addition, you can use Kudu to query and analyze tremendous Parquet data or larger amount of data stored in HDFS.
    -   Kudu provides the C++ API and Java API for secondary development.
    -   Kudu integrates Impala, Spark, and Hive Metastore.
-   The Kudu component of E-MapReduce V3.22.0 is based on the open-source Apache Kudu 1.10.0. |
|OpenLDAP|-   The new component, OpenLDAP, is added to replace the previous component ApacheDS.
-   High availability is provided. |

## Updates

|Component|Description|
|---------|-----------|
|JindoFS|-   Provides two storage modes:
    -   Block mode: JindoFS uses Object Storage Service \(OSS\) as the backend storage. In the block storage mode, JindoFS stores data as blocks in OSS and uses Namespace Service to maintain metadata. The block mode provides better performance than the cache mode when you read and write data or query metadata. The block mode supports multiple storage policies, including WARM, COLD, HOT, TEMP, and ALL\_HDD. The WARM policy stores one local backup and one backup in OSS. The COLD policy stores only one backup in OSS. The HOT policy stores multiple local backups and one backup in OSS. The TEMP policy stores only one local backup. The ALL\_HDD policy stores multiple local backups. The default storage policy is WARM. You can set different storage policies for directories as required.
    -   Cache mode: This mode is compatible with OSS. In the cache mode, JindoFS stores data files as objects in OSS. When you access OSS objects stored in JindoFS, JindoFS can cache data and metadata of these OSS objects in the local cluster so that you can quickly access them next time. The cache mode provides multiple policies for you to synchronize metadata as required.
-   Offers an external client:
    -   JindoFS provides an external client so that you can access JindoFS from the outside of an E-MapReduce cluster. You can use the external client to access namespaces in the block mode. However, you cannot use the external client to access the data cached by JindoFS in E-MapReduce clusters. In addition, the performance of accessing data through the external client is worse than that of data access within E-MapReduce clusters.
    -   The cache mode is compatible with the existing OSS semantics. JindoFS accelerates data caching in E-MapReduce clusters. Therefore, you can use the OSS client to directly access JindoFS from the outside of E-MapReduce clusters. For example, you can use the OSS SDK or OssFileSystem of E-MapReduce to access JindoFS from the outside of E-MapReduce clusters.
-   Supports multiple ecosystem components:
    -   Currently, JindoFS supports various computing engines in E-MapReduce, such as Spark, Flink, Hive, MapReduce, Impala, and Presto.
    -   If you need to separate data computing from data storage, you can store data processing logs and storage logs in JindoFS, such as Spark event logs and YARN container logs.
    -   JindoFS can be used as the backend storage of HBase to store HFile files. This enhances the storage capability of HBase. |
|OssFileSystem|-   Added the feature of automatically detecting bad disks. This feature can resolve cache write failures caused by bad disks when you write data to OSS.
-   Completed the configurations of OssFileSystem. |
|Bigboot|-   Upgraded Bigboot to version 2.0.0.
-   Supports multiple namespaces, OSS, multiple storage modes, and access from external clients.
-   Fixed the issue where the Bigboot monitor status is abnormal during server restart.
-   Improved the service specifications of Kudu.
-   Supports a validity check for the specification of each component. |
|Hadoop|-   HDFS
    -   Adapted to HDFS Federation. You can create an HDFS Federation cluster through custom configurations or API operations to avoid formatting namenodes more than one time.
    -   Optimized the logic for detecting bad disks. If you use a local disk, the system detects the bad disk when the datanode blockreport is triggered by the dfsadmin command.
-   YARN

Fixed the issue where the list of history MapReduce jobs is not updated when YARN container logs are stored in JindoFS or JindoFS-based OssFileSystem. |
|Spark|-   Relational cache

Supports using a relational cache to accelerate data queries through pre-computing. You can create a relational cache to pre-compute data. During a data query, Spark Optimizer automatically detects an appropriate relational cache, optimizes the SQL execution plan, and continues data computing based on the relational cache. This accelerates data queries. For example, you can use relational caches to implement multidimensional online analytical processing \(MOLAP\), generate data reports, create data dashboards, and synchronize data across clusters.

    -   Supports using DDL to perform operations such as CACHE, UNCACHE, ALTER, and SHOW. A relational cache supports all data sources and data formats of Spark.
    -   Supports updating caches automatically or by using the REFRESH command. Supports incremental caching based on the specified partitions.
    -   Supports optimizing the SQL execution plan based on a relational cache.
-   Streaming SQL
    -   Normalized the parameter settings of Stream Query Writer.
    -   Optimized the schema compatibility check of Kafka data tables.
    -   Supports automatically registering a schema with Schema Registry for a Kafka data table that does not have a schema.
    -   Optimized log information recorded when a Kafka schema is incompatible.
    -   Fixed the issue where the column name must be explicitly specified when the query result is written to a Kafka data table.
    -   Removed the restriction that streaming SQL queries only support the Kafka and LogHub data sources.
-   Delta

Added the Delta component. You can use Spark to create a Delta data source to perform streaming data writing, transactional reading and writing, data verification, and data backtracking. For more information, see [Delta details](https://github.com/delta-io/delta).

    -   You can call the DataFrame API to read data from or write data to Delta.
    -   You can call the Structured Streaming API to read or write data by using Delta as the data source or sink.
    -   You can call the Delta API to update, delete, merge, vacuum, and optimize data.
    -   You can use SQL statements to create Delta tables, import data to Delta, and read data from Delta tables.
-   Others
    -   Supports primary keys and foreign keys. This is a constraint feature.
    -   Resolved JAR conflicts such as the servlet conflict. |
|Flink|Supports the log rollback feature of Log4j.|
|Kafka|-   Supports the log rollback feature of Log4j.
-   Upgraded Fastjson. |
|Zeppelin|Upgraded the dependent commons-lang3 package to version 3.7 to fix the issue where PySpark cannot write data to OSS. For more information, see [Spark 2.4 incompatibility with commons-lang3 in Zeppelin](https://issues.apache.org/jira/browse/ZEPPELIN-3939).|
|Ranger|Supports the SHOW GRANTS command.|
|Analytics-Zoo|Fixed the NumPy installation error.|
|Impala|Supports Apache Kudu 1.10.0.|
|Presto|Upgraded Presto to version 0.221.|
|ZooKeeper|Upgraded ZooKeeper to version 3.5.5.|

