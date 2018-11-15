# Spark SQL 作业配置 {#concept_vck_cjp_y2b .concept}

本文将介绍Spark SQL作业配置的操作步骤。

**说明：** Spark SQL 提交作业的模式默认是 yarn-client 模式。

## 操作步骤 {#section_ws2_gjp_y2b .section}

1.  通过主账号登录[阿里云 E-MapReduce 控制台](https://emr.console.aliyun.com/)，进入集群列表页面。
2.  单击上方的数据开发页签，进入项目列表页面。
3.  单击对应项目右侧的**工作流设计**，进入作业编辑页面。
4.  在页面左侧，在需要操作的文件夹上单击右键，选择**新建作业**。
5.  填写作业名称，作业描述。
6.  选择 Spark SQL 作业类型，表示创建的作业是一个 Spark SQL 作业。Spark SQL 作业在 E-MapReduce 后台使用以下的方式提交：

    ```
    spark-sql [options] [cli option]
    ```

7.  单击**确定**。

    **说明：** 您还可以通过在文件夹上单击右键，进行创建子文件夹、重命名文件夹和删除文件夹操作。

8.  在**作业内容**输入框中填入 Spark SQL 命令后续的参数。
    -   -e 选项

        -e 选项可以直接写运行的 SQL，在作业**作业内容**输入框中直接输入，如下所示：

        ```
        -e "show databases;"
        ```

    -   -f 选项

        -f 选项可以指定 Spark SQL 的脚本文件。通过将编写好的 Spark SQL 脚本文件放在 OSS 上，可以更灵活，建议您使用这种运行方式。如下所示：

        ```
        -f ossref://your-bucket/your-spark-sql-script.sql
        ```

9.  单击**保存**，Spark SQL 作业即定义完成。

