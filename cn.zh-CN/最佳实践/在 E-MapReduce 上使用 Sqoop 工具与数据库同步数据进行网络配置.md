# 在 E-MapReduce 上使用 Sqoop 工具与数据库同步数据进行网络配置 {#concept_405382 .concept}

如果您的 E-MapReduce（EMR） 集群需要和集群之外的数据库同步数据，需要确保网络是联通的。本文以 RDS，ECS 自建，云下私有数据库三种情况，分别介绍如何进行网络配置。

## 云数据库 RDS {#section_0a1_hp6_yzw .section}

-   经典网络 RDS

    想要访问经典网络RDS，EMR 最好也指定用经典网络。经典网络的 RDS 可以设置内网地址和外网地址。由于经典网络 EMR 集群只有 Master 节点可以访问公网，并且 Sqoop 是用 map 任务同步数据可能在任意节点上运行，所以 Sqoop 任务需要配置连接 RDS 的内网地址来连接。另外，需要确保 EMR 集群的内网 IP 在 RDS 白名单里。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/328059/155928899248260_zh-CN.png)

    创建 EMR 集群并指定经典网络类型，具体步骤请参见[创建集群](../../../../cn.zh-CN/集群规划与配置/集群配置/创建集群.md#)。

-   VPC 网络 RDS

    如果 RDS 在 VPC 网络下，EMR 集群也需要指定用 VPC 网络。推荐 EMR 集群和 RDS 在同一个 VPC 网络内，这样可以直接访问 RDS 地址。如果在不同的 VPC 网络下，需要通过[高速通道](https://help.aliyun.com/product/27782.html)打通网络连接。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/328059/155928899248269_zh-CN.png)


## ECS 自建数据库 {#section_5lw_w9o_j3p .section}

-   经典网络

    访问经典网络的 ECS 自建数据库与经典网络的 RDS 类似，也需要 EMR 集群指定使用经典网络，访问自建数据库的内网地址。区别是需要将数据库所在的 ECS 实例和 EMR 集群的实例放在同一个安全组内。您可以在 ECS 控制台**安全组** \> **管理实例** \> **添加实例**

-   VPC 网络

    访问 VPC 网络的自建数据库跟 VPC 网络的 RDS 类似，EMR 集群指定使用 VPC 网络。额外要做的是将数据库 ECS 实例和 EMR 集群实例放到同一个安全组内。


## 云下私有数据库 {#section_mmu_6q6_m8j .section}

有两种方式访问云下私有数据库，一种是绑定弹性IP（EIP）访问数据库的公网地址，一种是将云下数据库通过高速通道和 VPC 网络互联。

-   绑定 EIP

    如果云下私有数据库可以通过公网访问，推荐 EMR 集群使用 VPC 网络。创建一个 VPC 网络类型的 EMR 集群，创建成功后在 ECS 控制台**管理** \> **配置信息** \> **更多** \> **绑定弹性 IP**给集群的每个 ECS 实例绑定一个 EIP，就可以访问私有数据库的公网地址了。

-   高速通道

    如果私有数据库不能在公网暴露，可以创建一个 VPC 网络类型的 EMR 集群，通过高速通道连接私有 IDC 和阿里云上的 VPC 集群。高速通道详情请参见[高速通道产品文档](https://help.aliyun.com/product/27782.html) 


