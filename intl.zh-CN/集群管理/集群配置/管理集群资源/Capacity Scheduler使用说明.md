---
keyword: [Capacity Scheduler, Capacity]
---

# Capacity Scheduler使用说明

Capacity Scheduler称为容量调度器，是Apache YARN内置的调度器，E-MapReduce YARN使用Capacity Scheduler作为默认调度器。Capacity Scheduler是一种多租户、分层级的资源调度器，调度器中的子队列是通过设置Capacity来划分各个子队列的使用情况。

## 开启Capacity Scheduler

**说明：** 开启集群资源队列后，YARN组件**配置**中的**capacity-scheduler**配置区域将处于冻结状态，相关已有配置将会同步到**集群资源管理**页面中。如果需要继续在YARN服务的**配置**页面通过XML的方式设置集群资源，则需先在**集群资源管理**中关闭YARN资源队列。

1.  登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com/)。

2.  在顶部菜单栏处，根据实际情况选择地域（Region）和资源组。

3.  单击上方的**集群管理**页签。

4.  在**集群管理**页面，单击相应集群所在行的**详情**。

5.  在左侧导航栏中，单击**集群资源管理**。

6.  在**集群资源管理**页面，打开**开启YARN资源队列**开关。

7.  单击**保存**。

    默认开启Capacity Scheduler。


## 配置Capacity Scheduler

开启YARN资源队列后，可以执行以下步骤配置Capacity Scheduler。

1.  在**集群资源管理**页面，单击上方的**资源队列配置**。

2.  在**队列配置**区域，配置队列信息。

    -   单击**编辑**，可以更改资源队列。
    -   单击**更多** \> **创建子队列**，可以创建子队列。
    **root**为一级队列，是所有队列的父队列，管理YARN的所有资源，默认root下只有default队列。

    **说明：**

    -   同一个父队列下的同一级别的子队列Capacity之和必须为100。例如，root下有两个子队列default和department，两个子队列的Capacity求和必须为100，department下又设置了market和dev，这里department下两个子队列之和必须也是100。
    -   在程序运行时，如果不指定提交队列，默认会提交到default队列。
    -   新建root下一级队列无需重启ResourceManager，直接单击**部署生效**即可。
    -   新建和设置root下的二级队列需要重启ResourceManager。
    -   修改队列名称需要重启ResourceManager。

## 切换调度器类型

开启YARN资源队列后，可以执行以下步骤切换调度器类型。

1.  在**集群资源管理**页面，单击上方的**切换调度器**。

2.  单击需要切换的资源队列类型。

3.  单击**保存**。

4.  单击**操作** \> **重启ResourceManager**。

5.  在**执行集群操作**对话框中，设置相关参数，单击**确定**。

6.  在**确认**对话框中，单击**确定**。

    待提示操作成功时，表示切换调度器类型成功。


## 提交作业

-   提交作业时，如果不指定提交队列，会默认提交到default队列。
-   指定队列时应选择子节点，不能将任务提交到父队列。
-   提交到指定队列时需通过**mapreduce.job.queuename**来指定队列，示例如下。

    ```
    `hadoop jar /usr/lib/hadoop-current/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.8.5.jar pi -Dmapreduce.job.queuename=test  2 2`
    ```


