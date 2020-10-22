# Impala配置

本文介绍如何将Impala集成至Ranger，以及如何配置权限。

已创建EMR-4.4.1及后续版本的Hadoop集群，并且选择了Ranger和Impala服务。详情请参见[创建集群](/cn.zh-CN/集群管理/集群配置/创建集群.md)。

Impala集成Ranger后，支持通过Impala-shell、Hue和JDBC方式在访问Hive表时进行权限控制。

## Impala集成Ranger

1.  在[阿里云E-MapReduce控制台](https://emr.console.aliyun.com/)配置Hive集成Ranger，详情请参见[Hive配置](/cn.zh-CN/集群类型/Hadoop集群/Ranger/组件集成/Hive配置.md)。

    **说明：** 在Ranger中，因为Impala和Hive使用同一个Ranger Service（emr-hive）进行权限控制，所以需要先配置好Ranger Hive。

2.  Ranger启用Impala。

    1.  在[阿里云E-MapReduce控制台](https://emr.console.aliyun.com/)的**集群管理**页面，单击相应集群所在行的**详情**。

    2.  在左侧导航栏单击**集群服务** \> **RANGER**。

    3.  在Ranger集群服务页面，单击右上角的**操作** \> **启用Impala**。

        ![Impala_ranger](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3769623061/p175357.png)

    4.  在**执行集群操作**对话框设置相关参数，然后单击**确定**。在**确认**对话框，单击**确定**。

        单击右上角**查看操作历史**查看任务进度，等待任务完成。

3.  重启Impala。

    1.  在左侧导航栏单击**集群服务** \> **Impala**。

    2.  在Impala集群服务页面，单击右侧的**操作** \> **重启All Components**。

    3.  在**执行集群操作**对话框设置相关参数，然后单击**确定**。在**确认**对话框，单击**确定**。

        单击右上角**查看操作历史**查看任务进度，等待任务完成。


## 权限配置示例

**说明：** Ranger中Impala暂不支持Hive Row-level Filter功能。

例如：给用户foo授予表testdb.test的a列Select权限。

1.  进入Ranger UI页面，详情请参见[概述](/cn.zh-CN/集群类型/Hadoop集群/Ranger/概述.md)。

2.  在Ranger UI页面，单击配置好的**emr-hive**。

    ![权限配置示例](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3370108951/p11509.png)

3.  单击右上角的**Add New Policy**。

4.  配置权限。

    ![配置相关权限](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/2814027951/p11510.png)

    |参数|说明|
    |--|--|
    |**Policy Name**|策略名称，可以自定义。|
    |**database**|添加Hive中的数据库，例如testdb。|
    |**table**|添加表，例如test。|
    |**Hive Column**|添加列名。填写星号（\*）时表示所有列。|
    |**Select Group**|指定添加此策略的用户组。|
    |**Select User**|指定添加此策略的用户。|
    |**Permissions**|选择授予的权限。|

5.  单击**Add**。

    添加Policy后，实现对foo的授权。foo用户即可以访问testdb.test表。

    **说明：** 添加、删除或修改Policy后，需要等待约一分钟至授权生效。


## 关闭Impala集成Ranger

当您不需要使用Ranger控制Impala权限时，可以执行如下方法关闭Impala集成Ranger。

1.  Ranger禁用Impala。

    1.  在[阿里云E-MapReduce控制台](https://emr.console.aliyun.com/)的**集群管理**页面，单击相应集群所在行的**详情**。

    2.  在左侧导航栏单击**集群服务** \> **RANGER**。

    3.  在Ranger集群服务页面，单击右上角的**操作** \> **禁用Impala**。

    4.  在**执行集群操作**对话框设置相关参数，然后单击**确定**。在**确认**对话框，单击**确定**。

        单击右上角**查看操作历史**查看任务进度，等待任务完成。

2.  重启Impala。

    1.  在左侧导航栏单击**集群服务** \> **Impala**。

    2.  在Impala集群服务页面，单击右上角的**操作** \> **重启All Components**。

    3.  在**执行集群操作**对话框设置相关参数，然后单击**确定**。在**确认**对话框，单击**确定**。

        单击右上角**查看操作历史**查看任务进度，等待任务完成。


