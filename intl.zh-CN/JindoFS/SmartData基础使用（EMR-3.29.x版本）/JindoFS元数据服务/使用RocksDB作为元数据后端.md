# 使用RocksDB作为元数据后端

JindoFS元数据服务支持不同的存储后端，默认配置RocksDB为元数据存储后端。本文介绍使用RocksDB作为元数据后端时需要进行的相关配置。

RocksDB作为元数据后端时不支持高可用。如果需要高可用，推荐配置Tablestore（OTS）或者Raft作为元数据后端，详情请参见[使用Tablestore作为存储后端](/intl.zh-CN/JindoFS/JindoFS基础使用（EMR-3.27.0及以上版本）/JindoFS元数据服务/使用Tablestore作为存储后端.md)和[使用Raft-RocksDB-Tablestore作为存储后端](/intl.zh-CN/JindoFS/JindoFS基础使用（EMR-3.27.0及以上版本）/JindoFS元数据服务/使用Raft-RocksDB-Tablestore作为存储后端.md)。

单机RocksDB作为元数据服务的架构图如下所示。

![RocksDB](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/4257459951/p102002.png)

1.  进入SmartData服务。

    1.  登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com/)。

    2.  在顶部菜单栏处，根据实际情况选择地域（Region）和资源组。

    3.  单击上方的**集群管理**页签。

    4.  在**集群管理**页面，单击相应集群所在行的**详情**。

    5.  在左侧导航栏单击**集群服务** \> **SmartData**。

2.  进入**namespace**服务配置。

    1.  单击**配置**页签。

    2.  单击**namespace**。

        ![namespace](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0357459951/p161094.png)

3.  设置**namespace.backend.type**为**rocksdb**。

4.  保存配置。

    1.  单击右上角的**保存**。

    2.  在**确认修改**对话框中，输入执行原因，开启**自动更新配置**。

    3.  单击**确定**。

5.  单击右上角的**操作** \> **重启 Jindo Namespace Service**。


