# 访问JindoFS Web UI

JindoFS提供了Web UI服务，您可以快速查看集群当前的状态。例如，当前的运行模式、命名空间、集群StorageService信息和启动状态等。

通过SSH隧道方式才能访问Web UI，详情请参见[通过SSH隧道方式访问开源组件Web UI](/cn.zh-CN/集群管理/集群配置/连接集群/通过SSH隧道方式访问开源组件Web UI.md)。

## 访问JindoFS Web UI

打通SSH隧道后，您可以通过http://emr-header-1:8101/访问JindoFS Web UI功能。JindoFS 3.0版本提供总览信息（Overview）、Namespace信息、存储节点信息以及专家功能（Advanced）。

-   总览信息（Overview）

    包含Namespace启动时间、当前状态、元数据后端、当前Storage服务数量和版本信息等。

    ![Overview](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/5997443061/p175150.png)

-   Namespace信息

    包含当前节点可用的Namespace以及对应的模式和后端。Block模式的Namespace支持查看当前Namespace的统计信息，包括目录数、文件数以及文件总大小等。

    ![Namespace Info](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/5997443061/p175151.png)

-   StorageService信息

    包含当前集群的 StorageService 列表，以及对应 StorageService的地址、状态、使用量、最近连接时间、启动时间、StorageService编号和内部版本信息等。

    ![StorageService](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/5997443061/p175153.png)

    单击Node对应链接，可以查看每个磁盘的空间使用情况。

    ![StorageList](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/5997443061/p175154.png)

-   专家功能（Advanced）

    专家功能目前仅用于JindoFS开发人员排查问题。


