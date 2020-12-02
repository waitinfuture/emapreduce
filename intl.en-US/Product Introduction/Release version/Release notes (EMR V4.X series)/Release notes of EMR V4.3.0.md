# Release notes of EMR V4.3.0

This topic describes the release notes of E-MapReduce \(EMR\) V4.3.0, including the release date and updates.

## Release date

May 20, 2020 for EMR V4.3.0

## Updates

|Component|Description|
|---------|-----------|
|Ranger|-   Custom deployment of Hadoop Distributed File System \(HDFS\), Hive, and Spark plug-ins is supported. You can enable plug-ins on required service nodes.
-   The RangerUserSync and RangerAdmin components can be configured in the EMR console. |
|Presto|The Kudu client is updated.|
|Spark|-   Spark is updated to 2.4.5.
-   The associated Delta Lake is updated to 0.6.0.
-   The issue that PySpark cannot properly run after Ranger Hive is enabled is fixed. |
|HDFS|-   The issue that the HDFS\_NAMENODE\_OPTS parameter does not take effect is fixed.
-   Custom deployment is supported. |
|YARN|Custom deployment is supported.|
|Hive|Custom deployment is supported.|
|Knox|Information on the web UI of HDFS NameNode in Hadoop 3.X can be viewed.|
|Zeppelin|The issue that a zepping.keytab file fails to be generated is fixed.|
|Kafka|Kafka is updated to 2.4.1.|
|Kudu|Kudu is updated to 1.11.1.|
|Impala|Issues related to HAProxy are fixed.|
|Livy|Issues related to xmllint are fixed.|
|Hue|-   Hue can be deployed on gateway clusters.
-   Multiple Hue instances can be enabled on a single node. |

