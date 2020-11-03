# EMR-4.4.0版本说明

本文介绍EMR-4.4.0发行版本的发布日期和更新内容等信息。

## 发布日期

EMR-4.4.0 2020年7月08日

## 更新内容

|服务|变更点|
|--|---|
|Spark|-   ThriftServer进程增加Java Agent。
-   优化Spark SQL的NOT IN子查询性能。
-   修复读取大量空文件时栈溢的出问题。
-   通过代码去除对HAS显式依赖或升级HAS依赖。 |
|Hive|-   升级至3.1.2版本。
-   优化JindoFS。
-   优化MSCK。
-   HCatalog支持JindoCommitter。
-   升级HAS依赖。 |
|Knox|-   适配Impala。
-   适配高版本Flink UI。 |
|Impala|升级至开源3.4版本，支持集成Ranger。|
|SmartData|升级至2.7.202版本。|
|Bigboot|
|Superset|升级至0.36.0版本。|
|HAS|升级至1.2.0版本，修复HAS与OpenJDK高版本的兼容性问题。|
|Livy|升级HAS依赖。|
|YARN|
|HDFS|-   修复OSS实现配置的缺陷。
-   升级HAS依赖。 |
|HBase|升级HAS依赖。|
|Tez|
|Oozie|

