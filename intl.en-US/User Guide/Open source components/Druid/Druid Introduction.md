# Druid Introduction {#concept_csl_jkd_z2b .concept}

Druid is a distributed real-time memory analysis system launched by Metamarkets, a company that provides data analysis services to online media or advertising companies. Druid is used to resolve fast and interactive queries and analyse issues in large datasets.

## Basic features {#section_jcb_mkd_z2b .section}

Druid has the following features:

-   Sub-second OLAP queries, including multi-dimensional filtering, ad-hoc attribute grouping, and fast data aggregation.
-   Real-time data consumption, real-time data collection, and real-time data query.
-   Efficient multi-tenant capability, which enables thousands of users to perform searches online at the same time.
-   Strong scalability, which supports fast processing of PB-level data and 100 billion-level events, and thousands of concurrent queries per second.
-   Extremely high availability and support for rolling upgrades.

## Scenarios {#section_lcb_mkd_z2b .section}

Real-time data analysis is the most typical usage scenario for Druid. This scenario can cover a wide range of areas, such as:

-   Real-time indicator monitoring
-   Recommendation of model
-   Advertisement platforms
-   Search for models

In these scenarios, there are large amounts of data, and the requirement for time delay of data query is high. In real-time indicator monitoring, system problems need to be detected at the moment of occurrence so the user can be warned. In the recommendation model, the user behavior data needs to be collected in real time, and be timely sent to the recommendation system. After a few clicks, the system will be able to identify your search intent and recommend more reasonable results in subsequent searches.

## Druid architecture {#section_ncb_mkd_z2b .section}

Druid has an excellent architectural design with multiple components working together to complete a series of processes such as data collection, data indexing, data storage, and data query.

The following figure shows the components contained in the Druid working-layer \(data indexing as well as data query\).

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17905/154148575110852_en-US.png)

-   The real-time component is responsible for the real-time data collection.
-   In the broker phase, query tasks are distributed, and query results are collected and returned to users.
-   The historical node is responsible for the storage of the historical data after the indexing. The data is stored in deep storage. Deep storage can be either local or a distributed file system, such as HDFS.
-   The indexing service consists of two components \(not shown in the figure\).
    -   The Overlord component is responsible for managing and distributing indexing tasks.
    -   The MiddleManager component is responsible for executing indexing tasks.

The following figure shows the components involved in the management layer of Druid segments \(Druid index file\).

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17905/154148575110853_en-US.png)

-   The ZooKeeper component is responsible for storing the status of the cluster and discovering components, such as the topology information of the cluster, election of the Overlord leader, and management of the indexing task.
-   The Coordinator component is responsible for managing the segments, such as the download and deletion of the segments and balancing with historical components.
-   The Metadata storage component is responsible for storing the meta-information of segments and managing all kinds of persistent or temporary data in the cluster, such as configuration information and audit information.

## Product advantages {#section_o4g_wkd_z2b .section}

E-MapReduce Druid is improved a lot based on the open source Druid, including integration with E-MapReduce and the peripheral ecology of Alibaba Cloud, easy monitoring and operation support, and easy-to-use product interfaces. After buying it, you can use immediately. It does not need operation and maintenance for 7x24 hours.

E-MapReduce Druid supports the following features:

-   Using OSS as deep storage
-   Using OSS files as data sources for indexing in batches
-   Using RDS to store metadata
-   Integrating with Superset tools
-   Easy scale up and scale down \(scale down is for task node\)
-   diversified monitoring indicators and alarm rules
-   Bad node migration
-   High-security mode
-   Supporting HA

