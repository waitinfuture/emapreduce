# Kafka 跨集群访问 {#concept_zkm_mcx_y2b .concept}

通常，我们会单独部署一个Kafka集群来提供服务，所以经常需要跨集群访问Kafka服务。

## Kafka 跨集群访问说明 {#section_qm5_zcx_y2b .section}

跨集群访问Kafka场景分为两种：

-   阿里云内网环境中访问E-MapReduce Kafka集群。
-   公网环境访问E-MapReduce Kafka集群。

    **说明：** 经典网络的E-MapReduce Kafka集群暂不支持公网访问。


针对不同E-MapReduce主版本，我们提供了不同的解决方案。

## EMR-3.11.x及之后版本 {#section_c4v_bdx_y2b .section}

-   阿里云内网中访问Kafka

    直接使用Kafka集群节点的内网 IP 访问即可，内网访问Kafka请使用 9092 端口。

    访问Kafka前请保证网络是互通的，VPC访问VPC的配置请参见[配置 VPC 到 VPC 连接](../../../../cn.zh-CN/用户指南/配置IPsec-VPN/建立VPC到VPC的连接.md#)。

-   公网环境访问 Kafka

    Kafka 集群的 Core 节点默认无法通过公网访问，所以如果您需要公网环境访问 Kafka 集群，可以参考以下步骤完成：

    1.  使 Kafka 集群和公网主机网络互通。

        Kafka 集群部署在 VPC 网络环境，有两种方式：

        -   集群Core节点挂载弹性公网IP，以下操作步骤使用此方式。
        -   部署高速通道打通内网和公网网络，请参见[高速通道](../../../../cn.zh-CN/产品简介/什么是高速通道？.md#)文档。
    2.  在 E-MapReduce 集群列表页单击对应集群操作栏中的**详情**进入集群基础信息页面。
    3.  单击右上角**网络管理**，在下拉框中选择**挂载公网**。
    4.  配置 Kafka 集群安全组规则来限制公网可访问 Kafka 集群的 IP 等，目的是提高 Kafka 集群暴露在公网中的安全性。您可以在 E-MapReduce 控制台查看到集群所属的安全组，根据安全组 ID 去查找并配置安全组规则。[文档地址](../../../../cn.zh-CN/隐藏/Alibaba Cloud ECS/安全/安全组规则的典型应用.md#)
    5.  在 **集群基础信息**页单击页面右上角的**同步主机信息**，在下拉框中选择**同步主机信息**。
    6.  在**集群与服务管理**页面**服务列表**中依次单击 **Kafka** \> **配置**，在**服务配置**中找到 kafka.public-access.enable参数，并修改为 true。
    7.  重启 Kafka 服务。
    8.  公网环境使用 Kafka 集群节点的 EIP 访问 9093 端口。

## EMR-3.11.x 以前版本 {#section_lnz_hdx_y2b .section}

-   阿里云内网中访问 Kafka

    我们需要在主机上配置 Kafka 集群节点的 host 信息。注意，这里我们需要在 client 端的主机上配置 Kafka 集群节点的**长域名**，否则会出现访问不到 Kafka 服务的问题。示例如下：

    ``` {#codeblock_w11_g1y_dw1}
    ／etc/hosts
    # kafka cluster
    10.0.1.23 emr-header-1.cluster-48742
    10.0.1.24 emr-worker-1.cluster-48742
    10.0.1.25 emr-worker-2.cluster-48742
    10.0.1.26 emr-worker-3.cluster-48742
    ```

-   公网环境访问 Kafka

    Kafka 集群的 Core 节点默认无法通过公网访问，所以如果您需要在公网环境访问 Kafka 集群，可以参考以下步骤完成：

    1.  使 Kafka 集群和公网主机网络互通。

        Kafka 集群部署在 VPC 网络环境，有两种方式：

        -   集群Core节点挂载弹性公网 IP，以下步骤使用此方式。
        -   部署高速通道打通内网和公网网络，参考[高速通道](../../../../cn.zh-CN/产品简介/什么是高速通道？.md#)文档。
    2.  在[VPC 控制台](https://vpcnext.console.aliyun.com/eip)申请 EIP，根据您 Kafka 集群 Core 节点个数购买相应的 EIP。
    3.  配置 Kafka 集群安全组规则来限制公网可访问 Kafka 集群的 IP 等，目的是提高 Kafka 集群暴露在公网中的安全性。您可以在 EMR 控制台查看到集群所属的安全组，根据安全组 ID 去查找并配置安全组规则。[文档地址](../../../../cn.zh-CN/隐藏/Alibaba Cloud ECS/安全/安全组规则的典型应用.md#)
    4.  修改 Kafka 集群的软件配置 listeners.address.principal 为 HOST，并重启 Kafka 集群。
    5.  配置本地客户端主机的 hosts 文件。

