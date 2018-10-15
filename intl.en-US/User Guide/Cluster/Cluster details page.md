# Cluster details page {#concept_sxp_jrn_y2b .concept}

Cluster details page displays the cluster details,

including the following four parts.

## Cluster information {#section_zm4_lrn_y2b .section}

-   **ID/name**: ID and name of the cluster instances.

-   **Billing method**: The billing method of the cluster.

-   **Region**: The region where the cluster is located.

-   **Current status**: The current status of the cluster, for more information, see [Cluster status](https://help.aliyun.com/document_detail/28191.html).

-   **Start time**: The creation time of the cluster.

-   **Running time**: The running time of the cluster.

-   **Log activation**: Determines whether the log saving function is activated.

-   **Log position**: If the log saving function is activated, the position where the log is saved is displayed here.

-   **Software configuration**: Information about software configuration.

-   **Bootstrap/software configuration**: Whether abnormalities occur.

-   **High availability cluster**: Whether the high availability cluster is activated.


## Bootstrap action {#section_en4_lrn_y2b .section}

The names, paths, and parameters of all configured bootstrap actions are listed here.

## Software information {#section_gn4_lrn_y2b .section}

-   **Main version**: Use the main version of E-MapReduce.

-   **Cluster type**: The selected cluster type.

-   **Software information**: All application programs installed by the user and their versions are listed here, such as Hadoop 2.6.0 and Hive 0.14.


## Hardware information {#section_jn4_lrn_y2b .section}

-   **Network type**: Network type used by the cluster.

-   **Main security group**: The security group the cluster belongs to.

-   **Main availability zone**: The availability zone where the cluster is located, for example, cn-hangzhou-b, consistent with ECS.

-   **VPC/subnet**: The VPC where the user cluster is located and the ID of subnet VSwtich.

-   **Master node information**: Configurations corresponding to all master nodes, including the following content.

    -   **Quantity of nodes**: Quantity of current nodes and the actual number of applied nodes. Theoretically, the two values are the same. However, during creation, the quantity of current nodes is less than applied nodes until the creation is complete.

    -   **CPU**: The quantity of CPU cores at a single node.

    -   **Memory**: The memory volume of a single node.

    -   **Data disk type**: The type of data disk.

    -   **Data disk**: The data disk volume of a single node.

    -   **Instance status**: Including creating, normal, expanding, and released.

    -   **Public network IP**: The public network IP address of master node.

    -   **Intranet IP**: The intranet IP address of a machine which can be accessed by all nodes in the cluster.

    -   **ECS expiration time**: The expiration date for use of a purchased ECS.

    -   **E-MapReduce expiration time**: The expiration date for use of a purchased E-MapReduce.

-   **Core node information**: Configurations corresponding to all core nodes, including the following content.

    -   **Quantity of nodes**: Quantity of current nodes and the actual number of applied nodes.

    -   **CPU**: The quantity of CPU cores of a single node.

    -   **Memory**: The memory volume of a single node.

    -   **Data disk type**: The type of data disk.

    -   **Data disk**: The hard disk volume of a single node.

    -   **Instance status**: Including creating, normal, and expanding.

    -   **Intranet IP**: The intranet IP address of a machine which can be accessed by all nodes in the cluster.

    -   **ECS expiration time**: The expiration date for use of a purchased ECS.

    -   **E-MapReduce expiration time**: The expiration date for use of a purchased E-MapReduce.


