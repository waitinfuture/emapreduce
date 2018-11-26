# Cluster details {#concept_sxp_jrn_y2b .concept}

Cluster details display cluster detailed information.

Cluster overview includes the following four parts.

## Cluster {#section_zm4_lrn_y2b .section}

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17857/154261829310441_en-US.png)

-   **Name**: The name of a cluster.
-   **ID**: The instance ID of a cluster.
-   **Region**: The region where a cluster is located.
-   **Start Time**: The creation time of a cluster.
-   **Software Configuration**: Software configurations.
-   **I/O Optimization**: Whether the I/O optimization setting is enabled.
-   **High Availability**: Whether high-availability clusters are enabled.
-   **Security Mode**: Software in clusters is started in Kerberos secure mode. For information about Kerberos, see [Introduction to Kerberos](intl.en-US/User Guide/Kerberos authentication/Introduction to Kerberos.md#).
-   **Billing Method**: Cluster payment type。
-   **Current Status**: See [Cluster status](../../../../intl.en-US/.md#).
-   **Runtime**: Clusters run time.
-   **Bootstrap**: The names, paths, and parameters of all configured bootstrap actions are listed here.
-   **ECS Role**: When your program runs on an EMRcompute node, you can access the related Alibaba Cloud services, such OSS, without an AccessKey. EMR automatically requests a temporary AccessKey to authorize this access. The permission control of this temporary AccessKey is controlled by this role.

## Software {#section_gn4_lrn_y2b .section}

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17857/154261829310443_en-US.jpg)

-   **Main Version**: The main version of E-MapReduce.
-   **Cluster Type**: The selected cluster type.
-   **Software**: All application programs installed and their versions are listed here, such as HDFS2.7.2, Hive 2.3.3, and spark 2.3.1.

## Network {#section_jn4_lrn_y2b .section}

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17857/154261829310444_en-US.png)

-   **Region ID**: The region where a cluster is located, such as cn-hangzhou-b which is the same as ECS.
-   **Network Type**: The network type of a cluster.
-   **Security Group ID**: The security group ID that a cluster joined.
-   **VPC/VSwitch**: The VPC and VSwitch IDs of a cluster.

## Host {#section_jfh_btw_pfb .section}

-   **Master Instance Group \(Master\)**: Configurations of all master nodes.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17857/154261829314299_en-US.png)

    -   **Hosts**: The number of the current nodes. During the creation process, the number of the current nodes is less than that of nodes you applied for until the creation is complete.
    -   **CPU**: The number of cores of a node's CPU.
    -   **Memory**: Memory capacity of a node.
    -   **Data Disk Type**: Data disk type and capacity of a node.
    -   **ECS ID**: The ID of the ECS instances purchased.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17857/154261829314297_en-US.png)

    -   **Status**: Includes Creating, Normal, Expanding, and Released.
    -   **Public IP**: The public IP of master nodes.
    -   **Intranet IP**: The internal network IP of the machine that can be accessed by all nodes in the cluster.
    -   **Created At**: The creation time of the ECS instance purchased.
-   **Core Instance Group \(Core\)**: Configurations of all core nodes.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17857/154261829414300_en-US.png)

    -   **Hosts**: The number of the current nodes which is the same as that of the nodes you applied for.
    -   **CPU**: The number of cores of a node's CPU.
    -   **Memory**: Memory capacity of a node.
    -   **Data Disk Type**: Data disk type and capacity of a node.
    -   **ECS ID**: The ID of the ECS instances purchased.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17857/154261829414298_en-US.png)

    -   **Status**: Includes Creating, Normal, Expanding, and Released.
    -   **Intranet IP**: The internal network IP of the machine that can be accessed by all nodes in the cluster.
    -   **Created At**: The creation time of the ECS instance purchased.

