# Kafka配置

本文介绍如何将Kafka集成到Ranger，以及如何配置权限。

已创建集群，并选择了Ranger服务，详情请参见[创建集群](/cn.zh-CN/集群管理/集群配置/创建集群.md)。

## Kafka集成Ranger

1.  Ranger启用Kafka。

    1.  登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com/)。

    2.  在顶部菜单栏处，根据实际情况选择地域（Region）和资源组。

    3.  单击上方的**集群管理**页签。

    4.  在**集群管理**页面，单击相应集群所在行的**详情**。

    5.  单击右侧的**操作**，选择**启用Kafka**。

        ![Enable Kafka PLUGIN](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/2009197951/p11548.png)

    6.  在**执行集群操作**对话框设置相关参数，然后单击**确定**。在**确认**对话框，单击**确定**。

        单击右上角**查看操作历史**查看任务进度，等待任务完成。

2.  Ranger UI页面添加Kafka Service。

    1.  进入Ranger UI页面，详情请参见[概述](/cn.zh-CN/集群类型/Hadoop集群/Ranger/概述.md)。

    2.  在Ranger UI页面，添加Kafka Service。

        ![Ranger WebUI](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0698197951/p10841.png)

    3.  配置Kafka Service。

        ![add_service](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3014027951/p81272.png)

        |参数|说明|
        |--|--|
        |**Service Name**|固定值**emr-kafka**。|
        |**Username**|固定值**kafka**。|
        |**Password**|可自定义。|
        |**Zookeeper Connect String**|填写格式**emr-header-1:2181/kafka-x.xx**。 **说明：** 其中`kafka-x.xx`根据Kafka实际版本填写。 |

    4.  单击**Add**。

3.  重启Kafka Broker。

    1.  左侧导航栏单击**集群服务** \> **Kafka**。

    2.  单击右上角**操作**下拉菜单，选择 **重启 Kafka Broker**。

    3.  在**执行集群操作**对话框设置相关参数，然后单击**确定**。在**确认**对话框，单击**确定**。

        单击右上角**查看操作历史**查看任务进度，等待任务完成。


## 权限配置示例

上面一节中已经将Ranger集成到Kafka，现在可以设置权限。

**说明：** 标准集群中，在添加了Kafka Service后，Ranger会默认生成规则all - topic，不作任何权限限制（即允许所有用户进行所有操作），此时Ranger无法通过用户进行权限识别。

以test用户为例，添加Publish权限。

1.  进入Ranger UI页面，详情请参见[概述](/cn.zh-CN/集群类型/Hadoop集群/Ranger/概述.md)。

2.  在Ranger UI页面，单击配置好的**emr-kafka**。

    ![click_emr_kafka](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7187593061/p81278.png)

3.  单击右上角的**Add New Policy**。

4.  填写参数。

    ![add_Policy](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3014027951/p81282.png)

    |参数|说明|
    |--|--|
    |**Policy Name**|策略名称，可以自定义。|
    |**topic**|自定义。可填写多个，填写一个需按一次Enter键。|
    |**Select Group**|指定添加此策略的用户组。|
    |**Select User**|指定添加此策略的用户。|
    |**Permissions**|单击![add_permissions](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3014027951/p81288.png)，选择**Publish**。|

    单击**Select Group**下方的![add_permissions](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3014027951/p81288.png)，可以对多个Group进行授权。

5.  单击**Add**。

    添加Policy后，实现对**test**的授权。test用户即可以对名为**test**的topic执行写入操作。

    **说明：** 添加、删除或修改Policy后，需要等待约一分钟至授权生效。


