# Hive SQL 作业配置 {#task_261440 .task}

本文介绍 Hive SQL 作业配置的操作步骤。

1.  通过主账号登录[阿里云 E-MapReduce 控制台](https://emr.console.aliyun.com/)。
2.  单击上方的数据开发页签，进入项目列表页面。
3.  单击对应项目右侧的**工作流设计**，在左侧导航栏中单击**作业编辑**进入作业编辑页面。
4.  在页面左侧，在需要操作的文件夹上单击右键，选择**新建作业**
5.  填写作业名称，作业描述；选择 Hive SQL 作业类型，表示创建的作业是一个 Hive SQL 作业。Hive SQL 作业在 E-MapReduce 后台使用以下的方式提交： 

    ``` {#codeblock_uyi_ftd_qo1}
    hive -e {SQL CONTENT}
    ```

    其中 SQL\_CONTENT 为作业编辑器中填写的 SQL 语句。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/215990/155728273846634_zh-CN.png)

6.  单击**确定**。 

    **说明：** 您还可以通过在文件夹上单击右键，进行创建子文件夹、重命名文件夹和删除文件夹操作。

7.  在**作业内容**输入框中填入 Hive SQL 语句，例如： 

    ``` {#codeblock_4o1_7s1_rc7}
    -- SQL语句示例
    -- SQL语句最大不能超过64KB
    show databases;
    show tables;
    -- 系统会自动为SELECT语句加上'limit 2000'的限制
    select * from test1;
    ```

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/215990/155728273846635_zh-CN.png)

8.  单击**保存**，作业配置即定义完成。

