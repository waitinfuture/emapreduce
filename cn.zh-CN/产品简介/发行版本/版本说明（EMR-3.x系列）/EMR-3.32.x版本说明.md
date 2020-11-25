# EMR-3.32.x版本说明

本文介绍EMR-3.32.x发行版本的发布日期和更新内容等信息。

## 发布日期

EMR-3.32.0 2020年11月23日

## 更新内容

|服务|变更点|
|--|---|
|SmartData|升级至3.1.0版本。详情请参见[SmartData 3.1.0版本简介]() |
|Alluxio|-   支持Alluxio 2.4.0版本。
-   默认的参数配置，可以根据集群节点大小调整。
-   默认使用EMR集群内的HDFS作为底层的UnderFS，开箱即用。
-   增强Alluxio OSS UnderFS，适配OSS多版本等新功能。
-   适配Hadoop、Hive、Spark和Presto等引擎。 |
|HUDI|支持HUDI 0.6.0版本。|
|Spark|JindoTable支持打开或关闭数据采集功能。|
|Hive|-   修复了HiveServer连接池泄漏的问题。
-   JindoTable支持打开或关闭数据采集功能。
-   优化`ADD COLUMN`的性能。
-   修复了读取HUDI表时数据不正确的问题。
-   默认的参数配置，可以根据集群节点大小调整。 |
|HDFS|支持了更高数量级的Snapshot。|
|YARN|默认的参数配置，可以根据集群节点大小调整。|
|Tez|默认的参数配置，可以根据集群节点大小调整。|
|Sqoop|修复了Avro格式的文件导入问题。|

