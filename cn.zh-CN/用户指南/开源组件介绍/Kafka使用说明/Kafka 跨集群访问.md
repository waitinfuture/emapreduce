# Kafka 跨集群访问 {#concept_zkm_mcx_y2b .concept}

## Kafka跨集群访问说明 {#section_qm5_zcx_y2b .section}

通常，我们会单独部署一个Kafka集群来提供服务，所以经常需要跨集群访问Kafka服务。跨集群访问Kafka场景分为两种：

-   阿里云内网环境中访问E-MapReduce Kafka集群。
-   公网环境访问E-MapReduce Kafka集群。

针对不同EMR主版本，我们提供了不同的解决方案。

## EMR-3.11.x及之后版本 {#section_c4v_bdx_y2b .section}

-   **阿里云内网中访问Kafka**

    直接使用Kafka集群节点的内网IP访问即可，内网访问Kafka请使用9092端口。

    访问Kafka前请保证网络是互通的：

    -   经典网络访问VPC，请参考[经典网络与VPC互访](cn.zh-CN/用户指南/集群规划/经典网络与VPC互访.md#)。
    -   VPC访问VPC，请参考[配置VPC到VPC连接](https://help.aliyun.com/document_detail/65073.html)。
-   **公网环境访问Kafka**

    Kafka集群的Core节点默认无法访问公网，所以如果您需要公网环境访问Kafka集群，可以参考以下步骤完成：

    1.  使Kafka集群和公网主机网络互通。
        -   Kafka集群部署在VPC网络环境，有两种方式：
            -   部署高速通道打通内网和公网网络，参考[高速通道](https://help.aliyun.com/document_detail/44848.html)文档。
            -   集群Core节点挂载弹性公网IP（[EIP文档](https://help.aliyun.com/product/61789.html)）。以下步骤使用挂载EIP方式。
        -   Kafka集群部署在经典网络，有两种方式：
            -   如果您创建的是按量集群，目前只能通过ECS API来实现，请参考[API文档](https://help.aliyun.com/document_detail/25544.html)。
            -   如果您创建的是包年包月集群，您可以直接在ECS控制台对相应机器变配开启分配公网IP。
    2.  在[ECS控制台](https://vpcnext.console.aliyun.com/eip)申请EIP，根据您Kafka集群Core节点个数购买相应的EIP。
    3.  配置kafka集群安全组规则来限制公网可访问Kafka集群的IP等，目的是提高Kafka集群暴露在公网中的安全性。您可以在EMR控制台查看到集群所属的安全组，根据安全组ID去查找并配置安全组规则。[文档地址](https://help.aliyun.com/document_detail/58746.html)
    4.  在EMR控制台的**集群列表**页面，单击指定集群后的**配置管理**，在页面左侧选择**集群基础信息**页签， 然后单击页面右上角的**同步主机信息**按钮。
    5.  重启集群Kafka服务。
    6.  公网环境使用Kafka集群节点的EIP访问即可，内网访问Kafka请使用9093端口。

## EMR-3.11.x以前版本 {#section_lnz_hdx_y2b .section}

-   **阿里云内网中访问Kafka**

    我们需要在主机上配置Kafka集群节点的host信息。注意，这里我们需要在client端的主机上配置Kafka集群节点的**长域名**，否则会出现访问不到Kafka服务的问题。示例如下：

    ```
    ／etc/hosts
    # kafka cluster
    10.0.1.23 emr-header-1.cluster-48742
    10.0.1.24 emr-worker-1.cluster-48742
    10.0.1.25 emr-worker-2.cluster-48742
    10.0.1.26 emr-worker-3.cluster-48742
    ```

-   **公网环境访问Kafka**

    Kafka集群的Core节点默认无法访问公网，所以如果您需要在公网环境访问Kafka集群，可以参考以下步骤完成：

    1.  使Kafka集群和公网主机网络互通。
        -   Kafka集群部署在VPC网络环境，有两种方式：
            -   部署高速通道打通内网和公网网络，参考[高速通道](https://help.aliyun.com/document_detail/44848.html)文档。
            -   集群Core节点挂载弹性公网IP（[EIP文档](https://help.aliyun.com/product/61789.html)）。以下步骤使用挂载EIP方式。
        -   Kafka集群部署在经典网络，有两种方式：
            -   如果您创建的是按量集群，目前只能通过ECS API来实现，请参考[API文档](https://help.aliyun.com/document_detail/25544.html)。
            -   如果您创建的是包年包月集群，您可以直接在ECS控制台对相应机器变配开启分配公网IP。
    2.  在[ECS控制台](https://vpcnext.console.aliyun.com/eip)申请EIP，根据您Kafka集群Core节点个数购买相应的EIP。
    3.  配置kafka集群安全组规则来限制公网可访问Kafka集群的IP等，目的是提高Kafka集群暴露在公网中的安全性。您可以在EMR控制台查看到集群所属的安全组，根据安全组ID去查找并配置安全组规则。[文档地址](https://help.aliyun.com/document_detail/58746.html)
    4.  修改Kafka集群的软件配置 listeners.address.principal 为 HOST，并重启Kafka集群。
    5.  参考上一节[跨集群访问Kafka](#)，配置本地客户端主机的 hosts 文件。

