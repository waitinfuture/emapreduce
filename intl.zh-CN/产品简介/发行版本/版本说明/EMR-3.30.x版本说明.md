# EMR-3.30.x版本说明

本文介绍EMR-3.30.x发行版本的发布日期和更新内容等信息。

## 发布日期

EMR-3.30.0 2020年10月26日

## 更新内容

|服务|变更点|
|--|---|
|SmartData|升级至3.0.0。|
|Spark|-   支持阿里云DLF（Data Lake Formation）元数据。
-   升级HAS依赖至2.0.1。
-   修复Streaming SQL反引号问题。
-   移除Delta的JAR包，修改为Delta单独部署。
-   修改日志路径统一写至HDFS下。 |
|Hive|-   支持阿里云DLF（Data Lake Formation）元数据。
-   解决了读Delta表空目录时写DUMMY文件问题。
-   升级HAS依赖至2.0.1。 |
|Presto|-   支持阿里云DLF（Data Lake Formation）元数据。
-   解决读Delta表的限制问题。
-   修复高安全模式下JVM配置缺失问题。
-   升级HAS依赖至2.0.1。 |
|HDFS|-   支持热交换磁盘模式。
-   升级HAS依赖至2.0.1。 |
|YARN|-   修复YARN RMZKStateStore的问题。
-   支持SLS输出的SNAPPY文件。
-   修改MapReduce Local模式目录配置，解决目录权限检查问题。
-   支持热交换磁盘模式。
-   日志路径统一写到HDFS下。
-   升级HAS依赖至2.0.1。 |
|Zookeeper|-   支持绑定内网IP启动服务端口。
-   升级HAS依赖至2.0.1。 |
|Flink-Vvp|-   升级至1.11-2.2.2版本。
-   支持SQL和Autopilot功能。

**说明：** 仅Dataflow集群支持Flink-Vvp，Hadoop集群暂不支持Flink-Vvp。 |
|Flink|-   支持缓存模式写入OSS，结合Flink的Checkpoint与可重发的Source实现EXACTLY\_ONCE语义。
-   同步了Flink社区1.11.1功能，SQL支持多路输出（MULTI INSERT）。
-   升级HAS依赖至2.0.1。 |
|Impala|-   支持自定义配置catalogd.flgs、impalad.flgs和statestored.flgs。
-   升级Shiro至1.6.0版本。
-   升级HAS依赖至2.0.1。 |
|Tez|-   优化AM的默认内存参数。
-   升级HAS依赖至2.0.1。 |
|HAS|升级HAS依赖至2.0.1。|
|Storm|
|Zeppelin|
|Ranger|
|OpenLDAP|
|Oozie|
|Knox|
|Kafka|
|HUE|
|HBase|
|Druid|

