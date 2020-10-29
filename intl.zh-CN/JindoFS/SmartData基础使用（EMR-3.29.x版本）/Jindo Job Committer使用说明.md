# Jindo Job Committer使用说明

本文主要介绍JindoOssCommitter的使用说明。

Job Committer是MapReduce或Spark等分布式计算框架的一个基础组件，用来处理分布式任务写数据的一致性问题。MapReduce和Spark默认使用FileOutputCommitter的基本实现方式是：Task将数据写到task临时目录，在task完成时，将文件移动到job临时目录，最后在所有task都完成后，在MapReduce Application Master或者Spark Driver中，将文件从job临时目录移动到最终目录。文件的移动是通过rename实现的，对于HDFS来说，rename的代价很小，这种commit方式安全且高效。但是对于OSS对象存储系统来说，rename相当于一次拷贝+删除的操作，job commit的时间随着分布式任务写出数据的增加线性增长，对于大数据量任务，这种job commit方式基本不太可用。

Jindo Job Committer是阿里云EMR针对OSS场景开发的一个高效的Job Committer实现，基于OSS的multipart upload接口，结合OSS Filesystem层的定制化支持，使用Jindo Job Committer时，task数据直接写到最终目录中，且在完成job commit前，中间数据对外不可见，彻底避免了rename操作，同时保证数据的一致性。

**说明：**

-   OSS拷贝数据的性能，针对不同的用户或Bucket可能有差异，和OSS带宽、是否开启某些高级特性等诸多因素有关，具体问题可咨询OSS的技术支持。
-   在所有任务都完成后，MapReduce Application Master或Spark Driver执行最终的job commit操作时，会有一个短暂的时间窗口，在这个时间窗口因为节点crash等其他因素导致任务失败时，无法保证写出数据的一致性，可能会出现部分结果文件在最终目录中的情况。时间窗口的大小和文件数量线性相关，通过增大`fs.oss.committer.threads`可以提高并发处理的速度。
-   Hive，Presto等没有使用Hadoop的Job Committer，而是使用了一套自己写出数据一致性保证的机制，所以无法使用Jindo Oss Committer优化。
-   Jindo Oss Committer相关参数在EMR集群中默认打开。

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

        在YARN服务的**mapred-site**页签，设置**mapreduce.outputcommitter.class**为**com.aliyun.emr.fs.oss.commit.JindoOssCommitter**。

    -   Hadoop 3.x版本

        在YARN服务的**mapred-site**页签，设置**mapreduce.outputcommitter.factory.scheme.oss**为**com.aliyun.emr.fs.oss.commit.JindoOssCommitterFactory**。

3.  保存配置。

    1.  单击右上角的**保存**。

    2.  在**确认修改**对话框中，输入执行原因，开启**自动更新配置**。

    3.  单击**确定**。

4.  进入SmartData服务的**smartdata-site**页签。

    1.  在左侧导航栏单击**集群服务** \> **SmartData**。

    2.  单击**配置**页签。

    3.  在**服务配置**区域，单击**smartdata-site**页签。

5.  在SmartData服务的**smartdata-site**页签，设置**fs.oss.committer.magic.enabled**为**true**。

6.  保存配置。

    1.  单击右上角的**保存**。

    2.  在**确认修改**对话框中，输入执行原因，开启**自动更新配置**。

    3.  单击**确定**。


**说明：** 在设置**mapreduce.outputcommitter.class**为**com.aliyun.emr.fs.oss.commit.JindoOssCommitter**后，可以通过开关**fs.oss.committer.magic.enabled**便捷地控制所使用的job committer。当打开时，MapReduce任务会使用无需Rename操作的Jindo Oss “Magic” Committer，当关闭时，JindoOssCommitter行为会和FileOutputCommitter一样。

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

    这三个参数分别用来设置写入数据到Spark DataSource表，Spark Parquet格式的DataSource表和Hive表时使用的job committer，可根据需要具体设置。

3.  保存配置。

    1.  单击右上角的**保存**。

    2.  在**确认修改**对话框中，输入执行原因，开启**自动更新配置**。

    3.  单击**确定**。

4.  进入SmartData服务的**smartdata-site**页签。

    1.  在左侧导航栏单击**集群服务** \> **SmartData**。

    2.  单击**配置**页签。

    3.  在**服务配置**区域，单击**smartdata-site**页签。

5.  在SmartData服务的**smartdata-site**页签，设置**fs.oss.committer.magic.enabled**为**true**。

    **说明：** 可以通过开关`fs.oss.committer.magic.enabled`便捷地控制所使用的job committer。当打开时，Spark任务会使用无需Rename操作的Jindo Oss “Magic” Committer，当关闭时，JindoOssCommitter行为会和FileOutputCommitter一样。

6.  保存配置。

    1.  单击右上角的**保存**。

    2.  在**确认修改**对话框中，输入执行原因，开启**自动更新配置**。

    3.  单击**确定**。


## 优化Jindo Job Committer性能

当MapReduce或Spark任务写大量文件的时候，可以调整MapReduce Application Master或Spark Driver中并发执行commit相关任务的线程数量，提升job commit性能。

1.  进入SmartData服务的**smartdata-site**页签。

    1.  在左侧导航栏单击**集群服务** \> **SmartData**。

    2.  单击**配置**页签。

    3.  在**服务配置**区域，单击**smartdata-site**页签。

2.  在SmartData服务的**smartdata-site**页签，设置**fs.oss.committer.threads**为**8**。

    默认值为8。

3.  保存配置。

    1.  单击右上角的**保存**。

    2.  在**确认修改**对话框中，输入执行原因，开启**自动更新配置**。

    3.  单击**确定**。


