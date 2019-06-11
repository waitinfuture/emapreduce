# Configure a network connection for using Sqoop to transfer data from a database to an EMR cluster {#concept_405382 .concept}

When you need to transfer data from external databases to your EMR cluster, make sure that the networks are connected. This topic describes how to configure network connections for accessing ApsaraDB for RDS instances, user-created databases hosted on ECS, and on-premises databases.

## ApsaraDB for RDS {#section_0a1_hp6_yzw .section}

-   Classic network

    We recommend that you use an EMR cluster deployed in the classic network to access a classic network RDS instance. You can set internal and public IP addresses for classic network RDS instances. Sqoop synchronizes data by running map tasks on the master node and worker nodes. However, only the master node of a classic network EMR cluster can access the public network. You need to access the internal IP address of the RDS instance for Sqoop. Make sure that the internal IP address of the EMR cluster is in the whitelist of the RDS instance.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/328059/156022366848260_en-US.png)

    For more information about how to create a classic network EMR cluster, see [Create a cluster](../../../../reseller.en-US/Cluster Planning and Configurations/Configure clusters/Create a cluster.md#).

-   VPC

    If the RDS is in a VPC network, you must specify a VPC network for the EMR cluster. We recommend that you use the same VPC for your EMR cluster and the RDS instance to save time for configuring a network connection. Otherwise, use [Express Connect](https://help.aliyun.com/product/27782.html) to configure a network connection.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/328059/156022366848269_en-US.png)


## User-created database hosted on ECS {#section_5lw_w9o_j3p .section}

-   Classic network

    The process for accessing a classic network user-created database and a classic network RDS instance is similar. Use the classic network for the EMR cluster to access the internal IP address of the user-created database. Make sure that the ECS instance on which the database is deployed and the instances of the EMR cluster are in the same security group. In the ECS console, choose **Security Groups** \> **Manage Instances** \> **Add Instance**.

-   VPC

    The process for accessing a VPC user-created database and a VPC RDS instance is similar. Use a VPC for your EMR cluster. Make sure that the ECS instance where the database is deployed and the EMR cluster are in the same security group.


## On-premises database {#section_mmu_6q6_m8j .section}

You can assign an EIP address to your EMR cluster and access the public IP address of the database. Or, you can use Express Connect to connect the VPCs to access the database.

-   Associate an EIP

    We recommend that you use a VPC EMR cluster if the on-premises database can be accessed over the public network. Create an EMR cluster in a VPC. Then, in the ECS console, choose **Manage** \> **Configuration Information** \> **More** \> **Bind EIP** and associate an EIP with each ECS instance. After the configurations, your cluster can access the public IP address of the on-premises database.

-   Express Connect

    If the on-premises database is not allowed to be accessed over the public network, create an EMR cluster in a VPC and use Express Connect to connect the on-premises IDC and the VPC. For more information about Express Connect, see [Express Connect](https://help.aliyun.com/product/27782.html).


