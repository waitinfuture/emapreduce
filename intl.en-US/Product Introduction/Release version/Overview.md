# Overview

This topic describes the following information about released versions of E-MapReduce \(EMR\): version format, version upgrade records, and release notes.

## Version format

EMR versions follow the EMR-**a.b.c** format, where:

-   a indicates that major changes are made in a new version.
-   b indicates that changes are made on some components in a new version.
-   c indicates that several bugs are fixed in a new version and the version is forward compatible.

The following example describes the version number changes in a version upgrade:

-   Upgrade from version 1.0.0 to 2.0.0: The value of a changes, which indicates that major changes are made in the new version. You must conduct a test to verify that all existing jobs can run properly after the upgrade.
-   Upgrade from version 1.0.0 to 1.1.0: The value of b changes, which indicates that minor changes are made on some components in the new version. Most features remain unchanged after the upgrade. You must conduct a test to verify that all existing jobs can run properly after the upgrade.
-   Upgrade from version 1.0.0 to 1.0.1: The value of c changes, which indicates that the new version is backward compatible with the previous version.

The following content describes the bundled software, cluster creation, and cluster upgrade of each EMR version:

-   Bundled software: The software and software version are fixed in each EMR version. We recommend that you do not change the software version.
-   Cluster creation: After you select an EMR version and create a cluster of this version, the cluster version is not automatically upgraded. Subsequent upgrades to the image used by this version do not affect the existing clusters. The new version of the image is used to create new clusters.
-   Cluster upgrade: When you upgrade the version of your cluster, for example, from version 1.0.X to 1.1.X, you must conduct a test to verify that the existing jobs in the cluster can run properly in the new software environment.

## Version upgrade records \(EMR V4.X series\)

