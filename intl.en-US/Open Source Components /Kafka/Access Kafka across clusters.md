# Access Kafka across clusters {#concept_zkm_mcx_y2b .concept}

Typically, Kafka services are deployed on a single cluster. Therefore, you have to access Kafka across clusters in certain scenarios.

## Scenarios {#section_qm5_zcx_y2b .section}

You may need to access Kafka across clusters in the following scenarios:

-   Access an E-MapReduce \(EMR\) Kafka cluster from a VPC network.
-   Access an EMR Kafka cluster from the public network.

    **Note:** If the EMR Kafka cluster is deployed in the classic network, you cannot access the cluster from the public network.


Different solutions are provided based on the version of EMR.

## V3.11.X and later versions {#section_c4v_bdx_y2b .section}

-   Access Kafka from a VPC network.

    You can access the Kafka service by connecting to the private IP address of a node in the Kafka cluster. Use port 9092 for accessing Kafka from a VPC network.

    Before you access the Kafka service, make sure that your VPC network is connected to the VPC network where the cluster is deployed. For more information, see [Connect a VPC network to another VPC network](../../../../reseller.en-US/User Guide/Configure IPsec-VPN connections/Establish a connection between two VPCs.md#).

-   Access Kafka from the public network.

    By default, core nodes in a Kafka cluster cannot be accessed from the public network. You can perform the following steps to access a Kafka cluster from the public network.

    1.  Enable communication between the Kafka cluster and the client in the public network.

        To connect to a Kafka cluster deployed in a VPC network, use the following methods:

        -   Connect to the Elastic IP Address \(EIP\) of a core node in the Kafka cluster. The follow steps describe how to use this method to access Kafka.
        -   Use Express Connect to connect the VPC network to the public network. For more information, see [Express Connect](../../../../reseller.en-US/Product Introduction/What is Express Connect?.md#).
    2.  On the Cluster Management page, click **View Details** for the target cluster. You are then navigated to the Cluster Overview page.
    3.  Click **Network Management**. From the drop-down list, select **Assign Public IP Address**.
    4.  Configure security group rules for the Kafka cluster to specify the public IP addresses that are allowed to access the Kafka cluster. This helps you protect the Kafka cluster when it is connected to the public network. You can log on to the EMR console to check the security group of the Kafka cluster, use the security group ID to find the security group in the ECS console, and then configure rules. [View details](../../../../reseller.en-US//Typical applications of security group rules.md#)
    5.  On the **Cluster Overview** page, click **Instance State Management**. From the drop-down list, select **Sync Cluster Host Info**.
    6.  In the **Services** list on the **Clusters and Services** page, choose **Kafka** \> **Configuration**. In the **Service Configuration** section, set the value of the kafka.public-access.enable parameter to true.
    7.  Restart the Kafka service.
    8.  Use the EIP of the cluster and port 9093 to connect to the Kafka cluster.

## Versions before V3.11.X {#section_lnz_hdx_y2b .section}

-   Access Kafka from a VPC network.

    You need to modify the host configuration on your client for the Kafka cluster nodes. Note: You need to configure the **long domains** of the Kafka cluster nodes on the client to avoid connection failures. Example:

    ``` {#codeblock_w11_g1y_dw1}
    /etc/hosts
    # kafka cluster
    10.0.1.23 emr-header-1.cluster-48742
    10.0.1.24 emr-worker-1.cluster-48742
    10.0.1.25 emr-worker-2.cluster-48742
    10.0.1.26 emr-worker-3.cluster-48742
    ```

-   Access Kafka from the public network.

    By default, core nodes of a Kafka cluster cannot be accessed from the public network. You can perform the following steps to access a Kafka cluster from the public network.

    1.  Enable communication between the Kafka cluster and the client in the public network.

        To connect to a Kafka cluster deployed in a VPC network, use the following methods:

        -   Connect to the EIP of a core node in the Kafka cluster. The follow steps describe how to use this method to access Kafka.
        -   Use Express Connect to enable the connection between the VPC network and the public network. For more information, see [Express Connect](../../../../reseller.en-US/Product Introduction/What is Express Connect?.md#).
    2.  In the [VPC console](https://partners-intl.aliyun.com/login-required#/vpc/eip), purchase EIPs. The number of purchased EIPs equals the number of core nodes in the Kafka cluster.
    3.  Configure security group rules for the Kafka cluster to specify the public IP addresses that are allowed to access the Kafka cluster. This helps you protect the Kafka cluster when it is connected to the public network. You can log on to the EMR console to check the security group of the Kafka cluster, use the security group ID to find the security group in the ECS console, and then configure rules. [View details](../../../../reseller.en-US//Typical applications of security group rules.md#)
    4.  In the Service Configuration section, set the value of the listeners.address.principal parameter to HOST. Restart the Kafka cluster.
    5.  Configure the hosts file on your client.

