# Release notes {#concept_vc2_nm3_y2b .concept}

## 3.x {#section_um4_rm3_y2b .section}

|Version|EMR-3.7.1|EMR-3.8.1|EMR-3.9.1|EMR-3.10.1|EMR-3.11.0|EMR-3.12.0|
|:------|:--------|:--------|:--------|:---------|:---------|:---------|
|Release Date|2018.1|2018.1|2018.2|2018.4|2018.6|2018.7|
|Hadoop|2.7.2-emr-1.2.10|2.7.2-emr-1.2.12|2.7.2-emr-1.2.13|2.7.2-emr-1.2.14|2.7.2-emr-1.2.14|2.7.2-emr-1.2.14|
|Spark|2.2.1|2.2.1|2.2.1|2.2.1|2.2.1|2.3.1|
|Hive|2.3.2|2.3.2|2.3.2|2.3.2|2.3.3|2.3.3|
|Tez|0.8.4|0.8.4|0.8.4|0.9.1|0.9.1|0.9.1|
|Pig|0.14.0|0.14.0|0.14.0|0.14.0|0.14.0|0.14.0|
|Sqoop|1.4.6|1.4.6|1.4.6|1.4.6|1.4.6|1.4.7|
|Flink| |1.4.0|1.4.0|1.4.0|1.4.0|1.4.0|
|Druid| | |0.11.0|0.11.0|0.11.0|0.12.0|
|HBase|1.1.1|1.1.1|1.1.1|1.1.1|1.1.1|1.1.1|
|Phoenix|4.10.0|4.10.0|4.10.0|4.10.0|4.10.0|4.10.0|
|Zookeeper|3.4.11|3.4.11|3.4.11|3.4.11|3.4.11|3.4.12|
|Presto|0.188|0.188|0.188|0.188|0.188|0.188|
|Storm|1.0.1|1.0.1|1.0.1|1.1.2|1.1.2|1.1.2|
|Impala|2.10.0|2.10.0|2.10.0|2.10.0|2.10.0|2.10.0|
|Hue|3.12.0|3.12.0|3.12.0|4.1.0|4.1.0|4.1.0|
|Oozie|4.2.0|4.2.0|4.2.0|4.2.0|4.2.0|4.2.0|
|Zeppelin|0.7.1|0.7.1|0.7.1|0.7.1|0.7.3|0.7.3|
|Ranger| | |0.7.1|0.7.1|0.7.3|1.0.0|
|Ganglia|3.7.2|3.7.2|3.7.2|3.7.2|3.7.2|3.7.2|
|OS|CentOS 7.4|CentOS 7.4|CentOS 7.4|CentOS 7.4|CentOS 7.4|CentOS 7.4|

## 2.x {#section_dwn_j43_y2b .section}

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

## 1.x {#section_nln_pq3_y2b .section}

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

Hadoop version description:

To provide support for Alibaba Cloud OSS, the emr-core component is added based on the version of open-source Hadoop without any changes made to the original interface. The version of this component will be added after the Hadoop version. The emr-core component version follows the hadoop version.

## EMR 3.1.x {#section_g1f_mr3_y2b .section}

**EMR 3.1.1**: OS is upgraded to CentOS7.2. Spark is upgraded to 2.1.1. emr-core is upgraded to 1.2.6. Fixed the OSS AK-free operation bug.

## EMR 3.0.x {#section_h1f_mr3_y2b .section}

-   **EMR 3.0.2**: emr-core is upgraded to 1.2.5. AK-free OSS supports more regions. Adjusted the replacement strategy of AK, fixed Hive, Hadoop related bugs.
-   **EMR 3.0.1**: Supports interactive and unified table management. Saves hive meta using external unified database. emr-core is upgraded to 1.2.4. Optimized the read and write performance of OSS. All the clusters that use external hive meta share the same meta information. Spark is upgraded to 2.0.2.
-   **EMR 3.0.0**: Use the upgraded version 3.0.1. It is fully compatible with the earlier version 3.0.0.

## EMR 2.5.x {#section_i1f_mr3_y2b .section}

**EMR 2.5.1**: OS is upgraded to CentOS7.2. emr-core is upgraded to version 1.2.6. Fixed OSS AK-free operational bugs.

## EMR 2.4.x {#section_j1f_mr3_y2b .section}

-   **EMR 2.4.2**：emr-core is upgraded to 1.2.5. OSS AK-free operation supports more regions. Adjusted the replacement strategy of the role AK. Fixed Hive and Hadoop related bugs.
-   **EMR 2.4.1**: Supports interactive and unified table management, and uses external unified databases to save hive meta. emr-core is upgraded to version 1.2.4. Optimized the read and write performance of OSS. All the clusters using external hive meta share the same meta information .
-   **EMR 2.4.0**: Use the upgraded version 2.4.1. It is fully compatible with version 2.4.0.

## EMR 2.3.x {#section_k1f_mr3_y2b .section}

-   **EMR 2.3.1**: Supports an interactive workbench where each cluster can use an independent internal database to save hive meta.
-   **EMR 2.3.0**: An earlier version that is under maintenance. Supports an interactive workbench, but doesn't support table management. It is not recommended to use.

## EMR 2.2.0 {#section_l1f_mr3_y2b .section}

An temporary version that is deprecated.

## EMR 2.1.x {#section_m1f_mr3_y2b .section}

-   **EMR 2.1.1**: An earlier version that is under maintenance. Supports an interactive workbench, but doesn't support table management. It is not recommended to use.
-   **EMR 2.1.0**: An earlier version that is deprecated.

## EMR 2.0.x {#section_n1f_mr3_y2b .section}

An earlier version that is deprecated.

## EMR 1.x {#section_o1f_mr3_y2b .section}

An earlier version that has few features. It is deprecated for the moment.

