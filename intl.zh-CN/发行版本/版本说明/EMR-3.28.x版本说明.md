# EMR-3.28.x版本说明

本文介绍EMR-3.28.x发行版本的发布日期和更新内容等信息。

## 发布日期

EMR-3.28.0 2020年6月12日

## 新增内容

|服务|变更点|
|--|---|
|Bigboot|-   发布首个JindoTable版本，基于表或分区的热度统计。
-   支持Block模式上完整的存储策略，支持分层存储策略，包括低频和归档等。
-   增加数据迁移工具Jindo DistCp。
-   完善和修复Jindo Fuse。
-   完善Cache模式中JFS Scheme在Hive引擎和Jindo JobCommitter上的集成。
-   Block模式读路径上，设置比重可以直接读OSS，用来缓解和分摊读本地缓存的开销。
-   JindoFS软件模块解耦，分为Bigboot（管控层）、Smartdata（分布式服务）和JindoFS SDK。每块独立升级维护。 |

## 更新内容

|服务|变更点|
|--|---|
|Flink|已将开源Flink升级为企业版Ververica Platform，基于开源Flink 1.10深度定制，提供自研存储引擎Gemini等增值功能。|
|Bigboot|升级至2.7.0版本。|
|Delta|-   升级至0.6.0版本。
-   解耦Delta与Spark代码。 |
|Spark|-   升级至2.4.5版本。
-   兼容DataFactory的streaming-sql脚本。
-   支持Delta 0.6.0版本。 |
|Hive|支持Delta 0.6.0版本。|
|Ranger|-   支持HDFS、Hive和Spark自定义部署。
-   支持在控制台配置ranger-admin-site和ranger-ugsync-site。 |
|HDFS|针对HDFS写入时无可用DataNode节点的异常，打印对应DataNode异常信息（HDFS-9023）。|
|Hue|-   支持Gateway集群安装Hue组件。
-   支持在单个节点部署多个Hue实例。 |
|DataFactory|支持Delta 0.6.0版本。|
|Druid|升级至0.18.0版本。|
|Knox|-   升级至1.1.0-1.0.7版本。
-   适配HBase UI。 |

