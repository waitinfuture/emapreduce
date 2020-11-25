# Use EMR JindoCube

JindoCube is supported in E-MapReduce \(EMR\) V3.24.0 and later. This topic describes how to install, deploy, and use JindoCube in EMR.

## Prerequisites

A table or view is created.

## Overview

JindoCube is an advanced feature of EMR Spark. It implements pre-calculation to accelerate data processing. It improves query performance by dozens or even hundreds of times. You can persist data of a view to a data source supported by EMR Spark, such as \(HDFS\) or Object Storage Service \(OSS\). EMR Spark automatically discovers available persistent data and optimizes the execution plans for the data. This process is completely visible to users. JindoCube applies to business scenarios where a fixed query mode is used. JindoCube pre-calculates and pre-organizes data to accelerate data queries. For example, you can use JindoCube to implement multidimensional online analytical processing \(MOLAP\), generate data reports, create data dashboards, and synchronize data across clusters.

## Install and deploy JindoCube

JindoCube is an advanced feature of EMR Spark. You can use JindoCube to accelerate all Dataset, DataFrame API, and SQL tasks that are submitted by using EMR Spark. No additional component deployment or maintenance is required.

1.  Manage JindoCube on the Spark web UI.

    You can manage JindoCube on the Spark web UI. For example, you can create, delete, or update JindoCube caches. After you create a JindoCube cache, it automatically accelerates all Spark queries in the cluster. You can use the **spark.sql.cache.tab.display** parameter to specify whether to display the Cube Management tab on the Spark web UI. You can specify this parameter on the configuration page of the Spark service in the EMR console or in a Spark task submission command. The default value of this parameter is **false**.

    ![Spark_UI](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2026136061/p75248.png)

    You can also use the **spark.sql.cache.useDatabase** parameter to create a database based on your business requirements and store the view for which you want to create a cache in this database. If partitioned tables are used, you can use the **spark.sql.cache.cacheByPartition** parameter to specify whether to cache data by partition.

    |Parameter|Description|Example|
    |---------|-----------|-------|
    |**spark.sql.cache.tab.display**|Specifies whether to display the Cube Management tab.|true|
    |**spark.sql.cache.useDatabase**|The databases where cubes are stored.|db1,db2,dbn|
    |**spark.sql.cache.cacheByPartition**|Specifies whether to store cubes by partition.|true|

2.  Optimize Spark queries.

    You can use the **spark.sql.cache.queryRewrite** parameter to specify whether to use a JindoCube cache to accelerate Spark queries. You can specify this parameter at the cluster, session, or SQL statement level. The default value is **true**.


## Use JindoCube

