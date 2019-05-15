# Versioning {#concept_dtk_nj3_y2b .concept}

-   E-MapReduce follows an X.Y.Z format in its software version numbers:

    -   X indicates that major changes have been made in the version.
    -   Y indicates that minor changes have been made to some components in the version.
    -   Z indicates that while bugs have been fixed in the version, it is still compatible with previous versions.
    If you upgrade from version 1.0.0 to version 2.0.0, for example, a number of major changes will have been made. We therefore recommend verifying that all previous jobs can run normally. An update from version 1.0.0 to 1.1.0, however, is typically made to upgrade a component. We recommend performing a similar test to verify that jobs are running normally. An update from version 1.0.0 to version 1.0.1 is typically to perform some kind of maintenance, and it remains fully compatible with previous versions.

-   In each release of E-MapReduce, the software and software version are fixed. You cannot select multiple software versions, and changing the software version manually is not recommended.

-   If you select a version of E-MapReduce and use it in a cluster, it is not upgraded automatically. If the images corresponding to this version are upgraded, the cluster you created is not impacted. Only new clusters will use the new images.

-   When you upgrade the version of your cluster \(for example, from 1.0.x to 1.1.x\), we recommend performing a test to verify that your jobs are running normally in the new software environment.


## Release notes {#section_y1p_mtk_ngb .section}

