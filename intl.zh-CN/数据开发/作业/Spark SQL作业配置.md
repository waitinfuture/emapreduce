# Spark SQL作业配置

本文介绍如何配置Spark SQL类型的作业。

已创建好项目，详情请参见[项目管理](/intl.zh-CN/数据开发/项目管理.md)。

**说明：** Spark SQL提交作业的模式默认是Yarn-client模式。

## 操作步骤

1.  通过主账号登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com)。

2.  在顶部菜单栏处，根据实际情况选择地域（Region）和资源组。

3.  单击上方的**数据开发**页签。

4.  在**项目列表**页面，单击待编辑项目所在行的**作业编辑**。

5.  在**作业编辑**区域，在需要操作的文件夹上，右键选择**新建作业**。

6.  输入**作业名称**、**作业描述**，在**作业类型**下拉列表中选择**Spark SQL**作业类型。

    此类型的作业，实际是通过以下方式提交的Spark SQL作业。

    ```
    spark-sql [options] [cli options] {SQL_CONTENT}                
    ```

    参数描述如下：

    -   `options`： 在**作业设置**页面的**高级设置**页签，单击**环境变量**所在行的![add](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7056559951/p74998.png)图标，添加环境变量SPARK\_CLI\_PARAMS，例如`SPARK_CLI_PARAMS="--executor-memory 1g --executor-cores`。
    -   `cli options`：例如，`-e <quoted-query-string>`表示运行引号内的SQL查询语句。`-f <filename>`表示运行文件中的SQL语句。
    -   `SQL_CONTENT`：填写的SQL语句。
7.  单击**确定**。

8.  在**作业内容**中，输入Spark SQL语句。

    示例如下。

    ```
    -- SQL语句示例。
    -- SQL语句最大不能超过64KB。
    show databases;
    show tables;
    -- 系统会自动为SELECT语句加上'limit 2000'的限制。
    select * from test1;
    ```

9.  单击**保存**，作业配置即定义完成。


