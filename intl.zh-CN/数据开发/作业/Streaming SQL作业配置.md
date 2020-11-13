# Streaming SQL作业配置

本文介绍Streaming SQL作业配置的操作步骤。

-   已创建好项目，详情请参见[项目管理](/intl.zh-CN/数据开发/项目管理.md)。
-   已获取作业所需的资源，以及作业要处理的数据文件，例如，JAR包、数据文件名称，以及两者的保存路径。

Streaming SQL的详细信息请参见[Spark Streaming SQL]()。

在Streaming SQL作业配置过程中，您需要设置依赖库。以下列出了Spark Streaming SQL提供的数据源依赖包的版本信息和使用说明，原则上需要使用最新版本。

|库名称|版本|发布日期|引用字符串|详细信息|
|---|--|----|-----|----|
|datasources-bundle|1.9.0|2019/11/20|sharedlibs:streamingsql:datasources-bundle:1.9.0|支持数据源：Kafka、Loghub、Druid、TableStore、HBase、JDBC、Datahub、Redis和Kudu。|
|1.8.0|2019/10/17|sharedlibs:streamingsql:datasources-bundle:1.8.0|支持数据源：Kafka、Loghub、Druid、TableStore、HBase、JDBC、Datahub和Redis。|
|1.7.0|2019/07/29|sharedlibs:streamingsql:datasources-bundle:1.7.0|支持数据源：Kafka、Loghub、Druid、TableStore、HBase和JDBC。|

-   引用字符串在数据开发的**作业设置** \> **流任务设置** \> **依赖库**中使用。
-   以上所注明支持的数据源，特指数据源支持了流式读写。
-   如果需要了解更详细的使用方法，请参见[数据源](/intl.zh-CN/开发指南/Spark Streaming SQL/数据源/数据源支持概述.md)。

## 操作步骤

1.  已通过主账号登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com)。

2.  输入**作业名称**、**作业描述**，选择Streaming SQL作业类型。

3.  单击**确定**。

4.  在**作业内容**中，填写提交该作业需要提供的命令行参数。

    示例如下：

    ```
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

    **说明：** 这种类型的作业，其运行实际是通过`streaming-sql -f {sql_script}`提交的作业，其中`sql_script`中保存的即是Streaming SQL作业的代码，即Streaming SQL语句。

5.  配置依赖库和失败策略。

    1.  单击右上方的**作业设置**，然后选择**流任务设置**。

    2.  在流任务设置页面配置作业的依赖库和失败策略。

        |区域|配置项|说明|
        |--|---|--|
        |**失败处理策略**|**当前语句执行失败时**|当前语句执行失败时，支持如下策略：         -   **继续执行下一条语句**：如果查询语句执行失败， 继续执行下一条语句。
        -   **终止当前作业**：如果查询语句执行失败， 终止当前作业。 |
        |**依赖库**|**库列表**|Streaming SQL作业需要依赖一些数据源相关的库文件。E-MapReduce将这些库以依赖库的形式发布在调度服务的仓库中，在创建作业时需要指定使用哪个版本的依赖库。

 您只需设置相应的依赖库版本，例如sharedlibs:streamingsql:datasources-bundle:1.9.0。 |

6.  单击**保存**，作业配置即定义完成。


