# 基于JindoFS存储YARN MR/SPARK作业日志

本文介绍如何将MapReduce、Spark作业日志配置到JindoFS/OSS上。

## 概述

E-MapReduce集群支持按量计费以及包年包月的付费方式，满足不同用户的使用需求。对于按量计费的集群随时会被释放，而Hadoop默认会把日志存储在HDFS上，当集群释放以后，按量计费的用户就无法查询作业的日志了，这也给按量计费用户排查作业问题带来了困难。本文会介绍如何将MapReduce、Spark作业日志配置到JindoFS/OSS上，集群重新创建以后，也可以继续查询之前作业相关的日志。

## JindoFS、YARN Container日志和Spark HistoryServer配置

-   JindoFS配置

    |配置文件|参数|参数说明|示例|
    |----|--|----|--|
    |bigboot|jfs.namespaces|表示当前JindoFS支持的命名空间，多个命名空间时以逗号隔开。|emr-jfs|
    |jfs.namespaces.emr-jfs.uri|表示emr-jfs命名空间的后端存储。|oss://oss-bucket/oss-dir|
    |jfs.namespaces.test.mode|表示emr-jfs命名空间为块存储模式。|block **说明：** JindoFS支持block和cache两种存储模式。 |

-   YARN Container日志配置

    |配置文件|参数|参数说明|示例|
    |----|--|----|--|
    |yarn-site|yarn.nodemanager.remote-app-log-dir|当应用程序运行结束后，日志聚合的存储位置，YARN日志聚合功能默认已打开。|jfs://emr-jfs/emr-cluster-log/yarn-apps-logs或者oss://$\{oss-bucket\}/emr-cluster-log/yarn-apps-logs|
    |mapred-site|mapreduce.jobhistory.done-dir|JobHistory存放已经运行完的Hadoop作业记录的目录。|jfs://emr-jfs/emr-cluster-log/jobhistory/done或者oss://$\{oss-bucket\}/emr-cluster-log/jobhistory/done|
    |mapreduce.jobhistory.intermediate-done-dir|JobHistory存放未归档的 Hadoop作业记录的目录。|jfs://emr-jfs/emr-cluster-log/jobhistory/done\_intermediate或者oss://$\{oss-bucket\}/emr-cluster-log/jobhistory/done\_intermediate|

-   Spark HistoryServer配置

    |配置文件|参数|参数说明|示例|
    |----|--|----|--|
    |spark-defaults|spark\_eventlog\_dir|存放Spark作业历史的目录。|jfs://emr-jfs/emr-cluster-log/spark-history或者oss://$\{oss-bucket\}/emr-cluster-log/spark-history|


## 创建集群

添加软件自定义配置，如下图所示。

![config_sel](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3357459951/p63698.png)

## JindoFS样例

以JindoFS存储日志为例，替换oss\_bucket及对应路径，自定义配置如下：

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

以OSS存储日志为例，替换oss\_bucket及对应路径，自定义配置如下：

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

