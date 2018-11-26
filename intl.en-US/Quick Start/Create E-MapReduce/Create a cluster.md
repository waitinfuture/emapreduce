# Create a cluster {#concept_nrp_154_y2b .concept}

In this tutorial, you will learn how to create a cluster.

## Enter the cluster creation page {#section_w2j_2z4_y2b .section}

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://emr.console.aliyun.com/#/ap-southeast-1).
2.  Complete RAM authorization. For procedure, see [Role authorization](../../../../intl.en-US/User Guide/Role authorization.md#).
3.  Select a region for a cluster. The region cannot be changed once the cluster is created.
4.  Click **Create Cluster** to create a cluster.

## Cluster creation process {#section_rsk_gz4_y2b .section}

To create a cluster, follow the below steps:

1.  Step one: Software configuration

    Configuration description

    -   **EMR version**: Select the latest version by default.
    -   **Cluster type**: Currently E-MapReduce provides the following cluster types:
        -   Hadoop clusters, provide semi-managed ecosystem components:
            -   Hadoop, Hive, and Spark that offline store and compute distributed data at scale.
            -   SparkStreaming, Flink, and Storm that are stream processing systems.
            -   Presto and Impala, for running interactive analytics queries.
            -   Oozie and Pig.
        -   Druid clusters, provide semi-managed, real-time interactive analysis services, query large amount of data in millisecond latency, and support for multiple data intake methods. Used with services such as EMR Hadoop, EMR Spark, OSS, and RDS, Druid clusters offer real-time query solutions.
        -   Data Science clusters, are mainly for big data and AI scenarios, providing Hive and Spark offline big data, and TensorFlow model training.
        -   Kafka clusters, are taken as a semi-managed distributed message system of high throughput and high scalability, providing a complete service monitoring system that can keep a stable running environment.
    -   **Include configurations**: Use the default configuration. You can add, start and stop services in the management interface later.
    -   **High security mode**: In this mode, you can set the Kerberos authentication of the cluster. This feature is unnecessary for clusters used by individual users. It is turned off by default.
    -   **Enable custom setting**: You can specify a JSON file to change software configuration before you start a cluster.
2.  Hardware configuration

    Configuration description

    -   Billing configuration
        -   **Billing method**: Pay-As-You-Go is used in testing scenarios. Subscription production clusters can be created after all tests are verified.
    -   Network configuration
        -   **Zone**: Generally use the default zone.
        -   **Network type**: By default, the Virtual Private Cloud \(VPC\) network is selected which requires you to enter a VPC and a VSwitch. If you haven't created a network, go to the [VPC console](https://vpc.console.aliyun.com/) to create them.
        -   **VPC**: Select the region of the VPC network.
        -   **VSwitch**: Select a zone for VSwitch under the corresponding VPC. If no VSwitch is available in this zone, then you must create a new one.
        -   **Security group name**: Generally, no security group exists when you create a cluster for the first time. Enter a name to create a new security group. If you already have a security group in use, you can choose to use it directly here.
    -   Cluster configuration
        -   **High availability**: When enabled, two master instances in the Hadoop cluster are used to ensure the availability of the Resource Manager and Name Node. HBase clusters support high availability by default. When enabled, a master instance is used to ensure high availability.
        -   Master node
            -   **Master instance type**: Select an instance type as required. For information about instance types, see [Instance type families](../../../../intl.en-US/Product Introduction/Instance type families.md#).
            -   **System disk type**: Select a disk as required.
            -   **System disk size**: We recommend that you select 120G at least.
            -   **Data disk type**: Select a disk as required.
            -   **Data disk size**: We recommend that you select 80G at least.
            -   **Master instances**: It is set 1 master instance by default.
        -   Core node
            -   **Core instance type**: Select an instance type as required. For information about instance types, see [Instance type families](../../../../intl.en-US/Product Introduction/Instance type families.md#).
            -   **System disk type**: Select a disk as required.
            -   **System disk size**: We recommend that you select 80G at least.
            -   **Data disk type**: Select a disk as required.
            -   **Data disk size**: We recommend that you select 80G at least.
            -   **Core instances**: It is set 2 instances by default. You can adjust as required.
        -   Task instance group: It is turned off by default.
3.  Basic configuration

    Configuration description

    -   Basic information
        -   **Cluster name**: The cluster name can contain Chinese characters, English letters \(uppercase and lowercase\), numbers, hyphens \(-\), and underscores \(\_\), with a length limit between 1-64 characters.
    -   Running logs
        -   **Running logs**: The function for saving running logs is turned on by default. In the default state, you can select the OSS directory location to save running logs. You must activate OSS before using this function. Cost depends on the number of uploaded files. We recommend that you open the OSS log saving function, which helps in debugging and error screening.
        -   **Log path**: OSS path for saving the log.
        -   **Uniform Meta Database**: We recommend you disable this feature for the moment.
    -   Permission settings: Use default settings.
    -   Logon settings
        -   **Remote logon**: It is turned on by default to enable security group port 22.
        -   **Logon password**: Set the logon password at the master node. The logon password must contain English letters \(both uppercase and lowercase letters\), numbers, and special characters \(!@\#$%^&\*\) with a length limit between 8-30 characters.

## Purchase list and cluster cost {#section_gtk_gz4_y2b .section}

Confirm the configured items and the billing in the configuration list.

## Confirm creation {#section_htk_gz4_y2b .section}

After all configurations are set, the **Create** button is highlighted. Verify the information, and click **Create** to create clusters.

**Note:** 

-   If it is a Pay-As-You-Go cluster, the cluster is created immediately, and you are taken back to the **Overview** page where you can see a cluster in **Initializing** status. It takes several minutes to create the cluster. After creation, the cluster is switched to the **Idle** status.
-   For subscribed clusters, the cluster is not created until the order is generated and paid.

## Creation failure {#section_jtk_gz4_y2b .section}

If cluster creation failed, the message **Cluster creation failed** appears on the cluster list page. The reason for the failure can be seen when the pointer is placed on the red exclamation point.

No handling is required because the corresponding computing resources are not created. The cluster is automatically hidden after three days.

