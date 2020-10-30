# Jindo Job Committer使用说明

本文主要介绍JindoOssCommitter的使用说明。

Job Committer是MapReduce和Spark等分布式计算框架的一个基础组件，用来处理分布式任务写数据的一致性问题。

Jindo Job Committer是阿里云E-MapReduce针对OSS场景开发的高效Job Committer的实现，基于OSS的Multipart Upload接口，结合OSS Filesystem层的定制化支持。使用Jindo Job Committer时，Task数据直接写到最终目录中，在完成Job Commit前，中间数据对外不可见，彻底避免了Rename操作，同时保证数据的一致性。

**说明：**

-   OSS拷贝数据的性能，针对不同的用户或Bucket会有差异，可能与OSS带宽以及是否开启某些高级特性等因素有关，具体问题可以咨询OSS的技术支持。
-   在所有任务都完成后，MapReduce Application Master或Spark Driver执行最终的Job Commit操作时，会有一个短暂的时间窗口。时间窗口的大小和文件数量线性相关，可以通过增大`fs.oss.committer.threads`可以提高并发处理的速度。
-   Hive和Presto等没有使用Hadoop的Job Committer。
-   E-MapReduce集群中默认打开Jindo Oss Committer的参数。

## 在MapReduce中使用Jindo Job Committer

1.  进入YARN服务的**mapred-site**页签。

    1.  登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com/)。

    2.  在顶部菜单栏处，根据实际情况选择地域（Region）和资源组。

    3.  单击上方的**集群管理**页签。

    4.  在**集群管理**页面，单击相应集群所在行的**详情**。

    5.  在左侧导航栏单击**集群服务** \> **YARN**。

    6.  单击**配置**页签。

    7.  在**服务配置**区域，单击**mapred-site**页签。

2.  针对Hadoop不同版本，在YARN服务中配置以下参数。

    -   Hadoop 2.x版本

        在YARN服务的**mapred-site**页签，设置**mapreduce.outputcommitter.class**为com.aliyun.emr.fs.oss.commit.JindoOssCommitter。

    -   Hadoop 3.x版本

        在YARN服务的**mapred-site**页签，设置**mapreduce.outputcommitter.factory.scheme.oss**为com.aliyun.emr.fs.oss.commit.JindoOssCommitterFactory。

3.  保存配置。

    1.  单击右上角的**保存**。

    2.  在**确认修改**对话框中，输入执行原因，开启**自动更新配置**。

    3.  单击**确定**。

4.  进入SmartData服务的**smartdata-site**页签。

    1.  在左侧导航栏单击**集群服务** \> **SmartData**。

    2.  单击**配置**页签。

    3.  在**服务配置**区域，单击**smartdata-site**页签。

5.  在SmartData服务的**smartdata-site**页签，设置**fs.oss.committer.magic.enabled**为true。

6.  保存配置。

    1.  单击右上角的**保存**。

    2.  在**确认修改**对话框中，输入执行原因，开启**自动更新配置**。

    3.  单击**确定**。


**说明：** 在设置**mapreduce.outputcommitter.class**为com.aliyun.emr.fs.oss.commit.JindoOssCommitter后，可以通过开关**fs.oss.committer.magic.enabled**便捷地控制所使用的Job Committer。当打开时，MapReduce任务会使用无需Rename操作的Jindo Oss Magic Committer，当关闭时，JindoOssCommitter和FileOutputCommitter行为一样。

## 在Spark中使用Jindo Job Committer

1.  进入Spark服务的**spark-defaults**页签。

    1.  在左侧导航栏单击**集群服务** \> **Spark**。

    2.  单击**配置**页签。

    3.  在**服务配置**区域，单击**spark-defaults**页签。

2.  在Spark服务的**spark-defaults**页签，设置以下参数。

    |参数|参数值|
    |--|---|
    |**spark.sql.sources.outputCommitterClass**|com.aliyun.emr.fs.oss.commit.JindoOssCommitter|
    |**spark.sql.parquet.output.committer.class**|com.aliyun.emr.fs.oss.commit.JindoOssCommitter|
    |**spark.sql.hive.outputCommitterClass**|com.aliyun.emr.fs.oss.commit.JindoOssCommitter|

    这三个参数分别用来设置写入数据到Spark DataSource表、Spark Parquet格式的DataSource表和Hive表时使用的Job Committer。

3.  保存配置。

    1.  单击右上角的**保存**。

    2.  在**确认修改**对话框中，输入执行原因，开启**自动更新配置**。

    3.  单击**确定**。

4.  进入SmartData服务的**smartdata-site**页签。

    1.  在左侧导航栏单击**集群服务** \> **SmartData**。

    2.  单击**配置**页签。

    3.  在**服务配置**区域，单击**smartdata-site**页签。

5.  在SmartData服务的**smartdata-site**页签，设置**fs.oss.committer.magic.enabled**为true。

    **说明：** 您可以通过开关`fs.oss.committer.magic.enabled`便捷地控制所使用的Job Committer。当打开时，Spark任务会使用无需Rename操作的Jindo Oss Magic Committer，当关闭时，JindoOssCommitter和FileOutputCommitter行为一样。

6.  保存配置。

    1.  单击右上角的**保存**。

    2.  在**确认修改**对话框中，输入执行原因，开启**自动更新配置**。

    3.  单击**确定**。


## 优化Jindo Job Committer性能

当MapReduce或Spark任务写大量文件的时候，您可以调整MapReduce Application Master或Spark Driver中并发执行Commit相关任务的线程数量，提升Job Commit性能。

1.  进入SmartData服务的**smartdata-site**页签。

    1.  在左侧导航栏单击**集群服务** \> **SmartData**。

    2.  单击**配置**页签。

    3.  在**服务配置**区域，单击**smartdata-site**页签。

2.  在SmartData服务的**smartdata-site**页签，设置**fs.oss.committer.threads**为8。

    默认值为8。

3.  保存配置。

    1.  单击右上角的**保存**。

    2.  在**确认修改**对话框中，输入执行原因，开启**自动更新配置**。

    3.  单击**确定**。


