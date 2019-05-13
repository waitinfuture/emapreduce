# HDFS capacity of a cluster is full with large amounts of data stored in the /spark-history directory {#concept_pqs_v2c_ggb .concept}

You can enable the cleaner \(spark.history.fs.cleaner\) of Spark history on the Spark configuration management page to clean up event logs for completed jobs periodically.![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/83045/155773782735146_en-US.png)

The event logs to clean up include logs \(in-progress logs excluded\) that are stored in the /spark-history directory of HDFS.

If your cluster has many long-running Spark Streaming jobs, set the spark.eventLog.enabled property to false to avoid increasing event logs. If you cannot find the spark.eventLog.enabled configuration option on the page, create a custom configuration file on the server. For the configurations to take effect, restart the Spark Streaming jobs.

