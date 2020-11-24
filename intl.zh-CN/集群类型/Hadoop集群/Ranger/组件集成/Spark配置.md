# Spark配置

本文介绍如何将Spark集成到Ranger，以及相关的权限配置。

已创建集群，并选择了Ranger服务，详情请参见[创建集群](/intl.zh-CN/集群管理/集群配置/创建集群.md)。

Spark集成Ranger进行权限控制，仅适用于通过Spark ThriftServer执行Spark SQL作业。例如，使用Spark的Beeline客户端或JDBC接口，通过Spark ThriftServer提交Spark SQL作业。

## Spark SQL集成Ranger

1.  在[阿里云E-MapReduce控制台](https://emr.console.aliyun.com/)，配置Hive集成Ranger，详情请参见[Hive配置](/intl.zh-CN/集群类型/Hadoop集群/Ranger/组件集成/Hive配置.md)。

    在Ranger中，因为Spark SQL与Hive共享权限配置，所以只需要将Hive集成Ranger，便可以通过Ranger控制Spark SQL的权限。

2.  Ranger启用Spark。

    1.  在[阿里云E-MapReduce控制台](https://emr.console.aliyun.com/)的**集群管理**页面，单击相应集群所在行的**详情**。

    2.  在左侧导航栏单击**集群服务** \> **RANGER**。

    3.  单击右侧的**操作**下拉菜单，选择**启用Spark**。

        ![ranger_spark](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1814027951/p72107.png)

    4.  在**执行集群操作**对话框设置相关参数，然后单击**确定**。在**确认**对话框，单击**确定**。

        单击右上角**查看操作历史**查看任务进度，等待任务完成。

3.  重启Spark ThriftServer。

    1.  在左侧导航栏单击**集群服务** \> **Spark**。

    2.  在Spark**集群服务**页面，单击右侧的**操作** \> **重启ThriftServer**。

    3.  在**执行集群操作**对话框设置相关参数，然后单击**确定**。在**确认**对话框，单击**确定**。

        单击右上角**查看操作历史**查看任务进度，等待任务完成。


## 权限配置示例（Ranger UI配置相关权限）

例如：给用户foo授予表testdb.test的a列Select权限。

1.  进入Ranger UI页面，详情请参见[概述](/intl.zh-CN/集群类型/Hadoop集群/Ranger/Ranger 简介.md)。

2.  在Ranger UI页面，单击配置好的**emr-hive**。

    ![权限配置示例](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3370108951/p11509.png)

3.  单击右上角的**Add New Policy**。

4.  配置权限。

    ![配置相关权限](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2814027951/p11510.png)

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


