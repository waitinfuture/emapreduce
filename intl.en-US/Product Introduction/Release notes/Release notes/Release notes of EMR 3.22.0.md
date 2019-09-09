# Release notes of EMR 3.22.0 {#concept_2039303 .concept}

This topic provides an overview of EMR 3.22.0 version that has been released, including the release dates, component upgrades, new features, and updates.

## Release date {#section_6yd_tfl_1zf .section}

July 28, 2019

## New features {#section_qka_es6_zja .section}

-   Kudu
    -   The new component, Kudu, is added. Kudu fills in the gaps in the Hadoop ecosystem. It provides HBase-like fast data insertion and random access, and allows users to modify data. It also provides ultra-large-scale data analysis and query capabilities similar to querying Parquet files on HDFS.
        -   Kudu provides C++ and Java APIs for secondary development.
        -   Kudu supports integration of Impala, Spark, and Hive Metastore.
    -   The Kudu component of EMR-3.22.0 is based on the open-source Apache Kudu 1.10.0.
-   OpenLDAP
    -   The new component, OpenLDAP, is added to replace the previous component ApacheDS.
    -   High availability is provided.

## Component upgrades {#section_af2_di3_3x6 .section}

-   ZooKeeper

    Upgraded to version 3.5.5.

-   Presto

    Upgraded to version 0.221.

-   Bigboot

    Upgraded to version 2.0.0


## Updates {#section_jhb_s63_oyk .section}

-   JindoFileSystem
    -   Multiple storage modes
        -   Block mode: Data is stored in blocks in the backend Object Storage Service \(OSS\). Local namespaces are used to maintain metadata. The block mode provides better metadata and data performance than the cache mode. The block mode provides multiple storage policies, including WARM \(one local replica plus one replica in OSS\), COLD \(only one replica in OSS\), HOT \(multiple local replicas plus one replica in OSS\), TEMP \(only one local replica\), and ALL\_HDD \(multiple local replicas\). The default storage policy is WARM. You can set different storage policies for directories in different scenarios.
        -   Cache mode: This mode is compatible with the existing data storage pattern of OSS. In the cache mode, files are stored as objects in OSS. The data and metadata of each file are cached locally based on the actual situations to improve data and metadata access performance. The cache mode provides different metadata synchronization policies to meet the requirements of different scenarios.
    -   External clients
        -   The client SDK provides access to E-MapReduce JindoFileSystem from outside E-MapReduce clusters. You can use the external client to access namespaces in the block mode. However, you cannot use the external client to access the data cached by JindoFileSystem in E-MapReduce clusters. In addition, the performance of accessing data through the external client is worse than that of data access within E-MapReduce clusters.
        -   The cache mode is compatible with the existing semantics of OSS storage. JindoFileSystem accelerates data caching in E-MapReduce clusters. Therefore, you can use the OSS client to directly access data from outside E-MapReduce clusters. For example, you can use the OSS SDK or OSSFileSystem of E-MapReduce to gain external access to the data.
    -   Ecosystem components
        -   Currently, JindoFileSystem supports various compute engines on E-MapReduce, such as Spark, Flink, Hive, MapReduce, Impala, and Presto.
        -   If you need to separate data computing from data storage, you can store job logs, such as YARN container logs and Spark event logs, on JindoFileSystem.
        -   JindoFileSystem can be used as the HFile backend storage of HBase to enhance the storage capabilities of HBase.
-   OSSFileSystem
    -   Adds the feature of automatically detecting bad disks. This feature can fix cache write failures caused by bad disks when you write data to OSS.
    -   Completes the configurations of OSSFileSystem.
-   Bigboot
    -   Supports multiple namespaces, local data storage in blocks, multiple storage modes, and access from external clients.
    -   Fixes the issue where the Bigboot monitor status is abnormal during server restart.
    -   Improves the service specifications of Kudu.
    -   Supports a validity check for the specification of each service.
-   Hadoop
    -   HDFS
        -   Adapts to HDFS Federation. You can create an HDFS Federation cluster through custom configurations or APIs to avoid formatting the cluster a second time.
        -   Optimizes the bad disk detection logic. If you are using a local disk, the system can perform bad disk detection when DataNode BR is triggered by the dfsadmin command.
    -   YARN
        -   Fixes the issue where the MapReduce job history list is not updated when MapReduce job container logs are stored on JindoFileSystem or OSS.
-   Spark
    -   Relational cache
        -   Supports using a relational cache to accelerate data queries through pre-computing. You can create a relational cache to pre-compute data. During a data query, Spark Optimizer automatically discovers an appropriate relational cache, optimizes the SQL execution plan, and continues data computing based on the relational cache. This improves the query speed and is suitable for various scenarios, such as reports, dashboards, data synchronization, and multidimensional analysis.
            -   Supports using the data definition language \(DDL\) to perform operations such as CACHE, UNCACHE, ALTER, and SHOW. A relational cache supports all data sources and data formats of Spark.
            -   Supports automatic data updates, manual data updates by the REFRESH command, and partition-based incremental updates of a relational cache.
            -   Supports optimizing the SQL execution plan based on a relational cache.
    -   Streaming SQL
        -   Normalizes the parameter settings of Stream Query Writer.
        -   Optimizes the schema compatibility check of Kafka data tables.
        -   Automatically registers a schema with SchemaRegistry for a Kafka data table that does not have a schema.
        -   Optimizes log information recorded when a Kafka schema is incompatible.
        -   Fixes the issue where the column name must be explicitly specified when the query result is written to a Kafka data table.
        -   Removes the restriction that streaming SQL queries only support the Kafka and LogHub data sources.
    -   Delta
        -   Adds the Delta component. For more information about Delta, click [here](https://github.com/delta-io/delta). You can use Spark to create a Delta data source to perform streaming data writing, transactional reading and writing, data verification, and data backtracking.
            -   You can call the dataframe API operation to read data from or write data to Delta.
            -   You can call the structure streaming API operation to read or write data by using Delta as the data source or sink.
            -   You can call Delta API operations to update, delete, merge, vacuum, and optimize data.
            -   You can use SQL statements to create Delta tables, import data to Delta, and read data from Delta tables.
    -   Others
        -   \(Constraint feature\) Supports primary keys and foreign keys.
        -   Resolves JAR conflicts such as the servlet conflict
-   Flink
    -   Supports Log4j log rotation.
-   Kafka
    -   Supports Log4j log rotation.
    -   Upgrades Fastjson.
-   Zeppelin
    -   Upgrades the dependent commons-lang3 package to version 3.7 to fix the issue where PySpark cannot write data to OSS. For more information, click [here](https://issues.apache.org/jira/browse/ZEPPELIN-3939).
-   Ranger
    -   Supports the SHOW GRANTS command.
-   Analytics-zoo
    -   Fixes the Numpy installation error.
-   Impala
    -   Compatible with Apache Kudu 1.10.0.

