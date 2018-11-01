# Security group {#concept_ass_5yn_y2b .concept}

Security groups created in E-MapReduce can be used during the creation of clusters.

Only the port 22 is accessible in the cluster created by E-MapReduce. We recommend that you divide ECS instances by function, and put them into different user security groups. For example, the security group of E-MapReduce is **E-MapReduce security group**, while the security group that you have created is **User security group**. Each security group is provided with unique access control as required.

If it is necessary to link with the cluster that has been created, follow these steps.

## Add E-MapReduce cluster to the existing security group {#section_g1h_bzn_y2b .section}

1.  Log on to the[Alibaba Cloud E-MapReduce console](https://emr.console.aliyun.com/).
2.  At the top of the page, click **Cluster Management**.
3.  Click **View Details**.
4.  In the **Network** tab, find **Security Group ID** and click the ID link.
5.  In the left-side menu, click **Instances in Security Group** to view security group names of all ECS instances.
6.  Log on to the [Alibaba Cloud ECS console](https://ecs.console.aliyun.com/#/home), and in the left-side navigation panel, click **Security Group** to find the security group entry in the list as viewed in the preceding step.
7.  Click **Manage Instances** in a security group, and see ECS instances names starting with emr-xxx. These are the corresponding ECS instances in an E-MapReduce cluster.
8.  Select all these instances, click **Move to security group**, and then select a security group to move an E-MapReduce cluster to an existing group.

## Add the existing cluster into E-MapReduce security group {#section_rsc_wyn_y2b .section}

Find the security group where the existing cluster is located. Repeat the preceding operations, and move to the E-MapReduce security group. Select scattered machines in the ECS console directly and move the clusters to E-MapReduce security group in batch.

## Security group rules {#section_ssc_wyn_y2b .section}

The security group rules are subject to the OR relationship when an ECS instance is in several different security groups. For example, only port 22 of the E-MapReduce security group is accessible, while all ports of the **User security group** are accessible. After the cluster of E-MapReduce is added into the **User security group**, all ports of the machine in E-MapReduce are open.

