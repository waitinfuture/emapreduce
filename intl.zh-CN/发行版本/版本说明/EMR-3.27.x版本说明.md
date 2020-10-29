# EMR-3.27.x版本说明

本文介绍EMR-3.27.x发行版本的发布日期和更新内容等信息。

## 发布日期

|版本|日期|
|--|--|
|EMR-3.27.0|2020年4月29日|
|EMR-3.27.1|2020年5月8日|
|EMR-3.27.2|2020年5月20日|

## 新功能

|功能|变更点|
|--|---|
|组件自定义部署|支持对Master节点上的组件进行自定义部署，目前支持以下组件： -   Hadoop
-   Spark
-   Hive
-   Zookeeper
-   Presto |
|弹性伸缩功能支持优雅下线|开启优雅下线后，节点不会被立即释放，而是在设置的时间段内等待任务执行完成后释放。|

## 更新内容

|服务|变更点|
|--|---|
|Spark|-   CUBE中支持日期类型分区字段。
-   调大Spark-Submit的stack深度。 |
|Delta|-   DDL相关语法增强，包括CREATE、SHOW、DESCRIBE等相关命令。
-   支持带ZOrder的Optimize语法。 |
|Knox|-   适配Druid UI。
-   支持多Master部署。 |
|Hive|-   hcatalog表支持magic committer。
-   移除一些过时的默认配置。 |
|Bigboot|-   升级至2.6.3版本。
-   支持多Master部署。 |
|SmartData|-   升级至2.6.3版本。
-   支持多Master部署。 |
|Ranger|-   支持Solr组件。
-   支持PrestoSQL 311版本。 |
|Tez|支持scratchdir设置在OSS上。|
|Presto|升级至331版本。|
|Druid|升级至0.17.1版本。|
|Superset|升级至0.35.2版本。|
|Sqoop|-   MySql JDBC JAR包升级至5.1.48版本。
-   MySql direct导出模式支持通过`--mysql-charset`设置自定义编码。 |

