# Create a cluster {#concept_olg_vq3_y2b .concept}

In this tutorial, you will learn how to create an Alibaba Cloud E-MapReduce \(EMR\) cluster.

## Go to the EMR cluster creation page {#section_gnx_wq3_y2b .section}

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://emr.console.aliyun.com).
2.  Complete RAM authorization. For details, see [Role authorization](intl.en-US/User Guide/Role authorization.md#).
3.  Select a region for the cluster. The region cannot be changed once the cluster is created.
4.  Click **Create Cluster** to go to the cluster creation page.

## Create a cluster {#section_eps_wjn_y2b .section}

**Note:** After you create an EMR cluster, the only thing that can be changed is its name.

To create a cluster, follow these three steps:

1.  Configure the software.
    -   **EMR version**: The main version of E-MapReduce represents a complete open source software environment and can be upgraded regularly based on upgrades made to the internal component software. If the software related to Hadoop is upgraded, the main version of E-MapReduce is also upgraded. Clusters from an earlier version cannot be upgraded to a later version.
    -   **Cluster type**: Currently, E-MapReduce provides four cluster types.
        -   Hadoop clusters, which provide the following semi-managed ecosystem components:
            -   Hadoop, Hive, and Spark for large-scale offline distributed data storage and computing.
            -   Spark Streaming, Flink, and Storm for stream processing.
            -   Presto and Impala for running interactive analytics.
            -   Oozie and Pig.
        -   Druid clusters, which provide semi-managed, real-time interactive analysis services, query large amounts of data at millisecond latency, and support multiple data intake methods. When used with services such as EMR Hadoop, EMR Spark, OSS, and RDS, Druid clusters offer real-time query solutions.
        -   Data Science clusters, which are mainly applicable in big data and AI scenarios, providing Hive and Spark offline big data, and TensorFlow model training.
        -   Kafka clusters, which are semi-managed distributed message systems that feature high throughput and high scalability, providing a complete service monitoring system that can keep a stable running environment.
    -   **Required Services**: Displays a list of all software components under the selected cluster type, including their name and version number.
    -   **Optional Services**:You can select different components as required. The selected components start relevant service processes by default.

        **Note:** The more components you select, the higher the requirements are for configuration, as there may be insufficient resources to run these services.

    -   **High security mode**: In this mode, you can set the cluster's Kerberos authentication. This feature is unnecessary for clusters used by individual users and is turned off by default.
    -   **Enable custom setting**: Before you start a cluster, you can specify a JSON file to change the software configuration.
2.  Configure the hardware.
    -   Billing method
        -   Like with ECS, both Subscription and Pay-As-You-Go modes are supported. If you select Subscription mode, you must also select the duration. You can select 1, 2, 3, 6, or 9 months, or 1, 2, or 3 years. This mode is applicable to short-term testing or flexible dynamic tasks, but is relatively expensive.
    -   Cluster network configuration
        -   **Zone**: Select the zone where the cluster is to be located. If better network connectivity is required, we recommend selecting the same availability zone. However, this increases the risk of failure when creating a cluster, as the availability zone's storage may be insufficient. If you need a large number of nodes, please submit a ticket.
        -   **Network type**: The Virtual Private Cloud \(VPC\) network is selected by default, which requires you to enter a VPC and a VSwitch. If you have not created a network, go to the [VPC console](https://vpc.console.aliyun.com/) to create one. For more information about E-MapReduce VPC, see [VPC](intl.en-US/User Guide/VPC.md#).
        -   **VPC**: Select the region of the VPC network.
        -   **VSwitch**: Select a zone for VSwitch under the corresponding VPC. If no VSwitch is available in this zone, you must create a new one.
        -   **Security group name**: A security group does not typically exist when you first create a cluster. To create a new security group, enter a name. If you already have a security group, you can select it here.
    -   Cluster configuration
        -   **High availability**: When enabled, two master instances in the Hadoop cluster are used to ensure the availability of the Resource Manager and Name Node. HBase clusters support high availability by default.
        -   **Node type**: The three types of node supported are as follows:
            -   Master, which is mainly responsible for the deployment of control processes such as Resource Manager and Name Node.
            -   Core, which is mainly responsible for the storage of all data in the cluster, and can be scaled up as required.
            -   Task, which is the node used for computing. It does not store data and is used to adjust the computing capacity of the cluster.
        -   **Node configuration**: Select different node types. Different types of nodes have different application scenarios. You can select a type based on your requirements.
        -   **Data disk type**: The data disks used by a cluster node are either standard cloud disks, high-efficiency cloud disks, or SSD cloud disks. This varies between machine type and region. When the user selects different regions, disks that are supported by those regions are displayed in the drop-down list. By default, data disks are released when the cluster is released. The ephemeral disk type is set by default and cannot be changed.
        -   **Data disk volume**: The recommended minimum cluster volume for a single machine is 40 G, and the maximum is 8000 G. The capacity of the ephemeral disk is set by default and cannot be changed.
        -   **Instance quantity**: This indicates the number of instances of all required nodes. A cluster requires at least three instances. However, high availability clusters require at least four, and therefore add one master node. The maximum is 50. If more than 50 instances are required, please submit a ticket. A monthly subscribed cluster can provide 100 at most. If you need more than 50 nodes, please submit a ticket.
3.  Configure the basic information.
    -   Basic information

        -   Cluster name: The cluster name can contain Chinese characters, English letters \(uppercase and lowercase\), numbers, hyphens \(-\), and underscores \(\_\), with a length of between 1-64 characters.
    -   Running logs
        -   **Running logs**: The function for saving running logs is turned on by default. In the default state, you can select the OSS directory as a location to save running logs, but you must have activated OSS before using this function. The cost depends on the number of uploaded files. We recommend that you open the OSS log saving function, which helps in debugging and error screening.
        -   **Log path**: OSS path for saving logs.
        -   **Uniform Meta Database**: This is provided by E-MapReduce to store all Hive metadata in the external database of the cluster. We recommend that you use this function when the cluster uses OSS as the main storage.
    -   Permission settings
        -   **EMR role**: This role authorizes E-MapReduce to use other Alibaba Cloud services, such as ECS and OSS.
        -   **ECS role**: This role allows your programs running on the E-MapReduce computing nodes to access cloud services like OSS without providing the Alibaba Cloud AccessKey. E-MapReduce automatically applies for an on-demand AccessKey to authorize access. The AccessKey permission is controlled by this role.
    -   Logon settings
        -   **Remote logon**: It is turned on by default to enable security group port 22.
        -   **Logon password**: Set the logon password at the master node. The logon password must contain English letters \(both uppercase and lowercase letters\), numbers, and special characters \(!@\#$%^&\*\) with a length of between 8-30 characters.
    -   \(Optional\) Bootstrap operation: Before Hadoop is enabled in the cluster, you can run the customized script. For more information, see [Bootstrap action](intl.en-US/User Guide/Bootstrap actions.md#).

## Purchase lists and cluster costs {#section_mgt_mmn_y2b .section}

The cost of the cluster is displayed in the **Configuration List** pane. The price varies with the type of payment. For Subscription clusters, the total expense is shown. For Pay-As-You-Go clusters, the hourly cost is shown.

## Confirm creation {#section_ngt_mmn_y2b .section}

Once you have entered all the necessary information, the **Create** button is highlighted. Click **Create** to create the cluster.

**Note:** 

-   If your cluster is Pay-As-You-Go, it is created immediately, and you are taken back to the **Overview** page. Here, your cluster is displayed with the status **Initializing**. It can take several minutes to finish creating the cluster. After the cluster is created, its status is switched to **Idle**.
-   Subscription clusters are not created until the order is generated and paid.

## Log on to the core node {#section_pgt_mmn_y2b .section}

To log on to the core node, perform the following steps:

1.  Switch to the Hadoop account on the Master node.

    ```
    su hadoop
    ```

2.  Log on to the core node through SSH without a key.

    ```
    ssh emr-worker-1
    ```

3.  Get root permissions through the sudo command.

    ```
    sudo vi /etc/hosts
    ```


## Failure during cluster creation {#section_xz3_vmn_y2b .section}

If a cluster fails to be created, the message **Cluster creation failed** is displayed on the cluster list page. If you hover your cursor over the red exclamation point, the reason for the failure is displayed.

You do not need to perform any additional operations because the corresponding computing resources are not created. The cluster is automatically hidden after three days.

