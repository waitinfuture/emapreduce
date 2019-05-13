# 集群的 HDFS 容量被写满，发现/spark-history下面有大量的数据 {#concept_pqs_v2c_ggb .concept}

在 Spark 的配置管理页面可以查看 Spark history 的 cleaner（spark.history.fs.cleaner）是否有打开，如果没有打开，可以打开，这样能够周期性清理已经完成的作业的日志数据。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/83045/155773782335146_zh-CN.png)

同时，可以清理掉 HDFS 路径 /spark-hostory 下那些已经完成的作业的日志（不带 in-progress 的目录）。

如果有大量的 long running 的 Spark streaming 作业，可以把 eventLog（spark.eventLog.enabled）关掉，不然只要作业不停止，eventLog 数据会一直增加。如果页面上没有这个配置项，可以自定义一个配置项，保存，配置到主机。配置需改完之后，对于正在运行的 Spark streaming 作业，需要重启才能生效。

