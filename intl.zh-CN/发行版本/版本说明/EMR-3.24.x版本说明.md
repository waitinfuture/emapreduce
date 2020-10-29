# EMR-3.24.x版本说明

本文介绍EMR-3.24.x发行版本的发布日期和更新内容等信息。

## 发布日期

EMR-3.24.0 2019年11月18日

## 新功能

|服务|变更点|
|--|---|
|Delta|-   支持SQL语法，包括ALTER、CONVERT、CREATE、CTAS、DELETE、DESC、INSERT、MERGE、OPTIMIZE、UPDATE和VACUUM。
-   内置并优化Optimize。
-   支持Hive connector。
-   支持其他开源已有特性。 |
|Grafana|新增组件（Flink独立集群），版本6.4.2。|
|Prometheus|新增组件（Flink独立集群），版本2.13.0。|
|AlertManager|新增组件（Flink独立集群），版本0.19.0。|
|TensorFlow on spark|-   支持TensorFlow框架置于Spark之上，使得Spark与深度学习框架深度结合，包括了任务调度和数据交换优化方案等，为您提供从数据预处理到深度学习训练任务的一整套流程。
-   支持Streaming类型任务。 |

## 更新内容

|服务|变更点|
|--|---|
|SmartData|-   优化JindoFS使用模式：Block模式使用方式不变；Cache模式不仅支持原有用法，还兼容了原有OSS文件系统的使用方式，支持数据缓存和元数据缓存，并可以通过配置分别控制开关（默认均关闭）。
-   优化Block模式和Cache模式读写性能。
-   优化磁盘清理，对本地磁盘上缓存的热数据实现更精确的统计和更及时的清理，并且能够严格保证磁盘使用率不会超过配额。
-   完善对Gateway集群的支持，能够在Gateway上使用Block模式和Cache模式。
-   支持一个存储集群与多个计算集群分离的部署方式。 |
|Spark|-   增加Delta相关参数支持。
-   增加对Ranger spark plugin配置的支持。
-   JindoCube升级到0.3.0版本。 |
|Hive|-   增加SQL兼容性检查功能逻辑。
-   Hive2.3.5+Hadoop2.8.5组合发布。
-   重启组件时不同步hiveserver2-site.xml中的内容至spark-conf下的hive-site.xml。
-   支持使用MSCK命令添加增量目录。
-   修复Hive复用tez container时出现的bug。
-   支持使用MSCK命令优化列目录。 |
|Bigboot|升级至2.2.1，修复Native代码支持在部分机型上的问题。|
|Ranger|-   Spark plugin部署方式重构。
-   修复HA集群header2没有获取keytab的bug。 |
|Kudu|修复启动逻辑。|
|Zookeeper|增加四字命令配置，默认开启。|
|HDFS|适配JindoFS。|
|YARN|-   修改默认配置**yarn.scheduler.capacity.node-locality-delay**为-1。
-   适配JindoFS。 |
|Has|对接OpenLDAP做后端。|
|OpenLDAP|适配Has。|
|Presto|升级版本到0.228。|
|Kafka|移除D1坏盘。|
|Druid|升级至0.16.0。|
|Flume|升级至1.9.0。|
|Flink|-   升级至1.9.1。
-   支持Flink独立集群（白名单发布）。 |

