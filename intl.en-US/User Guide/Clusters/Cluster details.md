# Cluster details {#concept_sxp_jrn_y2b .concept}

Cluster details display detailed information about a cluster.

Detailed information is provided in the following areas: cluster, software, network, and host.

## Cluster {#section_zm4_lrn_y2b .section}

![Cluster](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17857/154770905710441_en-US.png)

-   **Name**: The name of a cluster.
-   **ID**: The instance ID of a cluster.
-   **Region**: The region where a cluster is located.
-   **Start Time**: The time at which a cluster is created.
-   **Software Configuration**: Software configurations.
-   **I/O Optimization**: Whether the I/O optimization setting is enabled.
-   **High Availability**: Whether high-availability clusters are enabled.
-   **Security Mode**: Software in clusters is started in Kerberos secure mode. For more information about Kerberos, see [Introduction to Kerberos](reseller.en-US/User Guide/Kerberos authentication/Introduction to Kerberos.md#).
-   **Billing Method**: Cluster billing method.
-   **Current Status**: For more information, see [Cluster statuses](../../../../../reseller.en-US/Trouble Shooting/Appendix/Status list.md#).
-   **Runtime**: The time from the point of creation to the current time.
-   **Bootstrap**: The names, paths, and parameters of all configured bootstrap actions are listed here.
-   **ECS Role**: When your program runs on an E-MapReduce compute node, you can access the related Alibaba Cloud services, such OSS, without an AccessKey. E-MapReduce automatically requests a temporary AccessKey to authorize this access. The permission control of this temporary AccessKey is controlled by this role.

## Software {#section_gn4_lrn_y2b .section}

![Software](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17857/154770905710443_en-US.jpg)

-   **Main Version**: The main version of E-MapReduce.
-   **Cluster Type**: The selected cluster type.
-   **Software**: All application programs installed are listed here with their versions, such as HDFS2.7.2, Hive 2.3.3, or Spark 2.3.1.

## Network {#section_jn4_lrn_y2b .section}

![Network](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17857/154770905710444_en-US.png)

-   **Region ID**: The region where a cluster is located, such as cn-hangzhou-b, which is the same as ECS.
-   **Network Type**: The network type of a cluster.
-   **Security Group ID**: The ID of the security group that a cluster joined.
-   **VPC/VSwitch**: The VPC and VSwitch IDs of a cluster.

## Host {#section_jfh_btw_pfb .section}

-   **Master Instance Group \(Master\)**: Configurations of all master nodes.

    ![Host](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17857/154770905714299_en-US.png)

    -   **Hosts**: The number of current nodes. During the creation process, the number of current nodes is less than the number of nodes you applied for until the creation is complete.
    -   **CPU**: The number of cores in a node's CPU.
    -   **Memory**: Memory capacity of a node.
    -   **Data Disk Type**: Data disk type and capacity of a node.
    -   **ECS ID**: The ID of the ECS instances purchased.

        ![Master Instance Group](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17857/154770905714297_en-US.png)

    -   **Status**: Includes Creating, Normal, Expanding, and Released.
    -   **Public IP**: The public IP of master nodes.
    -   **Intranet IP**: The internal network IP of the machine that can be accessed by all nodes in the cluster.
    -   **Created At**: The creation time of the ECS instance purchased.
-   **Core Instance Group \(Core\)**: Configurations of all core nodes.

    ![Core Instance Group](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17857/154770905714300_en-US.png)

    -   **Hosts**: The number of current nodes. This is the same as the number of the nodes you applied for.
    -   **CPU**: The number of cores in a node's CPU.
    -   **Memory**: Memory capacity of a node.
    -   **Data Disk Type**: Data disk type and capacity of a node.
    -   **ECS ID**: The ID of the ECS instances purchased.

        ![Core Instance Group List](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17857/154770905714298_en-US.png)

    -   **Status**: Includes Creating, Normal, Expanding, and Released.
    -   **Intranet IP**: The internal network IP of the machine that can be accessed by all nodes in the cluster.
    -   **Created At**: The creation time of the ECS instance purchased.

