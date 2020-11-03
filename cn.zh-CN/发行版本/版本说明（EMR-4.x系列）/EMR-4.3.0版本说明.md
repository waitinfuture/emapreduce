# EMR-4.3.0版本说明

本文介绍EMR-4.3.0发行版本的发布日期和更新内容等信息。

## 发布日期

EMR-4.3.0 2020年5月20日

## 更新内容

|服务|变更点|
|--|---|
|Ranger|-   支持HDFS、Hive、Spark plugin自定义部署，在对应服务节点执行plugin enable操作。
-   支持在控制台配置ranger-admin和ranger-usersync。 |
|Presto|升级Kudu Client。|
|Spark|-   升级至2.4.5版本。
-   升级关联的Delta Lake至0.6.0版本。
-   修复开启Ranger Hive后，Pyspark无法正常运行的缺陷。 |
|HDFS|-   修复HDFS\_NAMENODE\_OPTS参数无法生效的缺陷。
-   支持自定义部署。 |
|YARN|支持自定义部署。|
|Hive|支持自定义部署。|
|Knox|适配Hadoop 3.x中HDFS的NameNode UI。|
|Zeppelin|修复生成zepping.keytab时失败的缺陷。|
|Kafka|升级至2.4.1版本。|
|Kudu|升级至1.11.1版本。|
|Impala|修复haproxy问题。|
|Livy|修复xmllint问题。|
|HUE|-   支持Gateway安装HUE组件。
-   支持单个节点开启多个HUE实例。 |

