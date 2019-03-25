# Cluster list and details {#concept_dgg_5pn_y2b .concept}

## Cluster list {#section_gfm_5sk_bhb .section}

The Cluster Management page, displays basic information about all of your clusters.

On the **Cluster Management** page, information of clusters are displayed as follows:

![Cluster list](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17856/155349798410433_en-US.jpg)

The items in the cluster list are as follows:

-   **Cluster ID/Name**: The ID and name of a cluster. Move your cursor over a cluster's name to modify it.
-   **Cluster Type**: Hadoop is the only cluster type available.
-   **Status**: The status of a cluster. For more information, see [Cluster statuses](../../../../../reseller.en-US/FAQ/Appendix/Status list.md#). If a cluster experiences an abnormality, such as a creation failure, prompt information appears on the right. If you hover your cursor over it, you can view detailed error information. You can also sort the statuses by clicking **Status**.
-   **Created At**: Time at which a cluster was created.
-   **Runtime**: The time from the point of creation to the current time. Once the cluster is released, the timing is terminated.
-   **Billing Method**: The billing method of the cluster.
-   **Actions**: Operations that can be performed on clusters, including the following:
    -   **Monitoring**: Monitors the CPU usage rate, memory capacity, and disk capacity of E-MapReduce clusters to help users monitor the running status of the cluster.
    -   **Manage**: Enters the **Clusters and Services** panel.
    -   **View details**: Enters the **Cluster Overview** panel and view detailed information after the cluster is created.
    -   **More**:
        -   **Scale Up/Out**: Expands the cluster.
        -   **Release**: Releases a cluster. For more information, see [Release a cluster](reseller.en-US/Cluster Planning and Configurations/Configure clusters/Release a cluster.md#).
        -   **Restart**: Restarts a cluster.

## Cluster details {#section_ntj_xsk_bhb .section}

Cluster details display detailed information about a cluster.

Detailed information is provided in the following areas: cluster, software, network, and host:

-   Cluster

    ![Cluster](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17857/155349798410441_en-US.png)

    -   **Name**: The name of a cluster.
    -   **ID**: The instance ID of a cluster.
    -   **Region**: The region where a cluster is located.
    -   **Start Time**: The time at which a cluster is created.
    -   **Software Configuration**: Software configurations.
    -   **I/O Optimization**: Whether the I/O optimization setting is enabled.
    -   **High Availability**: Whether high-availability clusters are enabled.
    -   **Security Mode**: Software in clusters is started in Kerberos secure mode. For more information about Kerberos, see [Introduction to Kerberos](reseller.en-US/Cluster Planning and Configurations/Kerberos authentication/Introduction to Kerberos.md#).
    -   **Billing Method**: Cluster billing method.
    -   **Current Status**: For more information, see [Cluster statuses](../../../../../reseller.en-US/FAQ/Appendix/Status list.md#).
    -   **Runtime**: The time from the point of creation to the current time.
    -   **Bootstrap**: The names, paths, and parameters of all configured bootstrap actions are listed here.
    -   **ECS Role**: When your program runs on an E-MapReduce compute node, you can access the related Alibaba Cloud services, such OSS, without an AccessKey. E-MapReduce automatically requests a temporary AccessKey to authorize this access. The permission control of this temporary AccessKey is controlled by this role.
-   Software

    ![Software](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17857/155349798410443_en-US.jpg)

    -   **Main Version**: The main version of E-MapReduce.
    -   **Cluster Type**: The selected cluster type.
    -   **Software**: All application programs installed are listed here with their versions, such as HDFS2.7.2, Hive 2.3.3, or Spark 2.3.1.
-   Network

    ![Network](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17857/155349798410444_en-US.png)

    -   **Region ID**: The region where a cluster is located, such as cn-hangzhou-b, which is the same as ECS.
    -   **Network Type**: The network type of a cluster.
    -   **Security Group ID**: The ID of the security group that a cluster joined.
    -   **VPC/VSwitch**: The VPC and VSwitch IDs of a cluster.
-   Host
    -   **Master Instance Group \(Master\)**: Configurations of all master nodes.

        ![Host](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17857/155349798414299_en-US.png)

        -   **Hosts**: The number of current nodes. During the creation process, the number of current nodes is less than the number of nodes you applied for until the creation is complete.
        -   **CPU**: The number of cores in a node's CPU.
        -   **Memory**: Memory capacity of a node.
        -   **Data Disk Type**: Data disk type and capacity of a node.
        -   **ECS ID**: The ID of the ECS instances purchased.

            ![Master Instance Group](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17857/155349798414297_en-US.png)

        -   **Status**: Includes Creating, Normal, Expanding, and Released.
        -   **Public IP**: The public IP of master nodes.
        -   **Intranet IP**: The internal network IP of the machine that can be accessed by all nodes in the cluster.
        -   **Created At**: The creation time of the ECS instance purchased.
    -   **Core Instance Group \(Core\)**: Configurations of all core nodes.

        ![Core Instance Group](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17857/155349798414300_en-US.png)

        -   **Hosts**: The number of current nodes. This is the same as the number of the nodes you applied for.
        -   **CPU**: The number of cores in a node's CPU.
        -   **Memory**: Memory capacity of a node.
        -   **Data Disk Type**: Data disk type and capacity of a node.
        -   **ECS ID**: The ID of the ECS instances purchased.

            ![Core Instance Group List](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17857/155349798414298_en-US.png)

        -   **Status**: Includes Creating, Normal, Expanding, and Released.
        -   **Intranet IP**: The internal network IP of the machine that can be accessed by all nodes in the cluster.
        -   **Created At**: The creation time of the ECS instance purchased.

