# SparkSQL自适应执行 {#concept_lv1_ttp_kfb .concept}

阿里云EMR-3.13.0版本的SparkSQL支持自适应执行功能，用来解决Reduce个数的动态调整/数据倾斜/执行计划的动态优化问题。

## 解决问题 {#section_v4f_k5p_kfb .section}

SparkSQL自适应执行解决以下问题:

-   shuffle partition个数

    目前SparkSQL中reduce阶段的task个数取决于固定参数spark.sql.shuffle.partition\(默认值200\)，一个作业一旦设置了该参数，它运行过程中的所有阶段的reduce个数都是同一个值。

    而对于不同的作业，以及同一个作业内的不同reduce阶段，实际的数据量大小可能相差很大，比如reduce阶段要处理的数据可能是10MB，也有可能是100GB, 如果使用同一个值对实际运行效率会产生很大影响，比如10MB的数据一个task就可以解决，如果spark.sql.shuffle.partition使用默认值200的话，那么10MB的数据就要被分成200个task处理，增加了调度开销，影响运行效率。

    SparkSQL自适应框架可以通过设置shuffle partition的上下限区间，在这个区间内对不同作业不同阶段的reduce个数进行动态调整。

    通过区间的设置，一方面可以大大减少调优的成本\(不需要找到一个固定值\)，另一方面同一个作业内部不同reduce阶段的reduce个数也能动态调整。

    参数：

    |属性名称|默认值|备注|
    |----|---|--|
    |spark.sql.adaptive.enabled|false|自适应执行框架的开关|
    |spark.sql.adaptive.minNumPostShufflePartitions|1|reduce个数区间最小值|
    |spark.sql.adaptive.maxNumPostShufflePartitions|500|reduce个数区间最大值|
    |spark.sql.adaptive.shuffle.targetPostShuffleInputSize|67108864|动态调整reduce个数的partition大小依据，如设置64MB则reduce阶段每个task最少处理64MB的数据|
    |spark.sql.adaptive.shuffle.targetPostShuffleRowCount|20000000|动态调整reduce个数的partition条数依据，如设置20000000则reduce阶段每个task最少处理20000000条的数据|

-   数据倾斜

    join中会经常碰到数据倾斜的场景，导致某些task处理的数据过多，出现很严重的长尾。目前SparkSQL没有对倾斜的数据进行相关的优化处理。

    SparkSQL自适应框架可以根据预先的配置在作业运行过程中自动检测是否出现倾斜，并对检测到的倾斜进行优化处理。

    优化的主要逻辑是对倾斜的partition进行拆分由多个task来进行处理，最后通过union进行结果合并。

    支持的Join类型：

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
    |spark.sql.adaptive.skewedPartitionFactor|10|当一个partition的size大小 大于 该值\(所有parititon大小的中位数\) 且 大于spark.sql.adaptive.skewedPartitionSizeThreshold，或者parition的条数 大于 该值\(所有parititon条数的中位数\) 且 大于 spark.sql.adaptive.skewedPartitionRowCountThreshold， 才会被当做倾斜的partition进行相应的处理|
    |spark.sql.adaptive.skewedPartitionSizeThreshold|67108864|倾斜的partition大小不能小于该值|
    |spark.sql.adaptive.skewedPartitionRowCountThreshold|10000000|倾斜的partition条数不能小于该值|
    |spark.shuffle.statistics.verbose|false|打开后MapStatus会采集每个partition条数的信息，用于倾斜处理|

-   Runtime执行计划优化

    SparkSQL的Catalyst优化器会将sql语句转换成物理执行计划，然后真正运行物理执行计划。但是Catalyst转换物理执行计划的过程中，由于缺少Statistics统计信息，或者Statistics统计信息不准等原因，会到时转换的物理执行计划可能并不是最优的，比如转换为SortMergeJoinExec,但实际BroadcastJoin更合适。

    SparkSQL自适应执行框架会在物理执行计划真正运行的过程中，动态的根据shuffle阶段shuffle write的实际数据大小，来调整是否可以用BroadcastJoin来代替SortMergeJoin，提高运行效率。

    参数：

    |属性名称|默认值|备注|
    |----|---|--|
    |spark.sql.adaptive.enabled|false|自适应执行框架的开关|
    |spark.sql.adaptive.join.enabled|true|开关|
    |spark.sql.adaptiveBroadcastJoinThreshold|等于spark.sql.autoBroadcastJoinThreshold|运行过程中用于判断是否满足BroadcastJoin条件|


## 测试 {#section_zsn_jvp_kfb .section}

以TPC-DS中某些query为例

-   shuffle partition个数
    -   query30

        原生Spark:

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23347/154216146713542_zh-CN.png)

    -   自适应调整reduce个数:

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23347/154216146713543_zh-CN.png)

-   Runtime执行计划优化\(SortMergeJoin转BroadcastJoin\)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23347/154216146713544_zh-CN.png)

    自适应转换为BroadcastJoin

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23347/154216146813545_zh-CN.png)


