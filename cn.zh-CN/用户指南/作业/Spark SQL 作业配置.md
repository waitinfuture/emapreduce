# Spark SQL 作业配置 {#concept_vck_cjp_y2b .concept}

Spark SQL 作业配置

**说明：** Spark SQL 提交作业的模式默认是 yarn-client 模式。

## 操作步骤 {#section_ws2_gjp_y2b .section}

1.  进入[阿里云 E-MapReduce 控制台作业列表](https://emr.console.aliyun.com/?spm=5176.doc28084.2.1.gxpx8G#/job/region/cn-hangzhou)。
2.  单击该页右上角的**创建作业**，进入创建作业页面。
3.  填写作业名称。
4.  选择 Spark SQL 作业类型，表示创建的作业是一个 Spark SQL 作业。Spark SQL 作业在 E-MapReduce 后台使用以下的方式提交：

    ```
    spark-sql [options] [cli option]
    ```

5.  在**应用参数**选项框中填入 Spark SQL 命令后续的参数。
    -   -e 选项

        -e 选项可以直接写运行的 SQL，在作业**应用参数**框中直接输入，如下所示：

        ```
        -e "show databases;"
        ```

    -   -f 选项

        -f 选项可以指定 Spark SQL 的脚本文件。通过将编写好的 Spark SQL 脚本文件放在 OSS 上，可以更灵活，建议您使用这种运行方式。如下所示：

        ```
        -f ossref://your-bucket/your-spark-sql-script.sql
        ```

6.  选择执行失败后策略。
7.  单击**确定**，Spark SQL 作业即定义完成。

