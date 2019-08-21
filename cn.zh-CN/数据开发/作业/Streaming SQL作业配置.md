# Streaming SQL作业配置 {#task_1780555 .task}

本文介绍Streaming SQL作业配置的操作步骤。

-   已创建好项目，详情请参见[项目管理](cn.zh-CN/数据开发/项目管理.md#)。
-   已准备好作业要处理的数据以及作业所需的资源，如，配置Streaming SQL作业需要的依赖库文件。依赖库的详细内容请参考如下文档：《Spark Streaming SQL依赖库清单》。

## 步骤一：创建Streaming SQL作业 {#section_6te_poy_yrb .section}

1.  通过主账号登录[阿里云 E-MapReduce 控制台](https://emr.console.aliyun.com/)，进入集群列表页面。
2.  单击上方的数据开发页签，进入项目列表页面。
3.  单击对应项目右侧的**工作流设计**，然后在左侧导航栏中单击**作业编辑**。
4.  在作业编辑页面左侧，右键单击作业所属的文件夹并选择**新建作业**。 

    **说明：** 通过右键单击文件夹，您还可以进行创建子文件夹、重命名文件夹和删除文件夹操作。

5.  在弹出的新建作业对话框中，输入**作业名称**和**作业描述**，并从**作业类型**列表中选择**Streaming SQL**。![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1410430/156638796056337_zh-CN.png)

 

    Streaming SQL作业在E-MapReduce后台使用以下的方式提交：

    ``` {#codeblock_dfb_ffh_pej}
    streaming-sql -f {SQL_SCRIPT}
    ```

    **说明：** `SQL_SCRIPT`中保存着作业编辑器中填写的SQL语句。

6.  单击**确定**，完成Streaming SQL的作业创建。

## 步骤二：配置Streaming SQL作业 {#section_a4b_tzg_roy .section}

Streaming SQL作业创建完后，您需给作业配置内容。

1.  在作业内容输入框中输入Hive SQL语句，示例如下： 

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

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1410430/156638796056344_zh-CN.png)

2.  设置依赖库。 

    Streaming SQL作业需要依赖一些数据源相关的库文件。E-MapReduce将这些库以依赖库的形式发布在调度服务的仓库中，在创建作业时需要指定使用哪个版本的依赖库。

    设置路径为： 作业配置\>流任务配置\>依赖库。您只需设置相应的依赖库版本，如，sharedlibs:streamingsql:datasources-bundle:1.7.0。

3.  设置查询语句失败处理策略。 

    目前支持两种失败处理策略：

    -   继续执行下一条语句：如果查询语句执行失败， 继续执行下一条语句。
    -   终止当前作业： 如果查询作业失败，终止当前作业，包括之前已经启动了的流式任务。
4.  单击**保存**，Streaming SQL作业配置即定义完成。

