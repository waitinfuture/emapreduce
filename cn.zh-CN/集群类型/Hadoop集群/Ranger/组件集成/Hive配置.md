# Hive配置

本文介绍如何将Hive集成到Ranger，以及如何配置权限。

已创建集群，并选择了Ranger服务，详情请参见[创建集群](/cn.zh-CN/集群管理/集群配置/创建集群.md)。

## Hive访问模型

访问Hive数据，包括HiveServer2、Hive Client和HDFS三种方式：

-   HiveServer2方式
    -   场景： 您可以通过HiveServer2访问Hive数据。
    -   方式：使用Beeline客户端或者JDBC代码通过HiveServer2执行Hive脚本。
    -   权限设置：

        Hive官方自带的[Hive授权](/cn.zh-CN/集群类型/Hadoop集群/组件授权/Hive授权.md)针对HiveServer2使用场景进行权限控制。

        Ranger中对Hive的表或列级别的权限控制也是针对HiveServer2的使用场景。如果您还可以通过Hive Client或者HDFS访问Hive数据，仅对表或列层面做权限控制还不够，需要选择下面任一方式以进一步控制权限。

-   Hive Client方式
    -   场景： 您可以通过Hive Client访问Hive数据。
    -   方式： 使用Hive Client访问。
    -   权限设置

        Hive Client会请求HiveMetaStore进行DDL操作，例如`Alter Table Add Columns`，也可以通过提交MapReduce作业读取并处理HDFS中数据。

        Hive官方自带的[Hive授权](/cn.zh-CN/集群类型/Hadoop集群/组件授权/Hive授权.md)可以针对Hive Client使用场景进行权限控制，它会根据SQL中表的HDFS路径的读写权限，来决定您是否可以进行DDL或DML操作，例如`ALTER TABLE test ADD COLUMNS(b STRING)`。

        由于Ranger可以对Hive表的HDFS路径进行权限控制，HiveMetaStore可以配置Storage Based Authorization，因此二者结合可以实现对Hive Client访问场景的权限控制。

        **说明：** Hive Client场景的DDL操作权限通过底层HDFS控制权限 ，所以如果您有HDFS权限，则对应也会有表的DDL操作权限（例如Drop Table或Alter Table等）。

-   HDFS方式
    -   场景： 您可以通过HDFS访问数据。
    -   方式： HDFS客户端或代码等。
    -   权限设置：

        您可以直接访问HDFS，需要对Hive表的底层HDFS数据增加HDFS的权限控制。

        通过Ranger对Hive表底层的HDFS路径进行权限控制，详情请参见[权限配置示例](#section_ygn_y55_ctn)。


## Hive集成Ranger

1.  Ranger启用Hive。

    1.  登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com/)。

    2.  在顶部菜单栏处，根据实际情况选择地域（Region）和资源组。

    3.  单击上方的**集群管理**页签。

    4.  在**集群管理**页面，单击相应集群所在行的**详情**。

    5.  在左侧导航栏单击**集群服务** \> **RANGER**。

    6.  单击右侧的**操作**下拉菜单，选择**启用Hive**。

        ![start_hive](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7693027951/p81452.png)

    7.  在**执行集群操作**对话框设置相关参数，然后单击**确定**。在**确认**对话框，单击**确定**。

        单击右上角**查看操作历史**查看任务进度，等待任务完成。

        ![查看操作历史](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7693027951/p11502.png)

        **说明：** 重启Hive后，HiveServer2场景下，您需要使用Ranger进行权限配置。HiveClient场景下，您需要借助HDFS权限进行控制。开启HDFS的权限请参见[HDFS配置](/cn.zh-CN/集群类型/Hadoop集群/Ranger/组件集成/HDFS配置.md)。

2.  Ranger UI添加Hive Service。

    1.  进入Ranger UI页面，详情请参见[概述](/cn.zh-CN/集群类型/Hadoop集群/Ranger/概述.md)。

    2.  在Ranger UI页面，添加Hive Service。

        ![Ranger UI](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/8693027951/p11506.png)

    3.  配置参数。

        ![添加Hive Service](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/8693027951/p11507.png)

        |参数|说明|
        |--|--|
        |**Service Name**|固定值**emr-hive**。|
        |**Username**|固定值**hadoop**。|
        |**Password**|自定义。|
        |**jdbc.driverClassName**|默认值**org.apache.hive.jdbc.HiveDriver**。无需修改。|
        |**jdbc.url**|        -   标准集群： **jdbc:hive2://emr-header-1:10000/**
        -   高安全集群：**jdbc:hive2://$\{master1\_fullhost\}:10000/;principal=hive/$\{master1\_fullhost\}@EMR.$id.COM**
**说明：** $\{master1\_fullhost\}为master 1的长域名，可登录master 1执行`hostname`命令获取，$\{master1\_fullhost\} 中的数字即为$id的值。 |
        |**Add New Configurations**|        -   **Name**：固定值**policy.download.auth.users**。
        -   **Value**：hadoop（标准集群）、hive（高安全集群）。 |

    4.  单击**Add**。

3.  重启Hive。

    上述任务完成后，需要重启Hive才生效。

    1.  左侧导航栏单击**集群服务** \> **Hive**。

    2.  单击右上角**操作**下拉菜单，选择**重启 All Components**。

    3.  在**执行集群操作**对话框设置相关参数，然后单击**确定**。在**确认**对话框，单击**确定**。

        单击右上角**查看操作历史**查看任务进度，等待任务完成。


## 权限配置示例

例如给foo用户授予表testdb.test的a列Select权限。

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


