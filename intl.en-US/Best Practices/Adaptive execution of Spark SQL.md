# Adaptive execution of Spark SQL {#concept_lv1_ttp_kfb .concept}

Spark SQL of Alibaba Cloud Elastic MapReduce \(E-MapReduce\) 3.13.0 supports adaptive execution. It is used to set the number of reduce tasks automatically, solve data skew, and dynamically optimize execution plans.

## Solved problems {#section_v4f_k5p_kfb .section}

Adaptive execution of Spark SQL solves the following problems:

-   The number of shuffle partitions

    Currently, the number of tasks in the reduce stage in Spark SQL depends on the value of the spark.sql.shuffle.partition parameter \(the default value is 200\). Once this parameter has been specified for a job, the number of reduce tasks in all stages is the same value when the job is running.

    For different jobs, and for different reduce stages of one job, the actual data size can be quite different. For example, data to be processed in the reduce stage may have a size of 10 MB or 100 GB. If the parameter is specified using the same value, it has a significant impact on the actual processing efficiency. For example, 10 MB of data can be processed using only one task. If the value of the spark.sql.shuffle.partition parameter is set to the default value of 200, then 10 MB of data is partitioned to be processed by 200 tasks. This increases scheduling overheads and lowers processing efficiency.

    By setting the range of the shuffle partition number, the adaptive execution framework of Spark SQL can dynamically adjust the number of reduce tasks in the range for different stages of different jobs.

    This significantly reduces the costs for optimization \(no need to find a fixed value\). Additionally, the numbers of reduce tasks in different stages of one job can be dynamically adjusted.

    Parameter:

    |Attribute|Default value|Description|
    |---------|-------------|-----------|
    |spark.sql.adaptive.enabled|false|Enables or disables adaptive execution.|
    |spark.sql.adaptive.minNumPostShufflePartitions|1|The minimum number of reduce tasks.|
    |spark.sql.adaptive.maxNumPostShufflePartitions|500|The maximum number of the reduce tasks.|
    |spark.sql.adaptive.shuffle.targetPostShuffleInputSize|67108864|Dynamically adjusts the number of reduce tasks based on the partition size. For example, if the value is set to 64 MB, then each task in the reduce stage processes more than 64 MB data.|
    |spark.sql.adaptive.shuffle.targetPostShuffleRowCount|20000000|Dynamically adjusts the number of reduce tasks based on the row number in the partition. For example, if the value is set to 20000000, then each task in the reduce stage processes more than 20,000,000 rows of data.|

-   Data skew

    Data skew is a common issue in SQL join operations. It refers to the scenario where certain tasks involve too much data in the processing, which leads to long tails. Currently, Spark SQL does not perform optimization for skewed data.

    The Adaptive Execution framework of Spark SQL can automatically detect skewed data and perform optimization for it at runtime.

    SparkSQL optimizes skewed data as follows: it splits the data that is in the skewed partition, processes the data through multiple tasks, and then combines the results through SQL union operations.

    Supported join types:

    |Type|Description|
    |----|-----------|
    |Inner|Skewed data can be handled in both tables.|
    |Cross|Skewed data can be handled in both tables.|
    |LeftSemi|Skewed data can only be handled in the left table.|
    |LeftAnti|Skewed data can only be handled in the left table.|
    |LeftOuter|Skewed data can only be handled in the left table.|
    |RightOuter|Skewed data can only be handled in the right table.|

    Parameter:

    |Attribute|Default value|Description|
    |---------|-------------|-----------|
    |spark.sql.adaptive.enabled|false|Enables or disables the adaptive execution framework.|
    |spark.sql.adaptive.skewedJoin.enabled|false|Enables or disables the handling of skewed data.|
    |spark.sql.adaptive.skewedPartitionFactor|10|A partition is identified as a skewed partition only when the following scenarios occur. First, the size of a partition is greater than this value \(median size of all partitions\) and the value of the spark.sql.adaptive.skewedPartitionSizeThreshold parameter. Second, the rows in a partition are greater than this value \(median rows in all partitions\) and the value of the spark.sql.adaptive.skewedPartitionSizeThreshold parameter.|
    |spark.sql.adaptive.skewedPartitionSizeThreshold|67108864|The size threshold for a skewed partition.|
    |spark.sql.adaptive.skewedPartitionRowCountThreshold|10000000|The row number threshold for a skewed partition.|
    |spark.shuffle.statistics.verbose|false|When the value of this parameter is true, MapStatus collects information about the number of rows in each partition for handling skewed data.|

-   Execution plan optimization at runtime

    Catalyst optimizer of Spark SQL converts logical plans that are converted from SQL statements into physical execution plans and executes those physical execution plans. However, the physical execution plan produced by Catalyst may not be optimal because of lack or inaccuracy of statistics. For example, Spark SQL may choose SortMergeJoinExec instead of BroadcastJoin, while BroadcastJoin is the optimal option in the scenario.

    The Adaptive Execution framework of Spark SQL determines whether to use BroadcastJoin instead of SortMergeJoin to improve query performance based on the size of the shuffle write in the shuffle stage.

    Parameter:

    |Attribute|Default value|Description|
    |---------|-------------|-----------|
    |spark.sql.adaptive.enabled|false|Enables or disables the adaptive execution framework.|
    |spark.sql.adaptive.join.enabled|true|Whether to determine a better join strategy at runtime.|
    |spark.sql.adaptiveBroadcastJoinThreshold|Equals to spark.sql.autoBroadcastJoinThreshold.|Determines whether to use broadcast join to optimize join queries.|


## Test {#section_zsn_jvp_kfb .section}

Take some TPC-DS queries as test samples.

-   Shuffle partition number
    -   Query 30

        Native Spark:

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23347/154874730913542_en-US.png)

    -   Adjusts the number of reduce tasks adaptively.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23347/154874730913543_en-US.png)

-   Execution plan optimization at runtime \(SortMergeJoin to BroadcastJoin\).

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23347/154874730913544_en-US.png)

    Uses BroadcastJoin adaptively.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23347/154874730913545_en-US.png)


