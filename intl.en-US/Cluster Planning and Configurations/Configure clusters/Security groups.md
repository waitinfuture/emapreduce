# Security groups {#concept_ass_5yn_y2b .concept}

Currently, security groups created in the EMR console are required for creating an EMR cluster.

When creating an E-MapReduce cluster, you can open port 22 of the security group where the cluster resides \(It is closed by default, open in **Create Cluster** \> **Basic Configuration** \> **Remote Logon** \).We recommend that you add ECS instances to security groups based on their functions. For example, the security group for an EMR cluster is **EMR security group**. **user security group** is an existing security group that you have created. Instances of each security group are configured to open different ports based on the IP addresses allowed to access the instances.

## Add an EMR cluster to an existing security group {#section_g1h_bzn_y2b .section}

**Note:** 

-   You need to add instances in a classic network to a classic network security group of the same region.
-   You need to add instances in a VPC to a VPC security group of the same region.

1.  Log on to the Alibaba Cloud [E-MapReduce console](https://partners-intl.console.aliyun.com/#/emr).
2.  Click the Cluster Management tab.
3.  In the cluster list, locate the cluster that you want to add to the security group. Click **View Details** in the Actions column for the cluster to go to the Cluster Overview page.
4.  On the Cluster Overview page, click the **security group ID** in the **Network** area.
5.  In the left-side navigation pane, click **Instances in Security Group** to view the IDs and names of all instances.
6.  Go to the [Alibaba Cloud ECS console](https://partners-intl.console.aliyun.com/#/ecs). In the left-side navigation pane, click **Security Groups**.
7.  The ECS instances listed on the Instances page, whose names start with emr-xxx, are instances in an EMR cluster. Locate the instance that you want to add to a security group, click **Manage** in the **Actions** column for the instance.
8.  Click **Security Groups**.
9.  Click **Add to Security Group**.
10. Select the security group to which you want to add the instance. Click **Join multiple security groups**. From the Security Group drop-down list, select the security groups as needed. The security groups are displayed in the dotted box below.
11. Click **OK**.

## Add an existing cluster to an EMR security group {#section_rsc_wyn_y2b .section}

You can refer to the operations described in the Add an EMR cluster to an existing security group section. The full process starts with locating the security group of the existing cluster and ends with adding the instances to the EMR security group. For instances that do not belong to any security groups, select the instances and choose More \> Network and security \> Add to Security Group to add multiple instances to the EMR security group at a time.

## Security group rules {#section_ssc_wyn_y2b .section}

If an ECS instance belongs to multiple security groups, the instance follows the union of all security group rules. For example, the security group rule of the EMR security group is configured to open port 22 and the security group rules of the **user security group** are configured to open all ports. After you add the EMR cluster to the **user security group**, the instances of the EMR cluster open all ports. We recommend that you use security groups carefully.

