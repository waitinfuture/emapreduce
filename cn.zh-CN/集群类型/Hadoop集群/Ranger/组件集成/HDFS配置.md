# HDFS配置

本文介绍如何将HDFS集成到Ranger，以及如何配置权限。

已创建集群，并选择了Ranger服务，详情请参见[创建集群](/cn.zh-CN/集群管理/集群配置/创建集群.md)。

## HDFS集成Ranger

1.  Ranger启用HDFS。

    1.  登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com/)。

    2.  在顶部菜单栏处，根据实际情况选择地域（Region）和资源组。

    3.  单击上方的**集群管理**页签。

    4.  在**集群管理**页面，单击相应集群所在行的**详情**。

    5.  在左侧导航栏单击**集群服务** \> **RANGER**。

    6.  单击右侧的**操作**下拉菜单，选择**启用HDFS**。

        ![Enable HDFS PLUGIN](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/6998197951/p11456.png)

    7.  在**执行集群操作**对话框设置相关参数，然后单击**确定**。在**确认**对话框，单击**确定**。

        单击右上角**查看操作历史**查看任务进度，等待任务完成。

2.  Ranger UI添加HDFS Service。

    1.  进入Ranger UI页面，详情请参见[概述](/cn.zh-CN/集群类型/Hadoop集群/Ranger/概述.md)。

    2.  在Ranger UI页面添加HDFS Service。

        ![Ranger UI](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/5157459951/p11479.png)

    3.  配置相关参数。

        ![hdfs](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7781027951/p112304.png)

        |参数|说明|
        |--|--|
        |**Service Name**|固定值emr-hdfs。|
        |**Username**|固定值hadoop。|
        |**Password**|自定义。|
        |**Namenode URL**|        -   非高可用集群：hdfs://emr-header-1:9000。
        -   高可用集群：hdfs://emr-header-1:8020。 |
        |**Authorization Enabled**|标准集群选择**No**；高安全集群选择**Yes**。|
        |**Authentication Type**|        -   Simple：表示标准集群。
        -   Kerberos：表示高安全集群。 |
        |**dfs.datanode.kerberos.principal**|标准集群时不填写；高安全集群时填写hdfs/\_HOST@EMR.$\{id\}.com。 **说明：** 您可以登录服务器执行`hostname`命令，`hostname`中的数字即为`${id}`。 |
        |**dfs.namenode.kerberos.principal**|
        |**dfs.secondary.namenode.kerberos.principal**|
        |**Add New Configurations**|        -   **Name**：固定值**policy.download.auth.users**。
        -   **Value**：固定值**hdfs**。 |

    4.  单击**Add**。

3.  重启HDFS。

    1.  左侧导航栏单击**集群服务** \> **HDFS**。

    2.  单击右上角**操作**下拉菜单，选择**重启 All Components**。

    3.  在**执行集群操作**对话框设置相关参数，然后单击**确定**。在**确认**对话框，单击**确定**。

        单击右上角**查看操作历史**查看任务进度，等待任务完成。


## 权限配置示例

例如，授予test用户/user/foo路径的Write和Execute权限。

1.  进入Ranger UI页面，详情请参见[概述](/cn.zh-CN/集群类型/Hadoop集群/Ranger/概述.md)。

2.  在Ranger UI页面，单击配置好的**emr-hdfs**。

    ![权限配置示例](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7998197951/p11482.png)

3.  单击右上角的**Add New Policy**。

4.  配置相关参数。

    |参数|说明|
    |--|--|
    |**Policy Name**|策略名称，可以自定义。|
    |**Resoure Path**|资源路径。|
    |**recursive**|子目录或文件是否集成权限。|
    |**Select Group**|指定添加此策略的用户组。|
    |**Select User**|指定添加此策略的用户。|
    |**Permissions**|选择授予的权限。|

5.  单击**Add**。

    添加Policy后，实现了对test用户的授权。test用户即可访问/user/foo的HDFS路径。

    **说明：** 添加、删除或修改Policy后，需要等待约一分钟至授权生效。