1.  Create a JindoCube cache.
    1.  Log on to the [Alibaba Cloud EMR console](https://emr.console.aliyun.com) by using your Alibaba Cloud account.
    2.  Click the **Cluster Management** tab.
    3.  Find the cluster where you want to create a JindoCube cache and click the cluster ID.
    4.  In the left-side navigation pane, click **Connect Strings**.
    5.  On the **Public Connect Strings** page, click the link of YARN UI to go to the YARN web UI as a Knox user.

        For more information about Knox, see [Knox](/intl.en-US/Cluster types/Hadoop cluster/Knox.md).

    6.  Click **ApplicationMaster** in the row where **User** is **spark** and **Name** is **Thrift JDBC/ODBC Server**.
    7.  On the Spark web UI, click the **Cube Management** tab.
    8.  Click **New Cache**.

        Select a table or view, and click raw cache or cube cache in the action column. You can create two types of caches:

        -   Raw cache: persists the data of a table or view based on the cache configurations.

            When you create a raw cache, specify the parameters described in the following table.

            |Parameter|Description|Required|
            |---------|-----------|--------|
            |Cache Name|The name of the cache. The name can contain only letters, digits, hyphens \(-\), and underscores \(\_\).|Yes|
            |Column Selector|The columns from which you want to cache data.|Yes|
            |Rewrite|Specifies whether to use the cache to optimize the execution plans of subsequent queries.|Yes|
            |Provider|The storage format of the cached data. You can set this parameter to any format supported by Spark, such as JSON, Parquet, or ORC.|Yes|
            |Partition Columns|The partition fields by which data is cached.|No|
            |ZOrder Columns|The Z-order columns that are used to filter data. Z-order is a multi-column sorting method. After cached data is sorted by using this method, the data is filtered based on the Z-order columns during Spark queries. This further accelerates Spark queries.|No|

        -   Cube cache: builds a cube based on the raw data of a table or view and persists the cube data.

            When you create a cube cache, specify the parameters described in the following table.

            |Parameter|Description|Required|
            |---------|-----------|--------|
            |Cache Name|The name of the cache. The name can contain only letters, digits, hyphens \(-\), and underscores \(\_\).|Yes|
            |Dimension Selector|The dimension fields that are used to build a cube.|Yes|
            |Measure Selector|The measure fields and pre-calculation functions that are used to build a cube.|Yes|
            |Rewrite|Specifies whether to use the cache to optimize the execution plans of subsequent queries.|Yes|
            |Provider|The storage format of the cached data. You can set this parameter to any format supported by Spark, such as JSON, Parquet, or ORC.|Yes|
            |Partition Columns|The partition fields by which data is cached.|No|
            |ZOrder Columns|The Z-order columns that are used to filter data. Z-order is a multi-column sorting method. After cached data is sorted by using this method, the data is filtered based on the Z-order columns during Spark queries. This further accelerates Spark queries.|No|

            ![cube_cache](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2026136061/p75482.png)

            JindoCube builds a cube based on specified dimensions and measures. The following SQL statement shows the configurations of the cube cache in the preceding figure:

            ```
            SELECT c_city, c_nation, c_region, MAX(lo_quantity), SUM(lo_tax)
            FROM lineorder_flatten
            GROUP BY c_city, c_nation, c_region;
            ```

            JindoCube calculates the finest-grained combination of dimensions. Powered by the onsite computing capability of Spark, JindoCube implements re-aggregation to optimize the queries where a coarse-grained combination of dimensions is used. You must use the pre-calculation functions supported by JindoCube to define a cube cache. The following table lists the supported pre-calculation functions and the aggregate functions that correspond to them.

            |Aggregate function|Pre-calculation function|
            |------------------|------------------------|
            |COUNT|COUNT|
            |SUM|SUM|
            |MAX|MAX|
            |MIN|MIN|
            |AVG|COUNT and SUM|
            |COUNT \(DISTINCT\)|PRE\_COUNT\_DISTINCT|
            |APPROX\_COUNT\_DISTINCT|PRE\_APPROX\_COUNT\_DISTINCT|

            All created caches are displayed on the **Cube Management** tab. You can click **Detail** for a cache to view its details, including its basic information, partition information, building information, and building history.

2.  Build a JindoCube cache.

    During the creation of a cache, metadata is prepared, but no data is persisted. Therefore, after you create a cache, you must continue to build the cache. This way, the related data can be persisted to permanent storage, such as HDFS or OSS. In addition, the source table of a cache may be appended with new data or updated. In this case, you must update the data in the cache to ensure data consistency. You can use one of the following methods to build a cache:

    -   Manually build a cache

        Click Build Cache to manually build a cache. On the page that appears, configure the parameters, as shown in the following figure.

        ![build_Cache](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2026136061/p75437.png)

        The following table describes the parameters.

        |Parameter|Description|
        |---------|-----------|
        |**Save Mode**|The following modes are supported:

         -   Overwrite: overwrites exiting data in the cache.
        -   Append: adds new data to the cache. |
        |**Optional Filter**|You can select this parameter to add filter conditions. This way, JindoCube filters data based on the filter conditions before it persists the data.

         -   Column: the field based on which data is filtered.
        -   Filter Type: specifies whether to filter data by fixed values or by value range.
            -   Fixed Values: If this option is selected, specify one or more values. Separate values with commas \(,\).
            -   Range Values: If this option is selected, specify a minimum value and a maximum value. You can leave the maximum value empty. In this case, the value range is the minimum value and all values greater than it. |

        The following SQL statement shows the configurations of the raw cache for the lineorder\_flatten view in the preceding figure:

        ```
        SELECT * FROM lineorder_flatten
        WHERE s_region == 'ASIA' OR s_region == 'AMERICA';
        ```

        Click **Submit** to submit the cache building task to a Spark cluster. You can view the task status in the Build Information section of the cache details page. After the task is completed, you can view the cache information in the Build History section.

        **Note:** Spark writes the cached data into a specified directory in the same way common Spark tasks write data into a table or directory. If multiple users concurrently build a cache whose data format is Parquet, JSON, or ORC, the cached data may be inaccurate and invalid. If concurrent cache building operations or updates are inevitable, we recommend that you use a data format that supports concurrent writes, for example, Delta.

    -   Configure periodic cache building

        Click Trigger Period Build to configure a periodic cache update policy to ensure data consistency between the cache and source table. On the page that appears, configure the parameters, as shown in the following figure.

        ![Trigger_Period_build](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2026136061/p75457.png)

        The following table describes the parameters.

        |Parameter|Description|
        |---------|-----------|
        |**Save Mode**|The following modes are supported:

        -   Overwrite: overwrites exiting data in the cache.
        -   Append: adds new data to the cache. |
        |**Trigger Strategy**|Specifies the start time of the first building task, and the building interval.

        -   Start At: Select or enter the time you want to trigger the first building task. Format: yyyy-MM-dd hh:mm:ss.
        -   Period: Specify the interval at which cache building tasks are triggered. |
        |**Optional Step**|Specifies the filter conditions for cache building. JindoCube updates the cache in incremental mode based on the filter conditions and interval. If you do not select Optional Step, JindoCube updates the entire cache each time.

        -   Step By: Specify the type of the field that you want to update in incremental mode. TimeStamp Column\(Long Type\) indicates a timestamp field of the LONG type. DateTime Column\(String Type\) indicates a datetime field of the STRING type.
        -   Column Name: Specify the name of the field that you want to update in incremental mode. |

        On the cache details page, you can view the configured cache update policy. You can click Cancel Period Build to cancel periodic updates. You can view the information of a completed building task in the Build History section.

        **Note:**

        -   Periodic building tasks run on a Spark cluster. The related configurations are all saved in SparkContext, and the tasks are periodically triggered by Spark Driver. If you release the Spark cluster, periodic building is disabled.
        -   The Build History section displays the following information of each completed task in the current Spark cluster: start time, end time, save mode, building conditions, and final status. After you release the Spark cluster, the information in the Build History section is cleared.
3.  Manage a JindoCube cache.

    After you create and build a JindoCube cache, you can manage it on the Cube Management page.

    -   Delete a cache

        On the Cube Management page, find the cache you want to delete, and click **Drop** in the action column. All the metadata and data of the cache are deleted.

        ![Drop_table](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2026136061/p75382.png)

    -   Enable and disable optimization of Spark queries

        JindoCube allows you to specify whether to optimize Spark queries at the cache level. On the details page of a cache, click **Enabled** or **Disabled** in the lower part of the Basic Cache Information section to enable or disable the optimization.

        ![enable_disable](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2026136061/p75384.png)

    -   Delete partition data from a cache

        If data in a cache is stored by partition, you can delete the unnecessary partition data from the cache to release more space. On the details page of the cache, find the partition whose data you want to delete, and click **Delete** in the Action column.

        ![Delete](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3026136061/p75389.png)

        **Note:** Before you delete the data of a partition, make sure that the data is no longer needed. If you delete data that is required in subsequent queries, the queries return invalid results.

4.  Optimize Spark queries.

    JindoCube optimizes view-based queries. After you have created a raw cache or cube cache for a view, EMR Spark overwrites the execution plan of queries on this view. The new execution plan still uses the original logical semantics but can directly access the data in the cache. This accelerates Spark queries. Use the lineorder\_flatten view as an example. It is a wide table view generated by associating the lineorder table with other dimension tables. The following figure shows the information about this view.

    ![lineorder_define](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3026136061/p75355.png)

    The following figure shows the execution plan of simple queries on the lineorder\_flatten view.

    ![check_lineorder](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3026136061/p75356.png)

    After a raw cache is created and built for the lineorder\_flatten view, the same queries are performed on the view. EMR Spark automatically uses the cache to optimize the original execution plan. The following figure shows the new execution plan.

    ![lineorder_flatten_after](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3026136061/p75359.png)

    The new execution plan no longer uses the calculation logic of the lineorder\_flatten view. Instead, it directly accesses the cached data in HDFS.


## Precautions

1.  JindoCube does not ensure data consistency between the cache and source table. You must manually trigger data updates or configure a periodic update policy based on your requirements for data consistency.
2.  JindoCube determines whether to use a cache to optimize execution plans based on the metadata of views. If cached data is incomplete when JindoCube optimizes an execution plan, the query results may be invalid or incomplete. The following operations or configurations may cause incomplete cached data: 1. Delete partition data that is required in subsequent queries on the cache details page. 2. Data required in subsequent queries is filtered out based on the filter conditions specified when you build or update the cache. 3. Required data has not been updated to the cache when the query is performed.

