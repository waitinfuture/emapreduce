# 跨集群访问JindoFS

通常E-MapReduce集群之间相互独立，每个集群的客户端只能连接并访问本集群内配置的namespace。在多集群的情况下，您可以通过配置JindoFS实现跨集群互访。本文以集群A访问集群B为例，介绍如何跨集群访问JindoFS。

-   已创建EMR-3.30.0及后续版本的同一VPC下的集群A和B，详情请参见[创建集群](/cn.zh-CN/集群管理/集群配置/创建集群.md)。
-   配置/etc/hosts文件，同步B集群所有节点的hosts至A集群。

## 修改配置

1.  进入SmartData服务。

    1.  登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com/)。

    2.  在顶部菜单栏处，根据实际情况选择地域（Region）和资源组。

    3.  单击上方的**集群管理**页签。

    4.  在**集群管理**页面，单击相应集群所在行的**详情**。

    5.  在左侧导航栏单击**集群服务** \> **SmartData**。

2.  进入**client**服务配置。

    1.  单击**配置**页签。

    2.  在**服务配置**区域，单击**client**页签。

3.  修改配置信息，实现跨集群访问。

    根据B集群的**namespace.backend.type**参数配置A集群：

    -   当B集群的**namespace.backend.type**为**rocksdb**时，执行如下操作：
        1.  单击右上角的**自定义配置**。
        2.  在**新增配置项**对话框中，添加**client.external.namespace.rpc.addresses**为`emr-header-1.<cluster-B>:8101`，单击**确定**。

            **说明：** <cluster-B\>为集群B的集群ID。

    -   当B集群的**namespace.backend.type**为**raft**时，执行如下操作：
        1.  单击右上角的**自定义配置**。
        2.  在**新增配置项**对话框中，添加**client.external.namespace.rpc.addresses**为`emr-header-1.<cluster-B>:8101,emr-header-2.<cluster-B>:8101,emr-header-3.<cluster-B>:8101`，单击**确定**。
4.  保存配置。

    1.  单击右上角的**保存**。

    2.  在**确认修改**对话框中，输入执行原因，开启**自动更新配置**。

    3.  单击**确定**。


## 关联多个集群

**client.external.namespace.rpc.addresses**配置多个远端地址时，即可实现关联多个集群，不同的集群地址通过英文分号（;）隔开。

例如，集群A需要关联集群B和集群C，B集群（rocksdb实现）地址为`emr-header-1.<cluster-B>:8101`，C集群（raft实现）地址为`emr-header-1.<cluster-C>:8101,emr-header-2.<cluster-C>:8101,emr-header-3.<cluster-C>:8101`那A集群需要添加的配置信息为`client.external.namespace.rpc.addresses=emr-header-1.<cluster-B>:8101;emr-header-1.<cluster-C>:8101,emr-header-2.<cluster-C>:8101,emr-header-3.<cluster-C>:8101`。

