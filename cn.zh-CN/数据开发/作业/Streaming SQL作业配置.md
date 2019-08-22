# Streaming SQL作业配置 {#task_1780555 .task}

本文介绍Streaming SQL作业配置的操作步骤。

-   已创建好项目，详情请参见[项目管理](cn.zh-CN/数据开发/项目管理.md#)。
-   已获取Spark Streaming SQL的依赖库，详情请参见下面的**背景信息**。

Streaming SQL的详细信息请参见[Spark Streaming SQL](../../../../cn.zh-CN/开发指南/Spark Streaming SQL/前言.md#)。

在Streaming SQL作业配置过程中，您需要设置依赖库。以下列出了Spark Streaming SQL提供的数据源依赖包的版本信息和使用说明，原则上需要使用最新版本。

|库名称|版本|发布日期|引用字符串|详细信息|
|---|--|----|-----|----|
|datasources-bundle|1.7.0|2019/07/29|sharedlibs:streamingsql:datasources-bundle:1.7.0|支持数据源：Kafka、Loghub、Druid、TableStore、HBase和JDBC|

-   引用字符串在数据开发的**作业设置** \> **流任务设置** \> **依赖库**中使用。
-   以上所注明支持的数据源，特指数据源支持了流式读写。
-   如果需要了解更详细的使用方法，请参见[数据源](../../../../cn.zh-CN/开发指南/Spark Streaming SQL/数据源/数据源支持概述.md#)。

## 步骤一：创建Streaming SQL作业 {#section_6te_poy_yrb .section}

1.  通过主账号登录[阿里云 E-MapReduce 控制台](https://emr.console.aliyun.com/)，进入集群列表页面。
2.  单击上方的数据开发页签，进入项目列表页面。
3.  单击对应项目右侧的**工作流设计**，然后在左侧导航栏中单击**作业编辑**。
4.  在作业编辑页面左侧，右键单击作业所属的文件夹并选择**新建作业**。 

    **说明：** 通过右键单击文件夹，您还可以进行创建子文件夹、重命名文件夹和删除文件夹操作。

5.  在弹出的新建作业对话框中，输入**作业名称**和**作业描述**，并从**作业类型**列表中选择**Streaming SQL**。![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1410430/156646072456337_zh-CN.png)


6.  单击**确定**，完成Streaming SQL的作业创建。 作业创建完成后，自动进入该作业，您可根据实际需要配置作业的代码。

## 步骤二：配置作业Hive SQL语句 {#section_a4b_tzg_roy .section}

在E-MapReduce后台，Streaming SQL作业的提交方式是`streaming-sql -f {SQL_SCRIPT}`，其中`SQL_SCRIPT`中保存的即是Streaming SQL作业的代码，即Hive SQL语句。

1.  创建作业完成后，在作业内容文本框中输入Hive SQL语句。 

    Hive SQL语句示例：

    ``` {#codeblock_h5c_qkn_eba}
    --- 创建SLS数据表 
    CREATE TABLE IF NOT EXISTS ${slsTableName} 
       USING loghub 
       OPTIONS ( 
            sls.project = '${logProjectName}', 
            sls.store = '${logStoreName}', 
            access.key.id = '${accessKeyId}', 
            access.key.secret = '${accessKeySecret}', 
            endpoint = '${endpoint}'
       ); 
    --- 将数据导入HDFS 
    INSERT INTO 
        ${hdfsTableName} 
    SELECT 
        col1, col2 
    FROM  ${slsTableName} 
    WHERE ${condition}
    ```

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1410430/156646072456344_zh-CN.png)


## 步骤三：配置依赖库和失败策略 {#section_sa5_tmj_fdd .section}

依赖库：Streaming SQL作业需要依赖一些数据源相关的库文件。E-MapReduce将这些库以依赖库的形式发布在调度服务的仓库中，在创建作业时需要指定使用哪个版本的依赖库。

失败策略：当前语句执行失败时的执行策略。

1.  完成作业代码配置后，单击右上方的**作业设置**，然后选择**流任务设置**。
2.  在流任务设置页面配置作业的依赖库和失败策略。 

    |区域|配置项|说明|
    |--|---|--|
    |**失败处理策略**|**当前语句执行失败时**|当前语句执行失败时，支持如下策略：     -   **继续执行下一条语句**：如果查询语句执行失败， 继续执行下一条语句。
    -   **终止当前作业**：如果查询语句执行失败， 终止当前作业。
 |
    |**依赖库**|**库列表**|您只需设置相应的依赖库版本，例如sharedlibs:streamingsql:datasources-bundle:1.7.0。|

3.  单击**保存**，Streaming SQL作业配置即定义完成。

