# Security group {#concept_ass_5yn_y2b .concept}

The security group created in E-MapReduce is used during the cluster creation.

Only port 22 is opened in the cluster created by E-MapReduce. We recommend that you divide the ECS instances by function, and put them into different user security groups. For example, the security group of E-MapReduce is **E-MapReduce security group**, while the security group that you have created is **User security group**. Each security group is provided with unique access control as required.

If it is necessary to link with the cluster that has been created, follow these steps.

## Add E-MapReduce cluster to existing security group {#section_g1h_bzn_y2b .section}

1.  Log on to [Alibaba Cloud E-MapReduce Console Cluster List](https://emr.console.aliyun.com/).
2.  Specify the cluster entry to be added to the security group. Click View details to enter the cluster details page.
3.  Specify the security group under Network information on the cluster details page. View the name and ID of the security group where all ECS instances are located.
4.  Enter [Alibaba Cloud ECS console](https://ecs.console.aliyun.com/#/home). Click **Security group** on the left side to find the security group entry in the list as viewed in previous step.
5.  Click **Manage instances** in the security group, and see ECS instances names starting with emr-xxx. These are corresponding ECS instances in the E-MapReduce cluster.
6.  Select all of these instances, and click Move to security group. Then select the new security group.

## Add the existing cluster into E-MapReduce security group {#section_rsc_wyn_y2b .section}

Find the security group where the existing cluster is located. Repeat the preceding operations, and move to the E-MapReduce security group. Select the scattered machines in the ECS console directly and move the clusters to E-MapReduce security group in batch.

## Security group rules {#section_ssc_wyn_y2b .section}

The security group rules are subject to the OR relationship when an ECS instance is in several different security groups. For example, only port 22 of the E-MapReduce security group is opened, while all ports of **User security group** are opened. After the cluster of E-MapReduce is added into **User security group**, all ports of the machine in E-MapReduce are opened. Therefore, pay attention when performing this process.