[EMR-4.3.0~EMR-4.4.1](#section_82p_pat_zzm)

## Version upgrade records \(EMR V3.X series\)

-   [EMR V3.28.2-EMR V3.30.0](#section_wf1_meg_v4o)
-   [EMR V3.25.1-EMR V3.28.1](#section_9d1_8y5_i0c)
-   [EMR V3.22.1-EMR V3.25.0](#section_0ix_02u_ei7)
-   [Version upgrade records \(earlier than EMR V3.22.X\)](#section_ol8_d4n_psz)

## EMR V4.3.0-EMR V4.4.1

|Version|EMR V4.4.1|EMR V4.4.0|EMR V4.3.0|
|-------|----------|----------|----------|
|Release date|2020.8|2020.7|2020.5|
|HDFS|3.1.3|3.1.3|3.1.3|
|YARN|3.1.3|3.1.3|3.1.3|
|Hive|3.1.2|3.1.2|3.1.1|
|Spark|2.4.5|2.4.5|2.4.5|
|Knox|1.1.0|1.1.0|1.1.0|
|Tez|0.9.2|0.9.2|0.9.2|
|Ganglia|3.7.2|3.7.2|3.7.2|
|Sqoop|1.4.7|1.4.7|1.4.7|
|SmartData|2.7.301|2.7.2|2.6.7|
|Bigboot|2.7.301|2.7.2|2.6.7|
|OpenLDAP|2.4.44|2.4.44|2.4.44|
|Hue|4.4.0|4.4.0|4.4.0|
|HBase|2.1.9|2.1.9|2.1.9|
|ZooKeeper|3.5.6|3.5.6|3.5.6|
|Presto|331|331|331|
|Impala|3.4.0|3.4.0|3.2.0|
|Zeppelin|0.8.1|0.8.1|0.8.1|
|Pig|0.14.0|0.14.0|0.14.0|
|Flume|1.9.0|1.9.0|1.9.0|
|Livy|0.6.0|0.6.0|0.6.0|
|Superset|0.36.0|0.36.0|0.28.1|
|Ranger|2.0.0|2.0.0|2.0.0|
|Flink|1.10-vvr-1.0.2|1.10-vvr-1.0.2|1.10-vvr-1.0.2|
|Storm|1.2.2|1.2.2|1.2.2|
|Kudu|1.11.1|1.11.1|1.11.1|
|Oozie|5.1.0|5.1.0|5.1.0|
|Kafka|2.4.1|2.4.1|2.4.1|
|Kafka-Manager|1.3.3.16|1.3.3.16|1.3.3.16|

## EMR V3.28.2-EMR V3.30.0

|Version|EMR V3.30.0|EMR V3.29.0|EMR V3.28.2|
|:------|-----------|-----------|-----------|
|Release date|2020.6|2020.6|2020.5|
|Hadoop|2.8.5|2.8.5|2.8.5|
|Knox|1.1.0|1.1.0|1.1.0|
|Spark|2.4.5|2.4.5|2.4.5|
|Hive|2.3.5|2.3.5|2.3.5|
|Tez|0.9.2|0.9.2|0.9.2|
|Pig|0.14.0|0.14.0|0.14.0|
|Sqoop|1.4.7|1.4.7|1.4.7|
|YARN|2.8.5|2.8.5|2.8.5|
|HDFS|2.8.5|2.8.5|2.8.5|
|Flink|1.11-vvr-2.0.1|1.10-vvr-1.0.4|1.10-vvr-1.0.4|
|Druid|0.18.0|0.18.0|0.17.1|
|HBase|1.4.9|1.4.9|1.4.9|
|Phoenix|4.14.1|4.14.1|4.14.1|
|ZooKeeper|3.5.6|3.5.6|3.5.6|
|Livy|0.6.0|0.6.0|0.6.0|
|Presto|338|338|331|
|Storm|1.2.2|1.2.2|1.2.2|
|Impala|2.12.2|2.12.2|2.12.2|
|Flume|1.9.0|1.9.0|1.9.0|
|Hue|4.4.0|4.4.0|4.4.0|
|Oozie|5.1.0|5.1.0|5.1.0|
|Zeppelin|0.8.1|0.8.1|0.8.1|
|Ranger|1.2.0|1.2.0|1.2.0|
|Ganglia|3.7.2|3.7.2|3.7.2|
|SmartData|3.0.0|2.7.301|2.7.2|
|Kafka|1.1.1|1.1.1|1.1.1|
|Kafka-Manager|1.3.3.16|1.3.3.16|1.3.3.16|
|Superset|0.36.0|0.35.2|0.35.2|
|Bigboot|3.0.0|2.7.301|2.7.2|
|OpenLDAP|2.4.44|2.4.44|2.4.44|
|Kudu|1.10.0|1.10.0|1.10.0|
|Flink-Vvp|1.11-2.2.2|1.11-2.2.2|1.11-2.2.2|
|PAI-Alink|1.1.0|1.1.0|1.1.0|

## EMR V3.25.1-EMR V3.28.1

|Version|EMR V3.28.1|EMR V3.28.0|EMR V3.27.2|EMR V3.26.3|EMR V3.25.1|
|:------|-----------|-----------|-----------|-----------|-----------|
|Release date|2020.6|2020.6|2020.5|2020.4|2020.1|
|Hadoop|2.8.5|2.8.5|2.8.5|2.8.5|2.8.5|
|Knox|1.1.0|1.1.0|1.1.0|1.1.0|1.1.0|
|Spark|2.4.5|2.4.5|2.4.3|2.4.3|2.4.3|
|Hive|2.3.5|2.3.5|2.3.5|2.3.5|2.3.5|
|Tez|0.9.2|0.9.2|0.9.2|0.9.2|0.9.2|
|Pig|0.14.0|0.14.0|0.14.0|0.14.0|0.14.0|
|Sqoop|1.4.7|1.4.7|1.4.7|1.4.7|1.4.7|
|YARN|2.8.5|2.8.5|2.8.5|2.8.5|2.8.5|
|HDFS|2.8.5|2.8.5|2.8.5|2.8.5|2.8.5|
|Flink|1.10-vvr-1.0.2|1.10-vvr-1.0.2|1.9.1|1.9.1|1.9.1|
|Druid|0.18.0|0.18.0|0.17.1|0.16.0|0.16.0|
|HBase|1.4.9|1.4.9|1.4.9|1.4.9|1.4.9|
|Phoenix|4.14.1|4.14.1|4.14.1|4.14.1|4.14.1|
|ZooKeeper|3.5.6|3.5.6|3.5.6|3.5.6|3.5.6|
|Livy|0.6.0|0.6.0|0.6.0|0.6.0|0.6.0|
|Presto|331|331|331|310|310|
|Storm|1.2.2|1.2.2|1.2.2|1.2.2|1.2.2|
|Impala|2.12.2|2.12.2|2.12.2|2.12.2|2.12.2|
|Flume|1.9.0|1.9.0|1.9.0|1.9.0|1.9.0|
|Hue|4.4.0|4.4.0|4.4.0|4.4.0|4.4.0|
|Oozie|5.1.0|5.1.0|5.1.0|5.1.0|5.1.0|
|Zeppelin|0.8.1|0.8.1|0.8.1|0.8.1|0.8.1|
|Ranger|1.2.0|1.2.0|1.2.0|1.2.0|1.2.0|
|Ganglia|3.7.2|3.7.2|3.7.2|3.7.2|3.7.2|
|SmartData|2.7.1|2.7.0|2.6.5|2.6.3|2.2.3|
|Kafka|1.1.1|1.1.1|1.1.1|1.1.1|1.1.1|
|Kafka-Manager|1.3.3.16|1.3.3.16|1.3.3.16|1.3.3.16|1.3.3.16|
|Superset|0.35.2|0.35.2|0.35.2|0.28.1|0.28.1|
|Bigboot|2.7.1|2.7.0|2.6.5|2.6.3|2.2.3|
|OpenLDAP|2.4.44|2.4.44|2.4.44|2.4.44|2.4.44|
|Kudu|1.10.0|1.10.0|1.10.0|1.10.0|1.10.0|
|Analytics Zoo|-|-|-|0.5.0|0.5.0|
|TensorFlow on Spark|-|-|-|1.0.1|1.0.1|

## EMR V3.22.1-EMR V3.25.0

|Version|EMR V3.25.0|EMR V3.24.3|EMR V3.24.0|EMR V3.23.0|EMR V3.22.1|
|:------|-----------|-----------|-----------|-----------|-----------|
|Release date|2020.1|2019.11|2019.11|2019.9|2019.7|
|Hadoop|2.8.5|2.8.5|2.8.5|2.8.5|2.8.5|
|Knox|1.1.0|1.1.0|1.1.0|1.1.0|1.1.0|
|Spark|2.4.3|2.4.3|2.3.5|2.4.3|2.4.3|
|Hive|2.3.5|2.3.5|2.3.5|2.3.5|2.3.5|
|Tez|0.9.2|0.9.1|0.9.1|0.9.1|0.9.1|
|Pig|0.14.0|0.14.0|0.14.0|0.14.0|0.14.0|
|Sqoop|1.4.7|1.4.7|1.4.7|1.4.7|1.4.7|
|YARN|2.8.5|2.8.5|2.8.5|2.8.5|2.8.5|
|HDFS|2.8.5|2.8.5|2.8.5|2.8.5|2.8.5|
|Flink|1.9.1|1.9.1|1.9.1|1.8.2|1.7.2|
|Druid|0.16.0|0.16.0|0.16.0|0.15.1|0.14.2|
|HBase|1.4.9|1.4.9|1.4.9|1.4.9|1.4.9|
|Phoenix|4.14.1|4.14.1|4.14.1|4.14.1|4.14.1|
|ZooKeeper|3.5.6|3.5.5|3.5.5|3.5.5|3.5.5|
|Livy|0.6.0|0.6.0|0.6.0|0.6.0|0.6.0|
|Presto|310|0.228|0.228|0.221|0.221|
|Storm|1.2.2|1.2.2|1.2.2|1.2.2|1.2.2|
|Impala|2.12.2|2.12.2|2.12.2|2.12.2|2.12.2|
|Flume|1.9.0|1.9.0|1.9.0|1.9.0|1.9.0|
|Hue|4.4.0|4.4.0|4.4.0|4.4.0|4.4.0|
|Oozie|5.1.1|5.1.0|5.1.0|5.1.0|5.1.0|
|Zeppelin|0.8.1|0.8.1|0.8.1|0.8.1|0.8.1|
|Ranger|1.2.0|1.2.0|1.2.0|1.2.0|1.2.0|
|Ganglia|3.7.2|3.7.2|3.7.2|3.7.2|3.7.2|
|TensorFlow on Spark|1.0.1|1.0.1|1.0.1|1.0.0|1.0.0|
|Analytics Zoo|0.5.0|0.5.0|-|0.5.0|0.5.0|
|Kafka|1.1.1|1.1.1|1.1.1|1.1.1|1.1.1|
|Kafka-Manager|1.3.3.16|1.3.3.16|1.3.3.16|1.3.3.16|1.3.3.16|
|Superset|0.28.1|0.28.1|0.28.1|0.28.1|0.28.1|
|Bigboot|2.2.3|2.2.2|2.2.1|2.0.2|2.0.1|
|SmartData|2.2.3|2.2.2|2.2.1|2.0.2|2.0.1|
|OpenLDAP|2.4.44|2.4.44|2.4.44|2.4.44|2.4.44|
|Kudu|1.10.0|1.10.0|1.10.0|1.10.0|1.10.0|

## Version upgrade records \(earlier than EMR V3.22.X\)

-   **EMR V3.19.0-EMR V3.21.0**

    |Version|EMR V3.19.0|EMR V3.19.1|EMR V3.20.0|EMR V3.21.0|
    |:------|-----------|-----------|-----------|-----------|
    |Release date|2019.3|2019.4|2019.5|2019.6|
    |Hadoop|2.8.5|2.8.5|2.8.5|2.8.5|
    |Knox|1.1.0|1.1.0|1.1.0|1.1.0|
    |ApacheDS|2.0.0|2.0.0|2.0.0|2.0.0|
    |Spark|2.4.1|2.4.1|2.4.2|2.4.3|
    |Hive|3.1.1|3.1.1|3.1.1|3.1.1|
    |Tez|0.9.1|0.9.1|0.9.1|0.9.1|
    |Pig|0.14.0|0.14.0|0.14.0|0.14.0|
    |Sqoop|1.4.7|1.4.7|1.4.7|1.4.7|
    |YARN|2.8.5|2.8.5|2.8.5|2.8.5|
    |HDFS|2.8.5|2.8.5|2.8.5|2.8.5|
    |Flink|1.7.2|1.7.2|1.7.2|1.7.2|
    |Druid|0.13.0|0.13.0|0.13.0|0.14.2|
    |HBase|1.4.9|1.4.9|1.4.9|1.4.9|
    |Phoenix|4.14.1|4.14.1|4.14.1|4.14.1|
    |ZooKeeper|3.4.13|3.4.13|3.4.13|3.4.13|
    |Livy|0.6.0|0.6.0|0.6.0|0.6.0|
    |Presto|0.213|0.213|0.213|0.213|
    |Storm|1.2.2|1.2.2|1.2.2|1.2.2|
    |Impala|2.12.2|2.12.2|2.12.2|2.12.2|
    |Flume|1.8.0|1.8.0|1.8.0|1.8.0|
    |Hue|4.1.0|4.1.0|4.1.0|4.4.0|
    |Oozie|4.2.0|4.2.0|5.1.0|5.1.0|
    |Zeppelin|0.8.0|0.8.0|0.8.1|0.8.1|
    |Ranger|1.2.0|1.2.0|1.2.0|1.2.0|
    |Ganglia|3.7.2|3.7.2|3.7.2|3.7.2|
    |OS|CentOS 7.4|CentOS 7.4|CentOS 7.4|CentOS 7.4|
    |TensorFlow|-|-|1.8.0|1.8.0|
    |Kafka|2.11-1.1.1|1.1.1|2.11|1.1.1|
    |Superset|0.28.1|0.28.1|0.28.1|0.28.1|
    |Jupyter|-|-|4.4.0|4.4.0|
    |Analytics Zoo|-|-|0.2.0|0.5.0|
    |Bigboot|-|-|1.0.0|1.0.0|
    |OpenLDAP|-|-|-|-|
    |Kudu|-|-|-|-|

-   **EMR V3.15.0-EMR V3.18.1**

    |Version|EMR V3.15.0|EMR V3.16.0|EMR V3.17.0|EMR V3.18.1|
    |:------|-----------|-----------|-----------|-----------|
    |Release date|2018.11|2018.12|2019.1|2019.2|
    |Hadoop|2.7.2|2.7.2-1.3.2|2.7.2|2.8.5|
    |Knox|0.13.0|0.13.0|1.1.0|1.1.0|
    |ApacheDS|2.0.0|2.0.0|2.0.0|2.0.0|
    |Spark|2.3.2|2.3.2-1.0.1|2.3.2|2.3.2|
    |Hive|2.3.3|2.3.3-1.0.3|2.3.3|3.1.1|
    |Tez|0.9.1|0.9.1-1.0.2|0.9.1|0.9.1|
    |Pig|0.14.0|0.14.0|0.14.0|0.14.0|
    |Sqoop|1.4.7|1.4.7-1.0.0|1.4.7|1.4.7|
    |YARN|2.7.2|2.7.2|2.7.2|2.8.5|
    |HDFS|2.7.2|2.7.2|2.7.2|2.8.5|
    |Flink|1.4.0|1.6.2-1.0.0|1.6.2|1.6.2|
    |Druid|0.12.3|0.12.3-1.0.1|0.12.3|0.13.0|
    |HBase|1.1.1|1.1.1-1.0.2|1.1.1|1.4.9|
    |Phoenix|4.10.0|4.10.0-1.0.0|4.10.0|4.14.1|
    |ZooKeeper|3.4.13|3.4.13|3.4.13|3.4.13|
    |Livy|0.50.|0.50.|0.50.|0.60.|
    |Presto|0.208|0.208|0.213|0.213|
    |Storm|1.1.2|1.2.2|1.2.2|1.2.2|
    |Impala|2.10.0|2.10.0-1.0.0|2.12.2|2.12.2|
    |Flume|-|1.8.0|1.8.0|1.8.0|
    |Hue|4.1.0|4.1.0|4.1.0|4.1.0|
    |Oozie|4.2.0|4.2.0|4.2.0|4.2.0|
    |Zeppelin|0.8.0|0.8.0|0.8.0|0.8.0|
    |Ranger|1.0.0|1.0.0|1.0.0|1.0.0|
    |Ganglia|3.7.2|3.7.2|3.7.2|3.7.2|
    |OS|CentOS 7.4|CentOS 7.4|CentOS 7.4|CentOS 7.4|
    |TensorFlow|1.8.0|1.8.0|1.8.0|-|
    |Kafka|2.11-1.0.1|2.11-1.1.0|2.11-1.1.1|2.11-1.1.1|
    |Superset|0.27.0|0.28.1|0.28.1|0.28.1|
    |Jupyter|4.4.0|4.4.0|4.4.0|-|
    |Analytics Zoo|0.2.0|0.2.0|0.2.0|-|

-   **EMR V3.11.0-EMR V3.14.0**

    |Version|EMR V3.11.0|EMR V3.12.0|EMR V3.13.0|EMR V3.14.0|
    |:------|:----------|:----------|-----------|-----------|
    |Release date|2018.6|2018.7|2018.8|2018.10|
    |Hadoop|2.7.2-emr-1.2.14|2.7.2-emr-1.2.14|2.7.2|2.7.2|
    |Knox|0.13.0|0.13.0|0.13.0|0.13.0|
    |ApacheDS|2.0.0|2.0.0|2.0.0|2.0.0|
    |Spark|2.2.1|2.3.1|2.3.1|2.3.1|
    |Hive|2.3.3|2.3.3|2.3.3|2.3.3|
    |Tez|0.9.1|0.9.1|0.9.1|0.9.1|
    |Pig|0.14.0|0.14.0|0.14.0|0.14.0|
    |Sqoop|1.4.6|1.4.7|1.4.7|1.4.7|
    |YARN|2.7.2|2.7.2|2.7.2|2.7.2|
    |HDFS|2.7.2|2.7.2|2.7.2|2.7.2|
    |Flink|1.4.0|1.4.0|1.4.0|1.4.0|
    |Druid|0.11.0|0.12.0|0.12.2|0.12.3|
    |HBase|1.1.1|1.1.1|1.1.1|1.1.1|
    |Phoenix|4.10.0|4.10.0|4.10.0|4.10.0|
    |ZooKeeper|3.4.11|3.4.12|3.4.12|3.4.13|
    |Livy|-|-|-|-|
    |Presto|0.188|0.188|0.208|0.208|
    |Storm|1.1.2|1.1.2|1.1.2|1.1.2|
    |Impala|2.10.0|2.10.0|2.10.0|2.10.0|
    |Flume|-|-|-|-|
    |Hue|4.1.0|4.1.0|4.1.0|4.1.0|
    |Oozie|4.2.0|4.2.0|4.2.0|4.2.0|
    |Zeppelin|0.7.3|0.7.3|0.8.0|0.8.0|
    |Ranger|0.7.3|1.0.0|1.0.0|1.0.0|
    |Ganglia|3.7.2|3.7.2|3.7.2|3.7.2|
    |OS|CentOS 7.4|CentOS 7.4|CentOS 7.4|CentOS 7.4|
    |TensorFlow|-|-|1.8.0|1.8.0|
    |Kafka|-|-|2.11-1.0.1|2.11-1.0.1|
    |Superset|-|-|0.25.6|0.25.6|
    |Jupyter|-|-|-|-|
    |Analytics Zoo|-|-|-|-|

-   **EMR V3.7.1-EMR V3.10.1**

    |Version|EMR V3.7.1|EMR V3.8.1|EMR V3.9.1|EMR V3.10.1|
    |:------|:---------|:---------|----------|-----------|
    |Release date|2018.1|2018.1|2018.2|2018.4|
    |Hadoop|2.7.2-emr-1.2.10|2.7.2-emr-1.2.12|2.7.2-emr-1.2.13|2.7.2-emr-1.2.14|
    |Knox|0.13.0|0.13.0|0.13.0|0.13.0|
    |ApacheDS|2.0.0|2.0.0|2.0.0|2.0.0|
    |Spark|2.2.1|2.2.1|2.2.1|2.2.1|
    |Hive|2.3.2|2.3.2|2.3.2|2.3.2|
    |Tez|0.8.4|0.8.4|0.8.4|0.9.1|
    |Pig|0.14.0|0.14.0|0.14.0|0.14.0|
    |Sqoop|1.4.6|1.4.6|1.4.6|1.4.6|
    |YARN|2.7.2|2.7.2|2.7.2|2.7.2|
    |HDFS|2.7.2|2.7.2|2.7.2|2.7.2|
    |Flink|-|1.4.0|1.4.0|1.4.0|
    |Druid|-|-|0.11.0|0.11.0|
    |HBase|1.1.1|1.1.1|1.1.1|1.1.1|
    |Phoenix|4.10.0|4.10.0|4.10.0|4.10.0|
    |ZooKeeper|3.4.11|3.4.11|3.4.11|3.4.11|
    |Livy|-|-|-|-|
    |Presto|0.188|0.188|0.188|0.188|
    |Storm|1.0.1|1.0.1|1.0.1|1.1.2|
    |Impala|2.10.0|2.10.0|2.10.0|2.10.0|
    |Flume|-|-|-|-|
    |Hue|3.12.0|3.12.0|3.12.0|4.1.0|
    |Oozie|4.2.0|4.2.0|4.2.0|4.2.0|
    |Zeppelin|0.7.1|0.7.1|0.7.1|0.7.1|
    |Ranger|-|-|0.7.1|0.7.1|
    |Ganglia|3.7.2|3.7.2|3.7.2|3.7.2|
    |OS|CentOS 7.4|CentOS 7.4|CentOS 7.4|CentOS 7.4|
    |TensorFlow|-|-|-|-|
    |Kafka|-|-|-|-|
    |Superset|-|-|-|-|
    |Jupyter|-|-|-|-|
    |Analytics Zoo|-|-|-|-|

-   **EMR V2.9.2-EMR V2.11.0**

    |Version|EMR V2.9.2|EMR V2.10.0|EMR V2.11.0|
    |:------|:---------|:----------|:----------|
    |Release date|2018.2|2018.4|2018.7|
    |Hadoop|2.7.2-emr-1.2.12|2.7.2-emr-1.2.12|2.7.2-emr-1.2.12|
    |Spark|1.6.3|1.6.3|1.6.3|
    |Hive|2.3.2|2.3.2|2.3.3|
    |Tez|0.8.4|0.9.1|0.9.1|
    |Pig|0.14.0|0.14.0|0.14.0|
    |Sqoop|1.4.6|1.4.6|1.4.6|
    |Hue|3.12.0|4.1.0|4.1.0|
    |Zeppelin|0.7.1|0.7.1|0.7.3|
    |HBase|1.1.1|1.1.1|1.1.1|
    |Phoenix|4.10.0|4.10.0|4.10.0|
    |Storm|1.0.1|1.1.2|1.1.2|
    |Presto|0.188|0.188|0.188.0|
    |Impala|2.10.0|2.10.0|2.10.0|
    |ZooKeeper|3.4.6|3.4.11|3.4.11|
    |Oozie|4.2.0|4.2.0|4.2.0|
    |Ranger|0.7.1|0.7.1|0.7.3|
    |Ganglia|3.7.2|3.7.2|3.7.2|
    |OS|CentOS 7.4|CentOS 7.4|CentOS 7.4|

-   **EMR V1.0.0-EMR V1.3.0**

    |Version|EMR V1.0.0|EMR V1.1.0|EMR V1.2.0|EMR V1.3.0|
    |-------|----------|----------|----------|----------|
    |Release date|2015.11|2016.3|2016.4|2016.5|
    |Hadoop|2.6.0|2.6.0|2.6.0|2.6.0-emr-1.1.1|
    |Spark|1.4.1|1.6.0|1.6.1|1.6.1|
    |Hive|1.0.1|1.0.1|2.0.0|2.0.0|
    |Pig|0.14.0|0.14.0|0.14.0|0.14.0|
    |Sqoop|-|-|-|1.4.6|
    |Hue|-|-|-|3.9.0|
    |Zeppelin|-|-|-|0.5.6|
    |HBase|-|-|1.1.1|1.1.1|
    |Phoenix|-|-|-|-|
    |ZooKeeper|-|-|3.4.6|3.4.6|
    |Ganglia|3.7.2|3.7.2|3.7.2|3.7.2|


**Note:** About Hadoop versions:

To ensure compatibility with Alibaba Cloud Object Storage Service \(OSS\), the emr-core component is deployed based on an open source Hadoop version. Existing APIs remain unchanged. The version number of the emr-core component is suffixed to the end of the Hadoop version.

## Release notes

For more information about release notes of EMR versions, see the following topics:

-   EMR V4.X series
    -   [t1986742.md\#]()
    -   [t1986708.md\#]()
    -   [t1986707.md\#]()
-   EMR V3.X series
    -   [t1982393.md\#]()
    -   [Release notes of EMR V3.29.X](/intl.en-US/Product Introduction/Release version/Release notes/Release notes of EMR V3.29.X.md)
    -   [Release notes of EMR V3.28.X](/intl.en-US/Product Introduction/Release version/Release notes/Release notes of EMR V3.28.X.md)
    -   [Release notes of EMR V3.27.X](/intl.en-US/Product Introduction/Release version/Release notes/Release notes of EMR V3.27.X.md)
    -   [Release notes of EMR V3.26.X](/intl.en-US/Product Introduction/Release version/Release notes/Release notes of EMR V3.26.X.md)
    -   [Release notes of EMR V3.25.X](/intl.en-US/Product Introduction/Release version/Release notes/Release notes of EMR V3.25.X.md)
    -   [Release notes of EMR V3.24.X](/intl.en-US/Product Introduction/Release version/Release notes/Release notes of EMR V3.24.X.md)
    -   [Release notes of EMR V3.23.X](/intl.en-US/Product Introduction/Release version/Release notes/Release notes of EMR V3.23.X.md)
    -   [Release notes of EMR V3.22.X](/intl.en-US/Product Introduction/Release version/Release notes/Release notes of EMR V3.22.X.md)
    -   [Release notes of versions earlier than E-MapReduce V3.22.X](/intl.en-US/Product Introduction/Release version/Release notes/Release notes of versions earlier than E-MapReduce V3.22.X.md)

