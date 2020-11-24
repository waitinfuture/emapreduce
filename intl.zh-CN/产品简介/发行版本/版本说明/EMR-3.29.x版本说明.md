# EMR-3.29.x版本说明

本文介绍EMR-3.29.x发行版本的发布日期和更新内容等信息。

## 发布日期

EMR-3.29.0 2020年7月29日

## 更新内容

|服务|变更点|
|--|---|
|Bigboot|-   升级至2.7.301版本。
-   JindoDistCp支持写入时按OSS归档或低频写入。
-   增强Fuse功能，支持多Namespaces。
-   完善Cache模式的元数据缓存功能。 |
|Spark|-   Spark升级至2.4.5.2.0。
-   支持第三方Metastore的功能。
-   增加datalake metastore-client。 |
|Hive|-   Hive升级至2.3.5.6.0。
-   支持第三方Metastore的功能。
-   增加datalake metastore-client。 |
|Presto|升级至338版本。|
|Ranger|-   升级软件包至1.2.0-1.5.0。
-   支持Presto 338。
-   配置文件增加Description。 |
|HDFS|自适应配置datanode reserved空间大小。|
|Knox|适配Impala、高版本Flink和PAI。|
|Druid|升级至0.18.1版本。|
|SmartData|升级至2.7.301版本。|

