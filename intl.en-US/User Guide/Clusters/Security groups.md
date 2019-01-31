# Security groups {#concept_ass_5yn_y2b .concept}

Security groups created in E-MapReduce can be used during the creation of clusters.

Only port 22 is accessible in clusters created by E-MapReduce. We recommend that you divide ECS instances by function, and put them into different user security groups. For example, name the security group of E-MapReduce **E-MapReduce security group**, and name the security group that you create **User security group**. Each security group is provided with unique access control as required.

If you need to link a security group with a cluster that has already been created, follow these steps.

## Add an E-MapReduce cluster to an existing security group {#section_g1h_bzn_y2b .section}

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://emr.console.aliyun.com/).
2.  At the top of the page, click **Cluster Management**.
3.  Click **View Details**.
4.  In the **Network** tab, find **Security Group ID** and click the ID link.
5.  In the menu on the left, click **Instances in Security Group** to view the security group names of all ECS instances.
6.  Log on to the [Alibaba Cloud ECS console](https://ecs.console.aliyun.com/#/home) and click **Security Group** in the navigation panel on the left to find the security group as viewed in the preceding step.
7.  Click **Manage Instances** in a security group and view ECS instance names that start with emr-xxx. These are the ECS instances in an E-MapReduce cluster.
8.  Select all of these instances, click **Move to security group**, and then select a security group to move the E-MapReduce cluster to.

## Add an existing cluster to an E-MapReduce security group {#section_rsc_wyn_y2b .section}

1.  Find the security group where the existing cluster is located.
2.  Repeat the preceding operations, and move to the E-MapReduce security group.
3.  Select scattered machines in the ECS console directly and move the clusters to the **E-MapReduce security group** in batches.

## Security group rules {#section_ssc_wyn_y2b .section}

When an ECS instance is in several different security groups, the security group rules are subject to the OR relationship. For example, only port 22 of the E-MapReduce security group is accessible, whereas all ports of the **User security group** are accessible. After the E-MapReduce cluster is added to the **User security group**, all ports of the machine in E-MapReduce are open.

