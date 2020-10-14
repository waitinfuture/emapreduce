# Presto配置

本文介绍如何将Presto集成到Ranger，以及如何配置权限。

已创建E-MapReduce的集群，并且选择了Ranger和Presto服务。详情请参见[创建集群](/cn.zh-CN/集群管理/集群配置/创建集群.md)。

## Presto集成Ranger

1.  Ranger启用Presto。

    1.  登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com/)。

    2.  在顶部菜单栏处，根据实际情况选择地域（Region）和资源组。

    3.  单击上方的**集群管理**页签。

    4.  在**集群管理**页面，单击相应集群所在行的**详情**。

    5.  在左侧导航栏单击**集群服务** \> **RANGER**。

    6.  单击右侧的**操作**下拉菜单，选择**启用Presto**。

    7.  在**执行集群操作**对话框设置相关参数，然后单击**确定**。在**确认**对话框，单击**确定**。

        单击右上角**查看操作历史**查看任务进度，等待任务完成。

2.  Ranger UI添加Presto Service。

    1.  进入Ranger UI页面，详情请参见[概述](/cn.zh-CN/集群类型/Hadoop集群/Ranger/概述.md)。

    2.  在Ranger UI页面添加Presto Service。

        ![presto](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/4934027951/p81570.png)

    3.  配置参数。

        ![Presto](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/4934027951/p112352.png)

        |参数|说明|
        |--|--|
        |**Service Name**|固定填写emr-presto。|
        |**Username**|固定填写hadoop。|
        |**Password**|不填写。|
        |**jdbc.driverClassName**|固定填写io.prestosql.jdbc.PrestoDriver。|
        |**jdbc.url**|固定填写jdbc:presto://emr-header-1:9090。|
        |**Add New Configurations**|        -   **Name**：固定值policy.download.auth.users。
        -   **Value**：固定值hadoop。 |

    4.  单击**Add**。

3.  重启Presto Master。

    1.  在左侧导航栏，单击**集群服务** \> **Presto**。

    2.  单击右上角**操作**下拉菜单，选择**重启PrestoMaster**。

    3.  在**执行集群操作**对话框设置相关参数，然后单击**确定**。在**确认**对话框，单击**确定**。

        单击右上角**查看操作历史**查看任务进度，等待任务完成。


## 权限配置示例

Ranger Presto权限控制与Ranger Hive和Ranger Hbase等权限控制不同，Ranger Presto采用的是权限分层次控制的策略。

**说明：**

-   配置的权限应该与所属的层次保持一致。如果配置的权限与所属的层次不相符，则该权限配置将不起作用。
-   Presto会对用户进行两次权限检查，首先检查该用户是否有访问Catalog的权限，其次检查本次访问所涉及到的权限。

示例一：给用户liu授予访问hive表testdb.test的a列的Select权限。

1.  进入Ranger UI页面，详情请参见[概述](/cn.zh-CN/集群类型/Hadoop集群/Ranger/概述.md)。
2.  在Ranger UI页面，单击配置好的**emr-presto**。

    ![shili1](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/4934027951/p81621.png)

3.  单击右上角的**Add New Policy**。

    添加Policy对**catalog**的访问权限进行控制。因为该权限为**catalog**层次，所以只需要配置到**catalog**层次即可。授权用户liu访问**catalog**的Use权限。

    ![catelog](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/8834027951/p139688.png)

4.  单击**Add**。
5.  单击右上角的**Add New Policy**。

    配置liu用户对表testdb.test的a列的Select权限。由于表的Select权限属于**column**层次，因此需要单击![添加层级Policy](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/5934027951/p88629.png)图标，依次添加**schema**、**table**和**column**层次的内容。

    ![配置Column_liu](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/5934027951/p88630.png)

    |参数|说明|
    |--|--|
    |**Policy Name**|策略名称，可自定义。例如testdb。|
    |**catalog**|添加**catalog**的名称，可自定义。例如hive。|
    |**schema**|添加**schema**的名称，例如testdb。星号（\*）表示所有schema。|
    |**table**|添加**table**的名称，例如test。星号（\*）表示所有table。|
    |**column**|添加**column**的名称，例如a。星号（\*）表示所有column。|
    |**Select User**|指定添加此策略的用户，例如liu。|
    |**Permissions**|选择授予的权限，例如Select。|

