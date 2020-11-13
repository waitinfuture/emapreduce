# Impala SQL作业配置

在数据开发过程中如果您需要使用Impala SQL，可以在E-MapReduce中配置Impala SQL作业。本文介绍如何配置Impala SQL作业。

已创建好项目，详情请参见[项目管理](/intl.zh-CN/数据开发/项目管理.md)。

## 操作步骤

1.  通过主账号登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com)。

2.  在顶部菜单栏处，根据实际情况选择地域（Region）和资源组。

3.  单击上方的**数据开发**页签。

4.  在**项目列表**页面，单击待编辑项目所在行的**作业编辑**。

5.  在**作业编辑**区域，在需要操作的文件夹上，右键选择**新建作业**。

6.  输入**作业名称**、**作业描述**，在**作业类型**下拉列表中选择**Impala SQL**作业类型。

    此类型作业，实际是通过以下方式提交的Impala SQL作业。

    ```
    impala-shell -f {SQL_CONTENT} [options];
    ```

    参数描述如下：

    -   `options`： 在**作业设置**页面的**高级设置**页签，单击**环境变量**所在行的![add](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7056559951/p74998.png)图标，添加环境变量IMPALA\_CLI\_PARAMS，例如`IMAPAL_CLI_PARAMS="-u hive"`。
    -   `SQL_CONTENT`：填写的SQL语句。
7.  单击**确定**。

8.  在**作业内容**中，输入Impala SQL语句。

    示例如下。

    ```
    show databases;
    show tables;
    select * from test1;
    ```

9.  单击**保存**，作业配置即定义完成。


