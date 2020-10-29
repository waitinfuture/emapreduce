# EMR-3.25.x版本说明

本文介绍EMR-3.25.x发行版本的发布日期和更新内容等信息。

## 发布日期

EMR-3.25.0 2020年1月13日

## 新功能

Ranger服务：支持Ranger Presto操作。

## 更新内容

|服务|变更点|
|--|---|
|Ranger|-   初始化HA集群RangerAdmin数据库。
-   修复RangerUserSync启动脚本时的安全性问题。 |
|Spark|-   支持在控制台配置`spark.sql.extensions`等Delta相关参数。
-   支持Hive读取Delta table，避免set inputformat。
-   支持ALTER TABLE SET TBLPROPERTIES和UNSET TBLPROPERTIES语句。 |
|Delta|
|Hive|修复自动LOCAL模式下MR任务执行失败的问题。|
|Presto|-   升级至310版本。
-   升级joda-time版本至2.10.5。 |
|Tez|-   升级至0.9.2版本。
-   修复tez-ui application进度无法正常显示的问题。
-   修复tez-ui application history无法查看的问题。 |
|Impala|修复Impala无法访问lzo表的问题。|
|HDFS|移除mongo-hadoop的相关JAR包。|
|Zookeeper|升级至3.5.6版本。|
|YARN|适配tez-ui，**yarn-site**页签支持添加配置项**yarn.resourcemanager.system-metrics-publisher.enabled=true**。|
|Bigboot|-   升级至2.2.3版本。
-   OSS Cache模式下支持rename操作。 |
|SmartData|
|Knox|升级依赖包版本。|
|Oozie|升级依赖包版本。|