-   3.x

    |版本|EMR-3.7.1|EMR-3.8.1|EMR-3.9.1|EMR-3.10.1|EMR-3.11.0|EMR-3.12.0|EMR-3.13.0|EMR-3.14.0|EMR-3.15.0|EMR-3.16.0|EMR-3.17.0|EMR-3.18.1|EMR-3.19.0|EMR-3.19.1|EMR-3.20.0|
    |:-|:--------|:--------|:--------|:---------|:---------|:---------|----------|----------|----------|----------|----------|----------|----------|----------|----------|
    |发布时间|2018.1|2018.1|2018.2|2018.4|2018.6|2018.7|2018.8|2018.10|2018.11|2018.12|2019.1|2019.2|2019.3|2019.4|2019.5|
    |Hadoop|2.7.2-emr-1.2.10|2.7.2-emr-1.2.12|2.7.2-emr-1.2.13|2.7.2-emr-1.2.14|2.7.2-emr-1.2.14|2.7.2-emr-1.2.14|2.7.2|2.7.2|2.7.2|2.7.2-1.3.2|2.7.2|2.8.5|2.8.5|2.8.5|2.8.5|
    |Knox|0.13.0|0.13.0|0.13.0|0.13.0|0.13.0|0.13.0|0.13.0|0.13.0|0.13.0|0.13.0|1.1.0|1.1.0|1.1.0|1.1.0|1.1.0|
    |ApacheDS|2.0.0|2.0.0|2.0.0|2.0.0|2.0.0|2.0.0|2.0.0|2.0.0|2.0.0|2.0.0|2.0.0|2.0.0|2.0.0|2.0.0|2.0.0|
    |Spark|2.2.1|2.2.1|2.2.1|2.2.1|2.2.1|2.3.1|2.3.1|2.3.1|2.3.2|2.3.2-1.0.1|2.3.2|2.3.2|2.4.1|2.4.1|2.4.2|
    |Hive|2.3.2|2.3.2|2.3.2|2.3.2|2.3.3|2.3.3|2.3.3|2.3.3|2.3.3|2.3.3-1.0.3|2.3.3|3.1.1|3.1.1|3.1.1|3.1.1|
    |Tez|0.8.4|0.8.4|0.8.4|0.9.1|0.9.1|0.9.1|0.9.1|0.9.1|0.9.1|0.9.1-1.0.2|0.9.1|0.9.1|0.9.1|0.9.1|0.9.1|
    |Pig|0.14.0|0.14.0|0.14.0|0.14.0|0.14.0|0.14.0|0.14.0|0.14.0|0.14.0|0.14.0|0.14.0|0.14.0|0.14.0|0.14.0|0.14.0|
    |Sqoop|1.4.6|1.4.6|1.4.6|1.4.6|1.4.6|1.4.7|1.4.7|1.4.7|1.4.7|1.4.7-1.0.0|1.4.7|1.4.7|1.4.7|1.4.7|1.4.7|
    |YARN|2.7.2|2.7.2|2.7.2|2.7.2|2.7.2|2.7.2|2.7.2|2.7.2|2.7.2|2.7.2|2.7.2|2.8.5|2.8.5|2.8.5|2.8.5|
    |HDFS|2.7.2|2.7.2|2.7.2|2.7.2|2.7.2|2.7.2|2.7.2|2.7.2|2.7.2|2.7.2|2.7.2|2.8.5|2.8.5|2.8.5|2.8.5|
    |Flink| |1.4.0|1.4.0|1.4.0|1.4.0|1.4.0|1.4.0|1.4.0|1.4.0|1.6.2-1.0.0|1.6.2|1.6.2|1.7.2|1.7.2|1.7.2|
    |Druid| | |0.11.0|0.11.0|0.11.0|0.12.0|0.12.2|0.12.3|0.12.3|0.12.3-1.0.1|0.12.3|0.13.0|0.13.0|0.13.0|0.13.0|
    |HBase|1.1.1|1.1.1|1.1.1|1.1.1|1.1.1|1.1.1|1.1.1|1.1.1|1.1.1|1.1.1-1.0.2|1.1.1|1.4.9|1.4.9|1.4.9|1.4.9|
    |Phoenix|4.10.0|4.10.0|4.10.0|4.10.0|4.10.0|4.10.0|4.10.0|4.10.0|4.10.0|4.10.0-1.0.0|4.10.0|4.14.1|4.14.1|4.14.1|4.14.1|
    |Zookeeper|3.4.11|3.4.11|3.4.11|3.4.11|3.4.11|3.4.12|3.4.12|3.4.13|3.4.13|3.4.13|3.4.13|3.4.13|3.4.13|3.4.13|3.4.13|
    |Presto|0.188|0.188|0.188|0.188|0.188|0.188|0.208|0.208|0.208|0.208|0.213|0.213|0.213|0.213|0.213|
    |Storm|1.0.1|1.0.1|1.0.1|1.1.2|1.1.2|1.1.2|1.1.2|1.1.2|1.1.2|1.2.2|1.2.2|1.2.2|1.2.2|1.2.2|1.2.2|
    |Impala|2.10.0|2.10.0|2.10.0|2.10.0|2.10.0|2.10.0|2.10.0|2.10.0|2.10.0|2.10.0-1.0.0|2.12.2|2.12.2|2.12.2|2.12.2|2.12.2|
    |Hue|3.12.0|3.12.0|3.12.0|4.1.0|4.1.0|4.1.0|4.1.0|4.1.0|4.1.0|4.1.0|4.1.0|4.1.0|4.1.0|4.1.0|4.1.0|
    |Oozie|4.2.0|4.2.0|4.2.0|4.2.0|4.2.0|4.2.0|4.2.0|4.2.0|4.2.0|4.2.0|4.2.0|4.2.0|4.2.0|4.2.0|5.1.0|
    |Zeppelin|0.7.1|0.7.1|0.7.1|0.7.1|0.7.3|0.7.3|0.8.0|0.8.0|0.8.0|0.8.0|0.8.0|0.8.0|0.8.0|0.8.0|0.8.1|
    |Ranger| | |0.7.1|0.7.1|0.7.3|1.0.0|1.0.0|1.0.0|1.0.0|1.0.0|1.0.0|1.0.0|1.2.0|1.2.0|1.2.0|
    |Ganglia|3.7.2|3.7.2|3.7.2|3.7.2|3.7.2|3.7.2|3.7.2|3.7.2|3.7.2|3.7.2|3.7.2|3.7.2|3.7.2|3.7.2|3.7.2|
    |OS|CentOS 7.4|CentOS 7.4|CentOS 7.4|CentOS 7.4|CentOS 7.4|CentOS 7.4|CentOS 7.4|CentOS 7.4|CentOS 7.4|CentOS 7.4|CentOS 7.4|CentOS 7.4|CentOS 7.4|CentOS 7.4|CentOS 7.4|
    |Tensorflow| | | | | | |1.8.0|1.8.0|1.8.0|1.8.0|1.8.0| | | |1.8.0|
    |Kafka| | | | | | |2.11-1.0.1|2.11-1.0.1|2.11-1.0.1|2.11-1.1.0|2.11-1.1.1|2.11-1.1.1|2.11-1.1.1|1.1.1|2.11|
    |Superset| | | | | | |0.25.6|0.25.6|0.27.0|0.28.1|0.28.1|0.28.1|0.28.1|0.28.1|0.28.1|
    |Jupyter| | | | | | | | |4.4.0|4.4.0|4.4.0| | | |4.4.0|
    |Analytics Zoo| | | | | | | | |0.2.0|0.2.0|0.2.0| | | |0.2.0|

