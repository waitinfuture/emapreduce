# Step 3Create a cluster {#concept_nrp_154_y2b .concept}

This section describes how to create and configure an E-MapReduce \(EMR\) cluster.

## Prerequisite {#section_pgx_g4v_vgb .section}

Confirm that RAM authorization has been completed. For more information, see [Role authorization](../../../../reseller.en-US/Cluster Planning and Configurations/Cluster planning/Role authorization.md#).

## Go to the cluster creation page {#section_w2j_2z4_y2b .section}

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://partners-intl.console.aliyun.com/#/emr).
2.  Select a region where you want to create the cluster. The region cannot be changed after the cluster has been created.
3.  Click **Cluster Wizard** in the upper-right corner.

## Create a cluster {#section_rsk_gz4_y2b .section}

To create a cluster, follow these steps:

-   Software Settings
-   Hardware Settings
-   Basic Settings

Step 1: Software Settings

Description

-   **EMR Version**: By default, the latest version is selected.
-   **Cluster Type**:
    -   Hadoop clusters. These clusters provide multiple ecosystem components, such as Hadoop, Hive, Spark, Spark Streaming, Flink, Storm, Presto, Impala, Oozie, and Pig. Hadoop, Hive, and Spark are semi-hosted services and are used for distributed large-scale data storage and computing. Spark Streaming, Flink, and Storm can provide stream computing. Presto and Impala are used to achieve interactive queries. For more information about the components, see Services List displayed on the cluster and service management page.
    -   Kafka clusters. These clusters serve as a semi-hosted distributed message system with high throughput and high scalability. Kafka clusters provide a comprehensive service monitoring system that keeps the clusters running stably. Kafka clusters are more professional, reliable, and secure. You do not need to deploy or maintain these clusters. These clusters are commonly used in scenarios such as log collection and monitoring data aggregation. Offline data processing and stream computing, and real-time data analysis are also supported.
    -   Druid clusters. These clusters provide semi-hosted and real-time interactive analysis services. Druid clusters also support querying large amounts of data in milliseconds and writing data through multiple methods. Druid clusters offer flexible and stable real-time queries together with other services such as EMR Hadoop, EMR Spark, OSS, and RDS.
    -   Data Science clusters. These clusters are commonly applied in big data and AI scenarios. Data Science clusters provide Hive and Spark offline big data ETL, and TensorFlow model training. You can choose the CPU plus GPU heterogeneous computing framework and the deep learning algorithm supported by NVIDIA CPUs to run computing tasks efficiently.
-   **Required Services**: Select the default configuration. You can add, enable, or disable services on the management page later.
-   **Optional Services**:You can select different components as required. The selected components start relevant service processes by default.
-   Advanced Settings
    -   **Kerberos Mode**: Indicates whether to enable the Kerberos authentication feature for the cluster. This mode is disabled by default because typically this mode is not required by clusters created for general users.
    -   **Custom Software Settings**: You can specify a JSON file to modify software configuration. For more information about the procedure, see [Software configuration](../../../../reseller.en-US/Cluster Planning and Configurations/Third-party software/Software configuration.md#).

Step 2: Hardware Settings

Description

-   Billing Configuration

    -   **Billing Method**: You can select Pay-As-You-Go when testing the cluster. After all tests have been passed, you can create and use a Subscription-based cluster.
        -   **Pay-As-You-Go**
        -   **Subscription** 
            -   **Subscription Period**:You can get 15% off your total cost by choosing annual subscription.
            -   **Auto Renewal**:Your subscription will be automatically renewed seven days before the expiration date. The subscription duration is one month.
-   Network Settings
    -   **Zone**: Geographical areas that are located in the same region. These zones are interconnected through VPCs. Typically, the default zone is used.
    -   **Network Type**: VPC is selected by default. If you have not created a VPC, go to the [VPC console](https://partners-intl.console.aliyun.com/#/vpc) to create one.
    -   **VPC**: Select a VPC that has been created in the specified region. If no VPC is available, click **Create VPC/VSwitch** to create one in the current zone.
    -   **VSwitch**: Select a VSwitch for the specified VPC in the current zone. If no VSwitch is available, go to the VPC console and create one in the current zone.
    -   **Security Group Name**: By default, no security group is available if this is the first time that you have created a cluster. You need to enter a name and then create a security group. If you have already created security groups, select a security group.
-   Cluster configuration
    -   **High Availability**: After this feature has been enabled, two master nodes will be provided to guarantee the high availability of ResourceManager and NameNode. HBase clusters support high availability by default. An HBase cluster has to use a core node as one of the two master nodes. When the high availability feature is enabled, the HBase cluster only needs one mater node to support high availability, which is more secure and reliable. If you need to create clusters that support high availability, enable high availability during testing.
    -   Master Instance: Deploys processes, such as ResourceManager and NameNode.

        You can select instance specifications based on your needs. For more information, see [Instance type families](../../../../reseller.en-US/Instances/Instance type families.md#).

        -   **System Disk Type**: Select Ultra Disk or SSD Disk based on your needs.
        -   **System Disk Size**: You can resize the disk based on your needs. The recommended minimum disk size is 120 GB.
        -   **Data Disk Type**: Select Ultra Disk or SSD Disk based on your needs.
        -   **Data Disk Size**: You can resize the disk. The recommended minimum disk size is 80 GB.
        -   **Master Nodes**: The default number of master instances is one.
    -   Core Instance: Stores all cluster data. You can scale up the instance based on your needs.
        -   **System Disk Type**: Select Ultra Disk or SSD Disk based on your needs.
        -   **System Disk Size**: You can resize the disk based on your needs. The recommended minimum disk size is 80 GB.
        -   **Data Disk Type**: Select Ultra Disk or SSD Disk based on your needs.
        -   **Data Disk Size**: You can resize the disk based on your needs. The recommended minimum disk size is 80 GB.
        -   **Core Nodes**: The default number of core instances is two. You can adjust the number of core instances based on your needs.
    -   **Task Instance**: No data is stored on task instance groups. Task instance groups are used to adjust the computing capability of clusters. This feature is disabled by default. You can enable it based on your needs.

Step 3: Basic Settings

Description

-   **Basic Information**

    -   **Cluster Name**: Enter the name of the cluster. It must be 1-64 characters in length and can contain Chinese characters, uppercase English letters, lowercase English letters, numbers, hyphens \(-\), and underscores \(\_\).
    -   Logon Settings
        -   **Remote Logon**: Indicates whether to open port 22 of the security group. This feature is enabled by default.
        -   **Key Pair**:For the use of the key pair, please refer to [SSH Key Pair](../../../../reseller.en-US/Security/Key pairs/SSH key pair overview.md#)
        -   **Password**: Set the password used to log on to the master instance. The logon password must be 8 to 30 characters in length and can contain uppercase letters, lowercase letters, numbers , and special characters including exclamation marks \(!\), at signs \(@\), number signs \(\#\), dollar signs \($\), percent signs \(%\), ampersands \(&\), and asterisks \(\*\).
-   Advanced Settigs
    -   **Unified Metabases**: Hive uses a unified metadatabase, which is independent of the cluster. The meta information will not be deleted after the cluster is released. We recommend that you disable this feature if you are an E-MapReduce beginner.
    -   **Add Knox User**:This account is used to access the Web interface of open-source big data software.
    -   **Permission Settings**: Typically, the default setting is used.
    -   **Bootstrap Actions** \(optional\): You can set the cluster to run your custom script before it starts Hadoop. For more information, see Bootstrap action.

## Confirm the creation {#section_htk_gz4_y2b .section}

Confirm the configured items and fees in the configuration list. After you configure the settings and make sure that all settings are valid, the **Create** button is highlighted. Confirm the information, and click **Create** to create a cluster.

**Note:** 

-   The cluster is created immediately if the billing method is Pay-As-You-Go. You are the directed to the cluster list page. You can find an **initializing** cluster in the **Clusters** list. It may take several minutes to complete the cluster creation. After the cluster has been created, it changes its status to **Idle**.
-   An order is generated if the billing method is Subscription. The cluster will be created after you complete the payment.

## Creation failed {#section_jtk_gz4_y2b .section}

If the creation fails, **CREATE\_FAILED** is displayed on the clusters list. Move the pointer over the red exclamation mark \(!\) to view the cause.

You do not to handle clusters that are failed to be created because no computing resources have been created for these clusters. These clusters will be automatically hidden after three days.

