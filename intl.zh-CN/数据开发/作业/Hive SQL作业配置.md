# Hive SQL作业配置

本文介绍如何配置Hive SQL类型的作业。

已创建好项目，详情请参见[项目管理](/intl.zh-CN/数据开发/项目管理.md)。

## 操作步骤

1.  通过主账号登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com)。

2.  在顶部菜单栏处，根据实际情况选择地域（Region）和资源组。

3.  单击上方的**数据开发**页签。

4.  在**项目列表**页面，单击待编辑项目所在行的**作业编辑**。

5.  在**作业编辑**区域，在需要操作的文件夹上，右键选择**新建作业**。

6.  输入**作业名称**、**作业描述**，在**作业类型**下拉列表中选择**Hive SQL**作业类型。

    表示创建的作业是一个Hive SQL作业。这种类型的作业，其运行实际是通过以下方式提交的Hive SQL作业。

    ```
    hive -e {SQL CONTENT}
    ```

    其中`SQL_CONTENT`为作业编辑器中填写的SQL语句。

    ![HiveSQL](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9259929951/p46634.png)

7.  单击**确定**。

8.  在**作业内容**输入框中填入Hive SQL语句。

    ```
    -- SQL语句示例。
    -- SQL语句最大不能超过64KB。
    show databases;
    show tables;
    -- 系统会自动为SELECT语句加上'limit 2000'的限制。
    select * from test1;
    ```

    ![HiveSQL_content](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9259929951/p46635.png)

9.  单击**保存**，作业配置即定义完成。