6.  单击**Add**。

    添加Policy后，实现对liu用户的授权。liu用户即可以对testdb.test表的a列进行访问。

    **说明：** 添加、删除或修改Policy后，需要等待约一分钟至授权生效。


示例二：给用户chen添加创建hive表testdb.test的Create权限。

1.  进入Ranger UI页面，详情请参见[概述](/cn.zh-CN/集群类型/Hadoop集群/Ranger/概述.md)。
2.  给用户chen添加Catalog访问权限（此例中Catalog=hive）。
    -   如果已经添加了Catalog层次的Policy，则直接编辑该Policy，在Select User中增加用户chen。
        1.  单击配置好的**emr-presto**。
        2.  单击**catalog\_hive**所在行的![edit](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/8834027951/p139695.png)图标。

            ![chen](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/8834027951/p139694.png)

        3.  授权用户chen访问**catalog**的Use权限。

            ![add_condition](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/5934027951/p85657.png)

        4.  单击**Add**。
    -   如果还未添加Catalog层次的Policy，则按照[示例一](#p_1uo_eyy_3d9)中的方法，授权用户chen访问**catalog**的Use权限。
3.  单击右上角的**Add New Policy**。

    由于表的Create权限属于**schema**层次，因此您需要配置权限到**schema**层次。单击![添加层级Policy](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/5934027951/p88629.png)按钮，添加**schema**层次的内容。

    ![add_table](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/5934027951/p85660.png)


示例三：在Ranger UI页面，配置`show schemas`和`show tables`权限

-   EMR-3.28.1之前版本

    您需要配置`schema=information_schema,table=schemate,column=*`的Select来获取`show schemas`执行的结果，配置`schema=information_schema,table=tables,column=*`的Select来获取`show tables`执行的结果，两者可以配置在一个Policy当中，示例如下。

    ![eg3](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/5722500061/p165714.png)

-   EMR-3.28.1及后续版本和EMR-4.4.1版本

    如果您需要执行`show schemas`命令，则需要配置对Catalog的Show权限。同理，如果您需要执行`show tables`命令，则需要配置对Schema的Show权限。

    因为Presto在鉴权完成后，还会对获取到的Schema和Table的列表进行一次筛选，只筛选出具有Select权限的Schema和Table进行显示，所以您还需要配置Schema和Table的Select权限。当您在执行Show命令时，只会显示出具有Select权限的Schema和Table。

    ![eg3-2](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/5722500061/p165731.png)


## Ranger Presto与Ranger Hive共享权限配置

在部分场景下，Presto和Hive中配置的权限是一致的。由于EMR的Ranger提供了可以让Ranger Presto和Ranger Hive共享权限的方案，因此只需要在Ranger的Hive service中配置相关权限，Ranger Presto即可直接使用该权限配置用于检查用户权限。

**说明：** Ranger Presto与Ranger Hive共享权限配置，仅适用于Catalog为hive时的场景。

配置前需要关注以下事项：

-   已正确配置Ranger Hive，详情请参见[Hive配置](/cn.zh-CN/集群类型/Hadoop集群/Ranger/组件集成/Hive配置.md)。
-   已在Ranger UI中添加Ranger Presto service。
-   与Ranger Presto一样，如果需要使用`show schemas`或`show tables`命令，则在Ranger Hive service中配置用户对`database=information_schema,table=*,column=*`的Select权限。

使用及配置方法如下：

1.  登录Master节点，修改配置文件/etc/ecm/presto-conf/ranger-presto-security.xml中的`ranger.plugin.hive.authorization.enable`修改为`true`。

2.  在左侧导航栏中，单击**集群服务** \> **Presto**。

3.  单击右上角的**操作** \> **重启 PrestoMaster**。

    在**执行集群操作**对话框设置相关参数，然后单击**确定**。在**确认**对话框，单击**确定**。


