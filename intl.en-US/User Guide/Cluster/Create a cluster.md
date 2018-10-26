# Create a cluster {#concept_olg_vq3_y2b .concept}

In this tutorial, you will learn how to create a cluster.

## Enter the cluster creation page {#section_gnx_wq3_y2b .section}

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://emr.console.aliyun.com).
2.  In the left-side navigation bar, click **Cluster** to go to the **Cluster list** page.
3.  Complete RAM authorization. For procedure, see [Role authorization](intl.en-US/User Guide/Role authorization.md#).
4.  Select a region for the cluster. The region cannot be changed once the cluster is created.
5.  In the upper right corner, click **Create cluster**.

## Cluster creation process {#section_eps_wjn_y2b .section}

**Note:** Except for the name, clusters cannot be modified after creation.

To create a cluster, complete the following three steps:

1.  Software configuration

    Configuration description:

    -   **EMR version**: The main version of E-MapReduce represents a complete open source software environment and can be upgraded regularly based on the upgrade of internal component software. If the software related to Hadoop is upgraded, the main version of E-MapReduce is also upgraded. Earlier version clusters cannot be upgraded to a later version.
    -   **Cluster type**: Currently E-MapReduce provides the following cluster types:
        -   Hadoop clusters, provide semi-managed ecosystem components:
            -   Hadoop, Hive, and Spark that offline store and compute distributed data at scale.
            -   SparkStreaming, Flink, and Storm that are stream processing systems.
            -   Presto and Impala, for running interactive analytics queries.
            -   Oozie and Pig.
        -   Druid clusters, provide semi-managed, real-time interactive analysis services, query large amount of data in millisecond latency, and support for multiple data intake methods. Used with services such as EMR Hadoop, EMR Spark, OSS, and RDS, Druid clusters offer real-time query solutions.
        -   Data Science clusters, are mainly for big data and AI scenarios, providing Hive and Spark offline big data, and TensorFlow model training.
        -   Kafka clusters, are taken as a semi-managed message system of high throughput and high scalability, providing a complete service monitoring system that can keep a stable running environment.
    -   **Inclusion configurations**: Displays a list of all software components under the selected cluster type, including the name and version number. You can select different components as required. The selected components start relevant service processes by default.

        **Note:** The more components you select, the higher requirements are for your computer configuration. Otherwise, there may be insufficient resources to run these services.

    -   **High security mode**: In this mode, you can set the Kerberos authentication of the cluster. This feature is unnecessary for clusters used by individual users. It is turned off by default.
    -   **Enable custom setting**: You can specify a JSON file to change software configuration before you start a cluster.
2.  Hardware configuration

    Configuration description:

    -   Billing configuration:
        -   Billing method: The billing method is consistent with ECS. Both Subscription and Pay-As-You-Go modes are supported. If Subscription mode is selected, you must select the duration. It is applicable to short-term testing or flexible dynamic tasks. The payment is relatively high.
        -   Purchase duration: You can select 1, 2, 3, 6, or 9 months, or 1, 2, or 3 years.
    -   Cluster network configuration
        -   **Zone**: Select the zone where the cluster is to be located. If better network connectivity is required, we recommend that you select the same availability zone. However, the risk of cluster creation failure increases as the storage of a single availability zone may be insufficient. If you need a large number of nodes, open a ticket to consult with us.
        -   **Network type**: By default, the Virtual Private Cloud \(VPC\) network is selected which requires you to enter a VPC and a VSwitch. If you haven't created a network, go to the [VPC console](https://vpc.console.aliyun.com/) to create them. For more information about E-MapReduce VPC, see [VPC](intl.en-US/User Guide/VPC.md#).
        -   **VPC**: Select the region of the VPC network.
        -   **VSwitch**: Select a zone for VSwitch under the corresponding VPC. If no VSwitch is available in this zone, then you must create a new one.
        -   **Security group name**: Generally, no security group exists when you create a cluster for the first time. Enter a name to create a new security group. If you already have a security group in use, you can choose to use it directly here.
    -   Cluster configuration
        -   **High availability**: When enabled, two master instances in the Hadoop cluster are used to ensure the availability of the Resource Manager and Name Node. HBase clusters supports high availability by default. When enabled, a master instance is used to ensure high availability.
        -   **Node type**:
            -   Master, the master instance node is mainly responsible for the deployment of control processes such as Resource Manager and Name Node.
            -   Core, the core instance node is mainly responsible for the storage of all data in the cluster, and can be scaled up as needed.
            -   Task, the computing node, does not store data, and is used to adjust the computing capacity of the cluster.
        -   **Node configuration**: Select different types of nodes. Different types of nodes have different application scenarios. You can select one type based on requirements.
        -   **Data disk type**: The data disks used by a cluster node are ordinary cloud disks, high-efficiency cloud disks, and SSD cloud disks which may vary with machine type and region. When the user selects different regions, disks that are supported by the regions are displayed in the drop-down list. The data disk is set to release with the cluster release by default. The ephemeral disk type is set by default and cannot be changed.
        -   **Data disk volume**: The recommended minimum cluster volume of a single machine is 40 G, and the maximum is 8000 G. The capacity of the ephemeral disk is set by default and cannot be changed.
        -   **Instance quantity**: The quantity of instances of all required nodes. A cluster requires at least three instances \(the high availability cluster requires at least four instances, adding one master node\). The maximum is 50. If more than 50 instances are required, contact us by opening a ticket. While a monthly subscribed cluster can provide 100 at most. If you need more than 50 nodes, open a ticket to consult with us.
3.  Basic configuration

    Configuration description:

    -   Basic information

        Cluster name: The cluster name can contain Chinese characters, English letters \(uppercase and lowercase\), numbers, hyphens \(-\), and underscores \(\_\), with a length limit between 1-64 characters.

    -   Running logs
        -   **Running logs**: The function for saving running logs is turned on by default. In the default state, you can select the OSS directory location to save running logs. You must activate OSS before using this function. Cost depends on the number of uploaded files. We recommend that you open the OSS log saving function, which helps in debugging and error screening.
        -   **Log path**: OSS path for saving the log.
        -   **Uniform Meta Database**: Provided by E-MapReduce to store all Hive metadata in the external database of the cluster. We recommend that you use this function when the cluster uses OSS as the main storage.
    -   Permission settings
        -   **EMR role**: You can authorize E-MapReduce with this role to use other Alibaba Cloud services, such as ECS and OSS.
        -   **ECS role**: This role allows your programs running on the E-MapReduce computing nodes to access cloud services like OSS without providing the Alibaba Cloud AccessKey. E-MapReduce automatically applies for an on-demand AccessKey to authorize the access. The AccessKey permission is controlled by this role.
    -   Logon settings
        -   **Remote logon**: It is turned on by default to enable security group port 22.
        -   **Logon password**: Set the logon password at the master node. The logon password must contain English letters \(both uppercase and lowercase letters\), numbers, and special characters \(!@\#$%^&\*\) with a length limit between 8-30 characters.
    -   Bootstrap operation \(optional\): You can run the customized script before Hadoop is enabled in the cluster. For more information, see [Bootstrap action](intl.en-US/User Guide/Bootstrap action.md#).

## Purchase list and cluster cost {#section_mgt_mmn_y2b .section}

In the **Configuration List** pane, you can see the cost of the cluster. The presented price information varies with the type of payment. For Subscription cluster, the total expense is shown. For Pay-As-You-Go cluster, hourly expense is shown.

## Confirm creation {#section_ngt_mmn_y2b .section}

After all valid information is entered, the **Create** button is highlighted. Click **Create** to create a cluster.

**Note:** 

-   If it is a Pay-As-You-Go cluster, the cluster is created immediately, and you are taken back to the **Overivew** page where you can see a cluster in the **Initializing**status. It can take several minutes to create the cluster. After creation, the cluster is switched to Idle **Idle** status.
-   Subscription clusters are not created until the order is generated and paid.

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

If cluster creation failed, the message **Cluster creation failed** appears on the cluster list page. The reason for the failure can be seen when the pointer is placed on the red exclamation point.

No handling is required because the corresponding computing resources are not created. The cluster is automatically hidden after three days.

