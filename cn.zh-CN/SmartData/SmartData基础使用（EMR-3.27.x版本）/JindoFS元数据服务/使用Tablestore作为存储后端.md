# 使用Tablestore作为存储后端

JindoFS元数据服务支持不同的存储后端，本文介绍使用Tablestore（OTS）作为元数据后端时需要进行的配置。

-   已创建EMR集群。

    详情请参见[创建集群](/cn.zh-CN/集群管理/集群配置/创建集群.md)。

-   已创建Tablestore实例，推荐使用高性能实例。

    详情请参见[创建实例](/cn.zh-CN/快速入门/创建实例.md)。

    **说明：** 需要开启事务功能。


JindoFS在新版本中，支持使用Tablestore作为JindoFS元数据服务（Namespace Service）的存储。一个EMR JindoFS集群可以绑定一个Tablestore实例（Instance）作为JindoFS元数据服务的存储介质，元数据服务会自动为每个Namespace创建独立的Tablestore表进行管理和存储元数据信息。

元数据服务（双机Tablestore和HA）架构图如下所示。

![HA](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/2257459951/p101864.png)

## 配置Tablestore

使用Tablestore功能，需要把创建的Tablestore实例和JindoFS的Namespace服务进行绑定，详细步骤如下：

1.  进入SmartData服务。

    1.  登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com/)。

    2.  在顶部菜单栏处，根据实际情况选择地域（Region）和资源组。

    3.  单击上方的**集群管理**页签。

    4.  在**集群管理**页面，单击相应集群所在行的**详情**。

    5.  在左侧导航栏单击**集群服务** \> **SmartData**。

2.  进入bigboot服务配置。

    1.  单击**配置**页签。

    2.  单击**bigboot**。

        ![bigboot](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/5157459951/p101653.png)

3.  配置以下参数。

    例如，在华东1（杭州）地域下，创建了emr-jfs的Tablestore实例，EMR集群使用VPC网络，访问Tablestore的AccessKey ID为kkkkkk，Access Secret为XXXXXX。

    |参数|参数说明|是否必选|示例|
    |--|----|----|--|
    |**namespace.backend.type**|设置namespace后端存储类型，支持：     -   rocksdb
    -   ots
    -   raft
默认为rocksdb。

|是|ots|
    |**namespace.ots.instance**|Tablestore实例名称。|是|emr-jfs|
    |**namespace.ots.accessKey**|Tablestore实例的AccessKey ID。|否|kkkkkk|
    |**namespace.ots.accessSecret**|Tablestore实例的AccessKey Secret。|否|XXXXXX|
    |**namespace.ots.endpoint**|Tablestore实例的Endpoint地址，普通EMR集群，推荐使用VPC地址。|是|http://emr-jfs.cn-hangzhou.vpc.tablestore.aliyuncs.com|

4.  保存配置。

    1.  单击右上角的**保存**。

    2.  在**确认修改**对话框中，输入执行原因，开启**自动更新配置**。

    3.  单击**确定**。

5.  单击右上角的**操作** \> **重启 Jindo Namespace Service**。


## 配置Tablestore（高可用方案）

针对EMR的高可用集群，可以通过配置开启Namespace高可用模式。

![高可用](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/2257459951/p102406.png)

Namespace高可用模式采用Active和Standby互备方式，支持自动故障转移，当Active Namespace出现异常或者异常中止时，客户端可以请求自动切换到新的Active节点。

![OTS](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/6750369951/p102397.png)

1.  进入SmartData的bigboot服务配置，配置以下参数。

    1.  修改**jfs.namespace.server.rpc-address**值为**emr-header-1:8101,emr-header-2:8101**。

    2.  单击右上角的**自定义配置**，添加**namespace.backend.ots.ha**为**true**。

    3.  单击**确定**。

    4.  保存配置。

        1.  单击右上角的**保存**。
        2.  在**确认修改**对话框中，输入执行原因，开启**自动更新配置**。
        3.  单击**确定**。
2.  单击右上角的**操作** \> **重启Jindo Namespace Service**。

3.  单击右上角的**操作** \> **重启Jindo Storage Service**。


