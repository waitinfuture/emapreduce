# Spark SQL 作业配置 {#concept_vck_cjp_y2b .concept}

本文介绍 Spark SQL 作业配置的操作步骤。

**说明：** Spark SQL 提交作业的模式默认是 yarn-client 模式。

## 操作步骤 {#section_ws2_gjp_y2b .section}

1.  通过主账号登录[阿里云 E-MapReduce 控制台](https://emr.console.aliyun.com/)，进入集群列表页面。
2.  单击上方的数据开发页签，进入项目列表页面。
3.  单击对应项目右侧的**工作流设计**，在左侧导航栏中单击**作业编辑**进入作业编辑页面。
4.  在页面左侧，在需要操作的文件夹上单击右键，选择**新建作业**。
5.  填写作业名称，作业描述。
6.  选择 Spark SQL 作业类型，表示创建的作业是一个 Spark SQL 作业。Spark SQL 作业在 E-MapReduce 后台使用以下的方式提交：

    ```
    spark-sql [options] [cli option]spark-sql [options] -e {SQL_CONTENT}                    
    ```

    -   options： 通过在作业配置-\>高级配置-\>添加环境变量 SPARK\_CLI\_PARAMS 来设置， 如SPARK\_CLI\_PARAMS="--executor-memory 1g --executor-cores
    -   SQL\_CONTENT：作业编辑器中填写的 SQL 语句。
7.  单击**确定**。

    **说明：** 您还可以通过在文件夹上单击右键，进行创建子文件夹、重命名文件夹和删除文件夹操作。

8.  在**作业内容**输入框中填入 Spark SQL 语句，如：

    ``` {#codeblock_81y_h6g_sly}
    -- SQL语句示例
    -- SQL语句最大不能超过64KB
    show databases;
    show tables;
    -- 系统会自动为SELECT语句加上'limit 2000'的限制
    select * from test1;
    ```

9.  单击**保存**，Spark SQL 作业即定义完成。

