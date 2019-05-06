# Cross-cluster access to Kafka {#concept_zkm_mcx_y2b .concept}

Typically, Kafka services are provided through a separate cluster. You need to cross clusters to access Kafka services.

## Scenarios of cross-cluster access to Kafka {#section_qm5_zcx_y2b .section}

The scenarios of cross-cluster access to Kafka are described as follows.

-   You access an EMR Kafka cluster from a private network.
-   You access an EMR Kafka cluster from a public network.

Different solutions are provided based on the version of EMR.

## V3.11.X and later versions {#section_c4v_bdx_y2b .section}

-   Access Kafka from a private network

    You can access Kafka services by connecting to the private IP addresses of the cluster nodes. The port number is 9092.

    Make sure the networks are interconnected before you access Kafka.

    -   See [Interconnect classic networks and VPCs](reseller.en-US/Cluster Planning and Configurations/Cluster planning/Enable access between classic networks and VPCs.md#).
    -   See [Configure VPC peering connections](../../../../reseller.en-US/User Guide/Configure IPsec-VPN connections/Configure a VPC-to-VPC connection.md#).
-   Access Kafka in the public network

    By default, core nodes of a Kafka cluster cannot be accessed from the public network. You can perform the following steps to access a Kafka cluster over a public network.

    1.  Enable communication between the Kafka cluster and the host in the public network.
        -   The following methods are based on a Kafka cluster that is deployed in a VPC.
            -   Use Express Connect to enable the connection between the private network and the public network. For more information, see [Express Connect](../../../../reseller.en-US/Product Introduction/What is Express Connect?.md#).
            -   Mount the core nodes of a cluster with EIPs. The following steps are based on this method.
        -   The following methods are based on a Kafka cluster that is deployed in a classic network.
            -   For a Pay-As-You-Go cluster, you need to use ECS APIs. See [AllocatePublicIpAddress](../../../../reseller.en-US/API Reference/Networks/AllocatePublicIpAddress.md#).
            -   For a Subscription cluster, you can associate an EIP with the specified instance in the ECS console.
    2.  On the Cluster Management page, click **View Details** for a cluster to go to the Cluster Overview page.
    3.  Click **Network Management**. From the drop-down list, select **Assign Public IP Address**.
    4.  Configure security group rules for the Kafka cluster to specify the public IP addresses that can access the Kafka cluster. By doing this, the security of the Kafka cluster is improved. You can view the security group of the cluster and configure the security group rules in the EMR console. [For more information, see Typical applications of security group rules.](../../../../reseller.en-US//Typical applications of security group rules.md#)
    5.  On the **Cluster Overview** page, click **Instance State Management**. From the drop-down list, select **Sync Cluster Host Info**.
    6.  On the **Clusters and Services** page, choose **Kafka** \> **Configuration**. In the **Service Configuration** section, specify the value of the kafka.public-access.enable parameter to true.
    7.  Restart Kafka.
    8.  You can access Kafka services by connecting to the EIPs of the cluster nodes. The port number is 9093.

## Earlier versions before V3.11.X {#section_lnz_hdx_y2b .section}

-   Access Kafka from a private network

    You need to configure the host information of the Kafka cluster nodes. Note: You need to configure the **long domains** of the Kafka cluster nodes on the client to avoid failed access to Kafka services. Example:

    ```
    ／etc/hosts
    # kafka cluster
    10.0.1.23 emr-header-1.cluster-48742
    10.0.1.24 emr-worker-1.cluster-48742
    10.0.1.25 emr-worker-2.cluster-48742
    10.0.1.26 emr-worker-3.cluster-48742
    ```

-   Access Kafka from a public network

    By default, core nodes of a Kafka cluster cannot be accessed from a public network. If you need to access the Kafka cluster from a public network, perform the following steps.

    1.  Enable communication between the Kafka cluster and the host in the public network.
        -   The following methods are based on a Kafka cluster that is deployed in a VPC.
            -   Use Express Connect to enable the connection between the private network and the public network. For more information, see [Express Connect](../../../../reseller.en-US/Product Introduction/What is Express Connect?.md#).
            -   Associate EIPs with the core nodes of the cluster. Perform the following steps.
        -   The following methods are based on a Kafka cluster that is deployed in a classic network.
            -   For a Pay-As-You-Go cluster, you need to use ECS APIs. See [AllocatePublicIpAddress](../../../../reseller.en-US/API Reference/Networks/AllocatePublicIpAddress.md#).
            -   For a Subscription cluster, you can associate an EIP with the specified instance in the ECS console.
    2.  In the [VPC console](https://partners-intl.aliyun.com/login-required#/vpc/eip), purchase EIPs according to the number of the core nodes of the Kafka cluster.
    3.  Configure security group rules for the Kafka cluster to specify the public IP addresses that can access the Kafka cluster. By doing this, the security of the Kafka cluster is improved. You can view the security group of the cluster and configure the security group rules in the EMR console. [For more information, see Typical applications of security group rules.](../../../../reseller.en-US//Typical applications of security group rules.md#)
    4.  In the Service Configuration section, specify the value of the listeners.address.principal parameter to HOST. Restart the Kafka cluster.
    5.  Refer to Step 5 and configure the hosts file of the local client.

