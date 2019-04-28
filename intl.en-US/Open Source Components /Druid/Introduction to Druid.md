# Introduction to Druid {#concept_csl_jkd_z2b .concept}

Druid is a column-oriented, open-source, distributed data storeused to query and analyze issues in large datasets in real time.

## Basic features {#section_jcb_mkd_z2b .section}

Druid has the following features:

-   Sub-second OLAP queries, including multi-dimensional filtering, ad-hoc attribute grouping, and fast data aggregation.
-   Real-time data consumption, collection, and querying.
-   Efficient multi-tenant capability, which enables thousands of users to perform searches online at the same time.
-   Strong scalability, which supports the fast processing of PB-level data, 100 billion-level events, and thousands of concurrent queries per second.
-   Extremely high availability and support for rolling upgrades.

## Usage scenarios {#section_lcb_mkd_z2b .section}

Real-time data analysis is the most typical usage scenario for Druid and covers a wide range of areas, including:

-   Real-time indicator monitoring
-   Model recommendations
-   Advertisement platforms
-   Model searches

These scenariosinvolve large amounts of data, and the requirement for time delay in data querying is high. In real-time indicator monitoring, problems need to be detected at the moment of occurrence so thatyou can be warned as soon as possible. In the recommendation model, user behavior data needs to be collected in real time and sent to the recommendation system promptly. In just a few clicks, the system is able to identify your search intent and recommend more appropriate results in future searches.

## Architecture {#section_ncb_mkd_z2b .section}

Druid has an excellent architectural design with multiple components working together to complete a series of processes, such as data collection, indexing, storage, and querying.

The following figure shows the components contained in the Druid working-layer \(for data indexing and data querying\).

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17905/155643648710852_en-US.png)

-   The real-time component is responsible for the real-time data collection.
-   In the broker phase, query tasks are distributed, and the results are collected and returned to you.
-   The historical node is responsible for the storage of historical data after indexing. The data is stored in deep storage. Deep storage can be either local or a distributed file system, such as HDFS.
-   The indexing service consists of two components \(not shown in the figure\).
    -   The Overlord component is responsible for managing and distributing indexing tasks.
    -   The MiddleManager component is responsible for executing indexing tasks.

The following figure shows the components involved in the management layer of Druid segments \(Druid index file\).

![Druid segments](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17905/155643648710853_en-US.png)

-   The ZooKeeper component is responsible for storing the status of the cluster and discovering components, such as the topology information of the cluster, election of the Overlord leader, and management of the indexing task.
-   The Coordinator component is responsible for managing segments, such as the downloading and deletion of the segments and balancing them with historical components.
-   The Metadata storage component is responsible for storing the meta-information of segments and managing all kinds of persistent or temporary data in the cluster, such as configuration information and audit information.

## Product advantages {#section_o4g_wkd_z2b .section}

E-MapReduce Druid has improved a lot based on open-source Druid, including integration with E-MapReduce and the peripheral Alibaba Cloud ecosystem, easy monitoring and operation support, and easy-to-use product interfaces. You can use it immediately after purchase. It does not need 24/7 operation and maintenance.

E-MapReduce Druid supports the following features:

-   Using OSS as deep storage
-   Using OSS files as data sources for indexing in batches
-   Supporting indexing the streaming data from Log Service and providing high reliability and exactly-once semantics
-   Using RDS to store metadata
-   Integrating with Superset tools
-   Easy scale up and scale down \(scale down is for task node\)
-   Diversified monitoring indicators and alarm rules
-   Bad node migration
-   High-security mode
-   HA

