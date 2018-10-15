# Create a cluster {#concept_olg_vq3_y2b .concept}

Create a cluster

## Enter the cluster creation page {#section_gnx_wq3_y2b .section}

1.  Log on to [Alibaba Cloud E-MapReduce Console Cluster List](https://emr.console.aliyun.com/#/cluster/region/cn-hangzhou/).
2.  Complete RAM authorization. For procedure, see [Role authorization](intl.en-US/User Guide/Role authorization.md#).
3.  Select a region for the cluster. The region cannot be changed once the cluster is created.
4.  Click **Create a cluster** in the upper right corner.

## Cluster creation process {#section_eps_wjn_y2b .section}

**Note:** Except the name, clusters cannot be modified after creation. Carefully confirm the required configuration.

To create a cluster, complete three following steps:

1.  Software configuration

    **Configuration description:**

    -   **Product version**: The main version of E-MapReduce represents a complete open source software environment and can be upgraded regularly based on the upgrade of internal component software. If the software related to Hadoop is upgraded, the main version of E-MapReduce is also upgraded. Earlier version clusters cannot be upgraded to a later version.

    -   **Cluster type**: Currently E-MapReduce provides the following cluster types:

        -   Hadoop, and Standard Hadoop clusters that contain most of the Hadoop-related components. Details about the components are provided in the selection list.
        -   Kafka, and Independent Kafka clusters that provide messaging service.
    -   **Inclusion configuration**: Displays a list of all software components under the selected cluster type, including the name and version number. You can select different components as required. The selected components start relevant service processes by default.

**Note:** The more components you select, the higher requirements are for your computer configuration. Otherwise, there may be insufficient resources to run these services.

    -   **Security mode**: Whether to enable the Kerberos authentication function of the cluster.

    -   **Software configuration \(optional\)**: Hadoop, Spark, Hive, and other basic software in the cluster can be configured. For more information, see [Software configuration](intl.en-US/User Guide/Software configuration.md#).

2.  Hardware configuration

    **Configuration description:**

    -   **Billing configuration:**

        -   Billing method: The billing method is consistent with ECS. Both Subscription and Pay-As-You-Go modes are supported. If Subscription mode is selected, you must select the duration. It is applicable to short-term testing or flexible dynamic tasks. The payment is relatively high.

        -   Purchase duration: You can select 1, 2, 3, 6, or 9 months, or 1, 2, or 3 years.

    -   **Cluster network configuration**

        -   Availability zone of the cluster: Select the zone where the cluster is to be located. Different zones can have different machine types and disks. There are several availability zones in each region. The availability zone belongs to different physical areas. If better network connectivity is required, we recommend that you select the same availability zone. However, the risk of cluster creation failure increases as the storage of a single availability zone may be insufficient. If you need a large number of nodes, open a ticket to consult with us.

        -   Network type: Classic network and Virtual Private Cloud \(VPC\) can be selected. VPC requires an additional subordinate VPC and subnet \(VSwitch\). Click Create VPC/subnet \(VSwitch\) on the page to enter the [VPC console](https://vpc.console.aliyun.com/). Then refresh the list to see the created VPC/subnet \(VSwitch\). For more information about E-MapReduce VPC, see [VPC](intl.en-US/User Guide/VPC.md#).

            Classic network and VPC are not interoperable. The network type cannot be changed after purchase.

        -   ECS instance series: In terms of ECS instance series, different zones have different instance series \(such as I, II, III\). We recommend you use the latest generation.

        -   VPC: Select the region of VPC network.

        -   VSwitch: Select a zone for VSwitch under the corresponding VPC. If no VSwitch is available in this Zone, then you must create a new one.

        -   New security group: No security group exists when you create the cluster for the first time. Click New security group, and input the new security group name.

        -   Main security group: The security group that the cluster belongs to. Only the security group created by the user in E-MapReduce product is displayed. The security group cannot be created outside the E-MapReduce. To create a security group, select New security group and input the security group name. The name can contain Chinese characters, English letters, numbers, hyphens \(-\), and underscores \(\_\), with a length limit between 2-64 characters.

    -   **Cluster node configuration**

        -   High availability cluster: After opening, Hadoop cluster has two masters to support the high availability of Resource Manager and Name Node. Originally, HBase cluster supports the high availability, but another node serves as a core node. If the high availability is activated, an independent master node is used to support it, which is more secure and reliable. The default mode is non-high availability, and only one master node exists.

        -   Node type:

            -   Master, the master instance node is mainly responsible for the deployment of control processes such as Resource Manager and Name Node.
            -   Core, the core instance node is mainly responsible for the storage of all data in the cluster, and can be scaled up as needed.
            -   Task, the computing node, does not store data, and is used to adjust the computing capacity of the cluster.
        -   Node configuration: Select different types of nodes. Different types of nodes have different application scenarios. You can select one type based on requirements.

        -   Data disk type: The data disks used by a cluster node are ordinary cloud disks, high-efficiency cloud disks, and SSD cloud disks which may vary with machine type and region. When the user selects different regions, disks that are supported by the regions are displayed in the drop-down list. The data disk is set to release with the cluster release by default. The ephemeral disk type is set by default and cannot be changed.

        -   Data disk volume: The recommended minimum cluster volume of a single machine is 40 G, and the maximum is 8000 G. The capacity of the ephemeral disk is set by default and cannot be changed.

        -   Instance quantity: The quantity of instances of all required nodes. A cluster requires at least three instances \(the high availability cluster requires at least four instances, adding one master node\). The maximum is 50. If more than 50 instances are required, contact us by opening a ticket. While a monthly subscribed cluster can provide 100 at most. If you need more than 50 nodes, open a ticket to consult with us.

3.  Basic configuration

    **Configuration description:**

    -   **Basic information**

        Cluster name: The cluster name can contain Chinese characters, English letters \(uppercase and lowercase\), numbers, hyphens \(-\), and underscores \(\_\), with a length limit between 1-64 characters.

    -   **Operation log**

        -   Operation log: The function for saving the operation log is turned on by default. In the default state, you can select the OSS directory location to save the operation log. You must activate OSS before using this function. Cost depends on the number of uploaded files. We recommend that you open the OSS log saving function, which helps in debugging and error screening.

        -   Log path: OSS path for saving the log.

        -   Central metadatabase: Provided by E-MapReduce to store all Hive metadata in the external database of the cluster. We recommend that you use this function when the cluster uses OSS as the main storage.

    -   **Permission setting**

        -   Service role: You can authorize E-MapReduce with this role to use other Alibaba Cloud services like ECS and OSS.
        -   ECS application role: This role allows your programs running on the E-MapReduce computing nodes to access cloud services like OSS without providing the Alibaba Cloud AccessKey. E-MapReduce automatically applies for an on-demand AccessKey to authorize the access. The AccessKey permission is controlled by this role.
    -   **Logon setting**

        Logon password: Set the logon password at the master node. The logon password must contain English letters \(both uppercase and lowercase letters\), numbers, and special characters with a length limit between 8-30 characters.

    -   **Bootstrap action \(optional\)**: You can run the customized script before Hadoop is enabled in the cluster. For more information, see [Bootstrap action](intl.en-US/User Guide/Bootstrap action.md#).


## Purchase list and cluster cost {#section_mgt_mmn_y2b .section}

Your configuration list and cluster cost is shown on the right side of the page. The presented price information varies with the type of payment. For Subscription cluster, the total expense is shown. For Pay-As-You-Go cluster, hourly expense is shown.

## Confirm creation {#section_ngt_mmn_y2b .section}

After all valid information is inputted, the **Create button** is highlighted. Verify the information, and click **Create** to create clusters.

**Note:** 

-   If it is a Pay-As-You-Go cluster, the cluster is created immediately, and you are taken back to the Cluster List page where you can see a cluster in **Cluster Creation**status. It can take several minutes to create the cluster. After creation, the cluster is switched to Idle **Idle** status.

-   For Subscription cluster, the cluster is not created until the order is generated and paid.


## Log on to the Core Node {#section_pgt_mmn_y2b .section}

To log on to the Core node, take the following steps:

1.  Switch to the hadoop account on the Master node.

    ```
    su hadoop
    ```

2.  Log on to the Core node without a key through SSH.

    ```
    ssh emr-worker-1
    ```

3.  Get root permissions through the sudo command.

    ```
    sudo vi /etc/hosts
    ```


## Creation failed {#section_xz3_vmn_y2b .section}

If the cluster creation fails, the cluster list page displays a message: **Cluster creation failed**. To see the reason for failure, place the cursor over the red exclamation point.

No handling is required because the corresponding computing resources are not created. The cluster is automatically hidden after three days.

