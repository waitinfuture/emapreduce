# SparkSQL 自适应执行 {#concept_lv1_ttp_kfb .concept}

阿里云 E-MapReduce-3.13.0 版本的 SparkSQL 支持自适应执行功能，用来解决 Reduce 个数的动态调整/数据倾斜/执行计划的动态优化问题。

## 解决问题 {#section_v4f_k5p_kfb .section}

SparkSQL 自适应执行解决以下问题:

-   Shuffle partition个数

    目前 SparkSQL 中 reduce 阶段的 task 个数取决于固定参数 spark.sql.shuffle.partition\(默认值 200\)，一个作业一旦设置了该参数，它运行过程中的所有阶段的 reduce 个数都是同一个值。

    而对于不同的作业，以及同一个作业内的不同 reduce 阶段，实际的数据量大小可能相差很大，比如 reduce 阶段要处理的数据可能是 10MB，也有可能是 100GB, 如果使用同一个值对实际运行效率会产生很大影响，比如 10MB的数据一个 task 就可以解决，如果 spark.sql.shuffle.partition使用默认值 200 的话，那么 10MB 的数据就要被分成 200 个 task 处理，增加了调度开销，影响运行效率。

    SparkSQL 自适应框架可以通过设置 shuffle partition 的上下限区间，在这个区间内对不同作业不同阶段的 reduce 个数进行动态调整。

    通过区间的设置，一方面可以大大减少调优的成本\(不需要找到一个固定值\)，另一方面同一个作业内部不同 reduce 阶段的 reduce 个数也能动态调整。

    参数：

    |属性名称|默认值|备注|
    |----|---|--|
    |spark.sql.adaptive.enabled|false|自适应执行框架的开关|
    |spark.sql.adaptive.minNumPostShufflePartitions|1|reduce 个数区间最小值|
    |spark.sql.adaptive.maxNumPostShufflePartitions|500|reduce 个数区间最大值|
    |spark.sql.adaptive.shuffle.targetPostShuffleInputSize|67108864|动态调整 reduce 个数的 partition 大小依据，如设置 64MB 则reduce 阶段每个 task 最少处理 64MB 的数据|
    |spark.sql.adaptive.shuffle.targetPostShuffleRowCount|20000000|动态调整 reduce个数的 partition 条数依据，如设置 20000000 则reduce 阶段每个 task 最少处理 20000000 条的数据|

-   数据倾斜

    Join 中会经常碰到数据倾斜的场景，导致某些 task 处理的数据过多，出现很严重的长尾。目前 SparkSQL 没有对倾斜的数据进行相关的优化处理。

    SparkSQL 自适应框架可以根据预先的配置在作业运行过程中自动检测是否出现倾斜，并对检测到的倾斜进行优化处理。

    优化的主要逻辑是对倾斜的 partition 进行拆分由多个 task 来进行处理，最后通过union 进行结果合并。

    支持的 Join 类型：

    |join类型|备注|
    |------|--|
    |Inner|左/右表均可处理倾斜|
    |Cross|左/右表均可处理倾斜|
    |LeftSemi|只对左表处理倾斜|
    |LeftAnti|只对左表处理倾斜|
    |LeftOuter|只对左表处理倾斜|
    |RightOuter|只对右表处理倾斜|

    参数：

    |属性名称|默认值|备注|
    |----|---|--|
    |spark.sql.adaptive.enabled|false|自适应执行框架的开关|
    |spark.sql.adaptive.skewedJoin.enabled|false|倾斜处理开关|
    |spark.sql.adaptive.skewedPartitionFactor|10|当一个 partition 的 size 大小 大于 该值\(所有 parititon 大小的中位数\) 且 大于spark.sql.adaptive.skewedPartitionSizeThreshold，或者parition的条数 大于 该值\(所有parititon条数的中位数\) 且 大于 spark.sql.adaptive.skewedPartitionRowCountThreshold， 才会被当做倾斜的partition 进行相应的处理|
    |spark.sql.adaptive.skewedPartitionSizeThreshold|67108864|倾斜的 partition 大小不能小于该值|
    |spark.sql.adaptive.skewedPartitionRowCountThreshold|10000000|倾斜的 partition 条数不能小于该值|
    |spark.shuffle.statistics.verbose|false|打开后 MapStatus会采集每个 partition 条数的信息，用于倾斜处理|

-   Runtime执行计划优化

    SparkSQL 的 Catalyst 优化器会将 sql 语句转换成物理执行计划，然后真正运行物理执行计划。但是 Catalyst 转换物理执行计划的过程中，由于缺少 Statistics 统计信息，或者 Statistics 统计信息不准等原因，会到时转换的物理执行计划可能并不是最优的，比如转换为 SortMergeJoinExec，但实际 BroadcastJoin 更合适。

    SparkSQL 自适应执行框架会在物理执行计划真正运行的过程中，动态的根据shuffle 阶段 shuffle write 的实际数据大小，来调整是否可以用 BroadcastJoin 来代替SortMergeJoin，提高运行效率。

    参数：

    |属性名称|默认值|备注|
    |----|---|--|
    |spark.sql.adaptive.enabled|false|自适应执行框架的开关|
    |spark.sql.adaptive.join.enabled|true|开关|
    |spark.sql.adaptiveBroadcastJoinThreshold|等于spark.sql.autoBroadcastJoinThreshold|运行过程中用于判断是否满足BroadcastJoin 条件|


## 测试 {#section_zsn_jvp_kfb .section}

以TPC-DS 中某些 query 为例

-   shuffle partition 个数
    -   Query 30

        原生Spark:

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23347/154874730113542_zh-CN.png)

    -   自适应调整 reduce 个数:

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23347/154874730113543_zh-CN.png)

-   Runtime 执行计划优化\(SortMergeJoin转BroadcastJoin\)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23347/154874730113544_zh-CN.png)

    自适应转换为 BroadcastJoin

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23347/154874730213545_zh-CN.png)


