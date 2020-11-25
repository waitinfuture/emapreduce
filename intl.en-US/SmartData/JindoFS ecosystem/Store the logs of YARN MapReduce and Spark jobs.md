# Store the logs of YARN MapReduce and Spark jobs

This topic describes how to store the logs of MapReduce and Spark jobs to JindoFileSystem \(JindoFS\) or Object Storage Service \(OSS\).

## Overview

E-MapReduce \(EMR\) clusters support the pay-as-you-go and subscription billing methods to meet different needs. Pay-as-you-go clusters can be released at any time. Hadoop clusters store logs in the Hadoop Distributed File System \(HDFS\) by default. If a pay-as-you-go cluster is released, you cannot query job logs of the cluster. In this case, you may have difficulties in troubleshooting job issues. This topic describes how to store the logs of MapReduce and Spark jobs to JindoFS or OSS so that you can query the previous logs of jobs.

## Configure JindoFS, YARN Container logs, and Spark History Server

-   JindoFS configuration

    |Configuration file|Parameter|Description|Example|
    |------------------|---------|-----------|-------|
    |bigboot|jfs.namespaces|The namespace supported by JindoFS. Separate multiple namespaces with commas \(,\).|emr-jfs|
    |jfs.namespaces.emr-jfs.uri|The storage backend of the emr-jfs namespace.|oss://oss-bucket/oss-dir|
    |jfs.namespaces.test.mode|The storage mode of the emr-jfs namespace.|block **Note:** JindoFS supports the block storage mode and cache mode. |

-   Configuration for YARN Container logs

    |Configuration file|Parameter|Description|Example|
    |------------------|---------|-----------|-------|
    |yarn-site|yarn.nodemanager.remote-app-log-dir|The directory in which YARN aggregates and stores logs after your application stops running. The log aggregation feature of YARN is enabled by default.|jfs://emr-jfs/emr-cluster-log/yarn-apps-logs or oss://$\{oss-bucket\}/emr-cluster-log/yarn-apps-logs|
    |mapred-site|mapreduce.jobhistory.done-dir|The directory in which JobHistory stores the logs of Hadoop jobs that are completed.|jfs://emr-jfs/emr-cluster-log/jobhistory/done or oss://$\{oss-bucket\}/emr-cluster-log/jobhistory/done|
    |mapreduce.jobhistory.intermediate-done-dir|The directory in which JobHistory stores the logs that are not archived for Hadoop jobs.|jfs://emr-jfs/emr-cluster-log/jobhistory/done\_intermediate or oss://$\{oss-bucket\}/emr-cluster-log/jobhistory/done\_intermediate|

-   Configuration for Spark History Server

    |Configuration file|Parameter|Description|Example|
    |------------------|---------|-----------|-------|
    |spark-defaults|spark\_eventlog\_dir|The directory in which Spark History Server stores the logs of Spark jobs.|jfs://emr-jfs/emr-cluster-log/spark-history or oss://$\{oss-bucket\}/emr-cluster-log/spark-history|


## Create a cluster

You can add custom software configurations when you create an EMR cluster, as shown in the following figure.

![config_sel](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7907409951/p63698.png)

## Custom configuration example

For example, to store logs in JindoFS, use the following custom configuration and replace the OSS bucket and relevant directories:

```
[   
  {
       "ServiceName":"BIGBOOT",
       "FileName":"bigboot",
       "ConfigKey":"jfs.namespaces",
       "ConfigValue":"emr-jfs"
  },
  {
       "ServiceName":"BIGBOOT",
       "FileName":"bigboot",
       "ConfigKey":"jfs.namespaces.emr-jfs.uri",
       "ConfigValue":"oss://oss-bucket/jindoFS"
  },
  {
       "ServiceName":"BIGBOOT",
       "FileName":"bigboot",
       "ConfigKey":"jfs.namespaces.emr-jfs.mode",
       "ConfigValue":"block"
  },
  {
       "ServiceName":"YARN",
       "FileName":"mapred-site",
       "ConfigKey":"mapreduce.jobhistory.done-dir",
       "ConfigValue":"jfs://emr-jfs/emr-cluster-log/jobhistory/done"
  },
  {
       "ServiceName":"YARN",
       "FileName":"mapred-site",
       "ConfigKey":"mapreduce.jobhistory.intermediate-done-dir",
       "ConfigValue":"jfs://emr-jfs/emr-cluster-log/jobhistory/done_intermediate"
  },
  {
       "ServiceName":"YARN",
       "FileName":"yarn-site",
       "ConfigKey":"yarn.nodemanager.remote-app-log-dir",
       "ConfigValue":"jfs://emr-jfs/emr-cluster-log/yarn-apps-logs"
  }, 
  {
       "ServiceName":"SPARK",
       "FileName":"spark-defaults",
       "ConfigKey":"spark_eventlog_dir",
       "ConfigValue":"jfs://emr-jfs/emr-cluster-log/spark-history"
  }
]
```

For example, to store logs in OSS, use the following custom configuration and replace the OSS bucket and relevant directories:

```
[   
  {
       "ServiceName":"YARN",
       "FileName":"mapred-site",
       "ConfigKey":"mapreduce.jobhistory.done-dir",
       "ConfigValue":"oss://oss_bucket/emr-cluster-log/jobhistory/done"
  },
  {
       "ServiceName":"YARN",
       "FileName":"mapred-site",
       "ConfigKey":"mapreduce.jobhistory.intermediate-done-dir",
       "ConfigValue":"oss://oss_bucket/emr-cluster-log/jobhistory/done_intermediate"
  },
  {
       "ServiceName":"YARN",
       "FileName":"yarn-site",
       "ConfigKey":"yarn.nodemanager.remote-app-log-dir",
       "ConfigValue":"oss://oss_bucket/emr-cluster-log/yarn-apps-logs"
  }, 
  {
       "ServiceName":"SPARK",
       "FileName":"spark-defaults", 
       "ConfigKey":"spark_eventlog_dir",
       "ConfigValue":"oss://oss_bucket/emr-cluster-log/spark-history"
   }
]
```

