# Hive作业配置

E-MapReduce默认提供了Hive环境，您可以直接使用Hive来创建和操作创建的表和数据。

-   已创建好项目，详情请参见[项目管理](/intl.zh-CN/数据开发/项目管理.md)。
-   已准备好Hive SQL的脚本，并上传到OSS的某个目录中（例如oss://path/to/uservisits\_aggre\_hdfs.hive）。

    uservisits\_aggre\_hdfs.hive内容如下。

    ```
    USE DEFAULT;
     DROP TABLE uservisits;
     CREATE EXTERNAL TABLE IF NOT EXISTS uservisits (sourceIP STRING,destURL STRING,visitDate STRING,adRevenue DOUBLE,userAgent STRING,countryCode STRING,languageCode STRING,searchWord STRING,duration INT) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' STORED AS SEQUENCEFILE LOCATION '/HiBench/Aggregation/Input/uservisits';
     DROP TABLE uservisits_aggre;
     CREATE EXTERNAL TABLE IF NOT EXISTS uservisits_aggre (sourceIP STRING, sumAdRevenue DOUBLE) STORED AS SEQUENCEFILE LOCATION '/HiBench/Aggregation/Output/uservisits_aggre';
     INSERT OVERWRITE TABLE uservisits_aggre SELECT sourceIP, SUM(adRevenue) FROM uservisits GROUP BY sourceIP;
    ```


## 操作步骤

1.  通过主账号登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com)。

2.  在顶部菜单栏处，根据实际情况选择地域（Region）和资源组。

3.  单击上方的**数据开发**页签。

4.  在**项目列表**页面，单击待编辑项目所在行的**作业编辑**。

5.  在**作业编辑**区域，在需要操作的文件夹上，右键选择**新建作业**。

6.  输入**作业名称**、**作业描述**，在**作业类型**下拉列表中选择**Hive**作业类型。

    表示创建的作业是一个Hive作业。这种类型的作业，其运行实际是通过以下方式提交的Hive作业。

    ```
    hive [user provided parameters]
    ```

7.  单击**确定**。

8.  在**作业内容**中，填写提交该作业需要提供的命令行参数。

    例如，如果需要使用刚刚上传到OSS的Hive脚本，则填写的内容如下。

    ```
    -f ossref://path/to/uservisits_aggre_hdfs.hive
    ```

    **说明：** `path`为`uservisits_aggre_hdfs.hive`在OSS上的路径。

    您也可以单击下方的**+插入OSS路径**，从OSS中进行浏览和选择，系统会自动补齐OSS上Hive脚本的路径。请务必将Hive脚本的前缀修改为**OSSREF**，以保证E-MapReduce可以正确下载该文件。

9.  单击**保存**，作业配置即定义完成。


