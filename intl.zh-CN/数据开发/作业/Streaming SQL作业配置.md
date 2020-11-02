# Streaming SQL作业配置

本文介绍Streaming SQL作业配置的操作步骤。

-   已创建项目，详情请参见[项目管理](/intl.zh-CN/数据开发/项目管理.md)。
-   已获取作业所需的资源和数据文件。例如，JAR包、数据文件名称以及两者的保存路径。

Streaming SQL的详细信息请参见[Spark Streaming SQL](/intl.zh-CN/开发指南/Spark Streaming SQL/简介.md)。

在Streaming SQL作业配置过程中，您需要设置依赖库。以下列出了Spark Streaming SQL提供的数据源依赖包的版本信息和使用说明，建议使用最新版本。

|库名称|版本|发布日期|引用字符串|详细信息|
|---|--|----|-----|----|
|datasources-bundle|2.0.0（推荐）|2020/02/26|sharedlibs:streamingsql:datasources-bundle:2.0.0|支持数据源：Kafka、Loghub、Druid、TableStore、HBase、JDBC、Datahub、Redis、Kudu和DTS。|
|1.9.0|2019/11/20|sharedlibs:streamingsql:datasources-bundle:1.9.0|支持数据源：Kafka、Loghub、Druid、TableStore、HBase、JDBC、Datahub、Redis和Kudu。|
|1.8.0|2019/10/17|sharedlibs:streamingsql:datasources-bundle:1.8.0|支持数据源：Kafka、Loghub、Druid、TableStore、HBase、JDBC、Datahub和Redis。|
|1.7.0|2019/07/29|sharedlibs:streamingsql:datasources-bundle:1.7.0|支持数据源：Kafka、Loghub、Druid、TableStore、HBase和JDBC。|

如果需要了解更详细的使用方法，请参见[数据源](/intl.zh-CN/开发指南/Spark Streaming SQL/数据源/数据源支持概述.md)。

## 操作步骤

1.  新建作业。

    1.  通过主账号登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com)。

    2.  在顶部菜单栏处，根据实际情况选择地域（Region）和资源组。

    3.  单击上方的数据开发页签。

    4.  在**项目列表**页面，单击对应项目所在行的**作业编辑**。

    5.  在**作业编辑**区域，在需要操作的文件夹上，右键选择**新建作业**。

        **说明：** 您还可以通过在文件夹上单击右键，执行新建子文件夹、重命名文件夹和删除文件夹操作。

    6.  输入**作业名称**、**作业描述**，在**作业类型**下拉列表中选择**Streaming SQL**作业类型。

    7.  单击**确定**。

2.  在**作业内容**中，填写提交该作业需要提供的命令行参数。

    示例如下。

    ```
    --- 创建SLS数据表。 
    CREATE TABLE IF NOT EXISTS ${slsTableName} 
       USING loghub 
       OPTIONS ( 
            sls.project = '${logProjectName}', 
            sls.store = '${logStoreName}', 
            access.key.id = '${accessKeyId}', 
            access.key.secret = '${accessKeySecret}', 
            endpoint = '${endpoint}'
       ); 
    --- 导入数据至HDFS。
    INSERT INTO 
        ${hdfsTableName} 
    SELECT 
        col1, col2 
    FROM  ${slsTableName} 
    WHERE ${condition}
    ```

    **说明：** 此类型的作业是通过`streaming-sql -f {sql_script}`提交的。`sql_script`中保存着作业编辑器中填写的SQL语句。

3.  配置依赖库和失败策略。

    1.  单击右上方的**作业设置**。

    2.  分别在**流任务设置**和**共享库**页面，配置作业的依赖库和失败策略。

        |区域|配置项|说明|
        |--|---|--|
        |**失败处理策略**|**当前语句执行失败时**|当前语句执行失败时，支持如下策略：         -   **继续执行下一条语句**：如果查询语句执行失败， 继续执行下一条语句。
        -   **终止当前作业**：如果查询语句执行失败， 终止当前作业。 |
        |**依赖库**|**库列表**|执行作业需要依赖一些数据源相关的库文件。E-MapReduce将这些库以依赖库的形式发布在调度服务的仓库中，在创建作业时需要指定使用哪个版本的依赖库。您只需设置相应的依赖库版本，例如sharedlibs:streamingsql:datasources-bundle:2.0.0。 |

4.  单击**保存**，作业配置即定义完成。


