# HBase配置

本文介绍如何将HBase集成到Ranger，以及如何配置权限。

已创建集群，并选择了HBase和Ranger服务，详情请参见[创建集群](/cn.zh-CN/集群管理/集群配置/创建集群.md)。

## HBase集成Ranger

1.  Ranger启用HBase。

    1.  登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com/)。

    2.  在顶部菜单栏处，根据实际情况选择地域（Region）和资源组。

    3.  单击上方的**集群管理**页签。

    4.  在**集群管理**页面，单击相应集群所在行的**详情**。

    5.  在左侧导航栏单击**集群服务** \> **RANGER**。

    6.  单击右侧的**操作**下拉菜单，选择**启用HBase**。

        ![Enable HBase PLUGIN](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0009197951/p11513.png)

    7.  在**执行集群操作**对话框设置相关参数，然后单击**确定**。在**确认**对话框，单击**确定**。

        单击右上角**查看操作历史**查看任务进度，等待任务完成。

2.  Ranger UI添加HBase Service。

    1.  进入Ranger UI页面，详情请参见[概述](/cn.zh-CN/集群类型/Hadoop集群/Ranger/概述.md)。

    2.  在Ranger UI页面，添加HBase Service。

        ![add_hbase](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/5183027951/p81500.png)

    3.  配置相关参数。

        ![add_hbase](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/6183027951/p81312.png)

        |参数|说明|
        |--|--|
        |**Service Name**|固定值**emr-hbase**。|
        |**Username**|固定填写**hbase**。|
        |**Password**|自定义。|
        |**hadoop.security.authentication**|        -   标准集群（非高安全集群）：选择**Simple**。
        -   高安全集群：选择**Kerberos**。 |
        |**hbase.master.kerberos.principal**|标准集群时不填写；高安全集群时填写**hbase/\_HOST@EMR.$\{id\}.COM**。 **说明：** `${id}`可登录机器执行`hostname`命令，`hostname`中的数字即为$\{id\}的值。 |
        |**hbase.security.authentication**|        -   标准集群（非高安全集群）：选择**Simple**。
        -   高安全集群：选择**Kerberos**。 |
        |**hbase.zookeeper.property.clientPort**|固定值**2181**。|
        |**hbase.zookeeper.quorum**|固定值**emr-header-1,emr-worker-1**。|
        |**zookeeper.znode.parent**|固定值**/hbase**。|
        |**Add New Configurations**|        -   **Name**：固定值**policy.download.auth.users**。
        -   **Value**：固定值**hbase**。 |

    4.  单击**Add**。

3.  重启HBase。

    1.  左侧导航栏单击**集群服务** \> **HBase**。

    2.  单击右上角**操作**下拉菜单，选择**重启All Components**。

    3.  在**执行集群操作**对话框设置相关参数，然后单击**确定**。在**确认**对话框，单击**确定**。

        单击右上角**查看操作历史**查看任务进度，等待任务完成。


## 设置管理员账号

1.  在**Service Manager**页面，单击创建好的**emr-hbase**。

    ![view hbase](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9083027951/p130024.png)

2.  设置管理员账号的权限（admin权限）。

    用于执行管理命令，例如balance、compaction、flush或split等。

    因为当前服务已经存在权限策略，所以您只需要单击右侧的![edit](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/6183027951/p81341.png)图标，在User中添加需要设置的账号即可。另外也可以修改其中的权限（例如只保留admin权限）。HBase账号必须默认设置为管理员账号。

    ![设置管理员账号](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/1009197951/p11523.png)

    如果使用Phoenix，则需在Ranger的HBase中新增如下策略。

    |参数|说明|
    |--|--|
    |**HBase Column-family**|星号（**\***）|
    |**HBase Column**|星号（**\***）|
    |**Select Group**|**public**|
    |**Permissions**|**Read**、**Write**、**Create**和**Admin**|


## 权限配置示例

例如给test用户授予表foo\_ns:test的Create、Write和Read权限。

1.  进入Ranger UI页面，详情请参见[概述](/cn.zh-CN/集群类型/Hadoop集群/Ranger/概述.md)。

2.  在Ranger UI页面，单击配置好的**emr-hbase**。

    ![emr-habse](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/1009197951/p11524.png)

3.  单击右上角的**Add New Policy**。

4.  配置相关权限。

    |参数|说明|
    |--|--|
    |**Policy Name**|策略名称，可以自定义。|
    |**HBase Table**|表对象，格式为**$\{namespace\}:$\{tablename\}**。可输入多个，填写一个需按一次Enter键。 如果是default的namespace，不需要加default。支持通配符星号（\*），例如，**foo\_ns:\***表示foo\_ns下的所有表。

**说明：** 目前不支持**default:\***。 |
    |**HBase Column-family**|列簇。|
    |**HBase Column**|列名。|
    |**Select Group**|指定添加此策略的用户组。|
    |**Select User**|指定添加此策略的用户。|
    |**Permissions**|选择授予的权限。|

5.  单击**Add**。

    添加Policy后，实现对test用户的授权。test用户即可以对foo\_ns:test表进行访问。

    **说明：** 添加、删除或修改Policy后，需要等待约一分钟至授权生效。


