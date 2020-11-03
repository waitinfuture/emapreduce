# EMR-4.4.1版本

本文介绍EMR-4.4.1发行版本的发布日期和更新内容等信息。

## 发布日期

EMR-4.4.1 2020年9月15日

## 更新内容

|服务|变更点|
|--|---|
|YARN|-   删除软件栈yarn.application.classpath配置中的hadoop/tools/lib目录。
-   优化MR作业默认的参数配置。 |
|Hive|优化默认的参数配置。|
|Tez|
|Ranger|-   支持Impala权限控制。
-   升级jackson-databind版本。 |
|Impala|-   支持集成Ranger。
-   升级Shiro至1.6.0版本。 |
|SmartData|升级至2.7.301版本。|
|Bigboot|
|Knox|-   支持Tez UI独立打开，支持YARN UI中的Tez。
-   升级Shiro至1.6.0版本。 |
|EMRDOCTOR|修复时间配置文件为空时，导致不采集作业信息的问题。|
|Ganglia|增加HDFS Service RPC Port的端口探测。|
|Oozie|-   修复Web UI无法打开的问题。
-   升级jackson-databind版本。 |
|Zookeeper|支持绑定内网IP启动服务端口。|
|Superset|修复启动脚本。|
|Livy|升级jackson-databind和fastjson版本。|
|Zepplin|升级jackson-databind和Shiro版本。|
|HAS|升级jackson-databind和fastjson版本。|
|Flume|升级fastjson版本。|