-   2.x

    |Version|EMR-2.9.2|EMR-2.10.0|EMR-2.11.0|
    |:------|:--------|:---------|:---------|
    |Release Date|2018.2|2018.4|2018.7|
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
    |Zookeeper|3.4.6|3.4.11|3.4.11|
    |Oozie|4.2.0|4.2.0|4.2.0|
    |Ranger|0.7.1|0.7.1|0.7.3|
    |Ganglia|3.7.2|3.7.2|3.7.2|
    |OS|CentOS 7.4|CentOS 7.4|CentOS 7.4|

-   1.x

    |Version|EMR-1.0.0|EMR-1.1.0|EMR-1.2.0|EMR-1.3.0|
    |-------|---------|---------|---------|---------|
    |Release Date|2015.11|2016.3|2016.4|2016.5|
    |Hadoop|2.6.0|2.6.0|2.6.0|2.6.0-emr-1.1.1|
    |Spark|1.4.1|1.6.0|1.6.1|1.6.1|
    |Hive|1.0.1|1.0.1|2.0.0|2.0.0|
    |Pig|0.14.0|0.14.0|0.14.0|0.14.0|
    |Sqoop|-|-|-|1.4.6|
    |Hue|-|-|-|3.9.0|
    |Zeppelin|-|-|-|0.5.6|
    |HBase|-|-|1.1.1|1.1.1|
    |Phoenix|-|-|-|-|
    |Zookeeper|-|-|3.4.6|3.4.6|
    |Ganglia|3.7.2|3.7.2|3.7.2|3.7.2|

-   Hadoop version description:

    To support Alibaba Cloud OSS, the emr-core component is added based on the version of Hadoop, without any changes made to the original interface. The version of this component is added after the Hadoop version.

-   EMR 3.1.x
    -   OS is upgraded to CentOS7.2.
    -   Spark is upgraded to 2.1.1.
    -   emr-core is upgraded to 1.2.6.
    -   Fixes bugs in AK-free OSS operations.
-   EMR 3.0.x
    -   **EMR 3.0.2**:
        -   emr-core is upgraded to 1.2.5.
        -   AK-free OSS supports more regions.
        -   Adjusts the AK replacement strategy.
        -   Fixes bugs in Hive and Hadoop.
    -   **EMR 3.0.1**:
        -   Supports interactive and unified table management.
        -   Uses an external unified database to save Hive metadata.
        -   Optimizes the read and write performance of OSS.
        -   All clusters that use external Hive metadata share the same meta information.
        -   Spark is upgraded to 2.0.2.
        -   emr-core is upgraded to 1.2.4.
    -   **EMR 3.0.0**:
        -   Uses 3.0.1, which is fully compatible with 3.0.0.
-   EMR 2.4.x
    -   **EMR 2.4.2**:
        -   emr-core is upgraded to 1.2.5.
        -   AK-free OSS supports more regions.
        -   Adjusts the AK replacement strategy.
        -   Fixes bugs in Hive and Hadoop.
    -   **EMR 2.4.1**:
        -   Supports interactive and unified table management.
        -   Uses an external unified database to save Hive metadata.
        -   Optimizes the read and write performance of OSS.
        -   All clusters that use external Hive metadata share the same meta information.
        -   emr-core is upgraded to 1.2.4.
    -   **EMR 2.4.0**: Uses 2.4.1, which is fully compatible with 2.4.0.
-   EMR 2.3.x
    -   **EMR 2.3.1**: Supports an interactive workbench where each cluster uses an independent internal database to save Hive metadata.
    -   **EMR 2.3.0**: Under maintenance. Supports an interactive workbench, but does not support table management. Using this version is not recommended.
-   EMR 2.2.0

    Deprecated version.

-   EMR 2.1.x
    -   **EMR 2.1.1**: Under maintenance. Supports an interactive workbench, but does not support table management. Using this version is not recommended.
    -   **EMR 2.1.0**: Deprecated version.
-   EMR 2.0.x

    Deprecated version.

-   EMR 1.xDeprecated version.

