# Release notes of EMR V3.28.X

This topic describes the release notes of E-MapReduce \(EMR\) V3.28.X, including the release date, new features, and updates.

## Release date

June 12, 2020 for EMR V3.28.0

## New features

|Component|Description|
|---------|-----------|
|Bigboot|-   JindoTable is released for data statistics based on the popularity of tables or partitions.
-   Complete storage policies in block storage mode are supported. Tiered storage policies, including the IA and Archive storage classes, are supported.
-   The data migration tool Jindo DistCp is used.
-   The performance of Jindo Fuse is improved and issues related to Jindo Fuse are fixed.
-   The integration of JFS Scheme on Jindo Job Committer and a Hive engine is optimized in cache mode.
-   The weight for reading OSS data in block storage mode is configured to alleviate the pressure on reading locally cached data.
-   JindoFS is divided into the following software modules: Bigboot \(management layer\), SmartData \(distributed service\), and JindoFS SDK. These modules are independently updated and maintained. |

## Updates

|Component|Description|
|---------|-----------|
|Flink|Open source Flink is upgraded to the enterprise-level Ververica Platform. Ververica Platform is customized based on open source Flink 1.10 and provides value-added features such as self-developed storage engine Gemini.|
|Bigboot|Bigboot is updated to 2.7.0.|
|Delta|-   Delta is updated to 0.6.0.
-   Delta code is decoupled from Spark code. |
|Spark|-   Spark is updated to 2.4.5.
-   Spark is compatible with the Streaming SQL scripts of DataFactory.
-   Delta 0.6.0 is supported. |
|Hive|Delta 0.6.0 is supported.|
|Ranger|-   Custom deployment of HDFS, Hive, and Spark is supported.
-   You can configure parameters on the ranger-admin-site and ranger-ugsync-site tabs for the Ranger service in the EMR console. |
|HDFS|If no DataNode is available when you write data to HDFS, the relevant DataNode exception information \(HDFS-9023\) is displayed.|
|Hue|-   Hue can be deployed on Gateway clusters.
-   Multiple Hue instances can be deployed on a single node. |
|DataFactory|Delta 0.6.0 is supported.|
|Druid|Druid is updated to 0.18.0.|
|Knox|-   Knox is updated to a version that ranges from 1.0.7 to 1.1.0.
-   Information on the web UI of HBase can be viewed. |

