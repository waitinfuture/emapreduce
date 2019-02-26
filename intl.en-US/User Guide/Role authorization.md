# Role authorization {#concept_ykn_bd3_y2b .concept}

When you activate the E-MapReduce service, a default system role named AliyunEMRDefaultRole must be granted to the E-MapReduce service account. Once assigned, E-MapReduce can call the relevant services \(such as ECS and OSS\), create clusters, save logs, and perform other related tasks.

**Note:** 

If you are activating E-MapReduce for the first time, you must authorize roles by using the primary account. Otherwise, the primary and user accounts cannot access or use E-MapReduce.

## Role authorization process {#section_ktk_jh3_y2b .section}

1.  When you create a cluster or an on-demand execution plan, if a default role is not authorized to the E-MapReduce service account, the following prompt is displayed. Click **Go to RAM for authorization** to authorize the role.

    ![EMR role authorization](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17844/155114746010342_en-US.jpg)

2.  On the RAM authorization page, click **Confirm Authorization Policy** to authorize the default role AliyunEMRDefaultRole to the E-MapReduce service account.

    ![Confirm Authorization Policy](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17844/155114746010343_en-US.jpg)

3.  Refresh the E-MapReduce console, and then perform relevant operations. If you want to view relevant detailed policy information of AliyunE-MapReduceDefaultRole, log on to the RAM console, or click [View Link](https://partners-intl.console.aliyun.com/#/ram/AliyunEMRRolePolicy/info).

## Default role permissions {#section_vpj_th3_y2b .section}

The default role AliyunEMRDefaultRole has the following permissions

-   ECS related permissions:

    |Permission name \(Action\)|Description|
    |:-------------------------|:----------|
    |ecs:CreateInstance|Create ECS instances|
    |ecs:RenewInstance|Renew ECS instances.|
    |ecs:DescribeRegions|Query ECS region information.|
    |ecs:DescribeZones|Query zone information.|
    |ecs:DescribeImages|Query image information.|
    |ecs:CreateSecurityGroup|Create security groups.|
    |ecs:AllocatePublicIpAddress|Allocate a public network IP address.|
    |ecs:DeleteInstance|Delete machine instances.|
    |ecs:StartInstance|Start machine instances.|
    |ecs:StopInstance|Stop machine instances.|
    |ecs:DescribeInstances|Query machine instances.|
    |ecs:DescribeDisks|Query the machine's relevant disk information.|
    |ecs:AuthorizeSecurityGroup|Set security group input rules.|
    |ecs:AuthorizeSecurityGroupEgress|Set security group output rules.|
    |ecs:DescribeSecurityGroupAttribute|Query security group details.|
    |ecs:DescribeSecurityGroups|Query security group list information.|

-   OSS related permissions

    |Permission name \(Action\)|Description|
    |:-------------------------|:----------|
    |oss: PutObject|Upload file or folder objects.|
    |oss: GetObject|Get file or folder objects.|
    |oss: ListObjects|Query file list information.|


## Grant permissions to a RAM user account {#section_dhl_1fr_bgb .section}

To ensure that user accounts can access the E-MapReduce service, you need to log on with your primary account to the [RAM console](https://ram.console.aliyun.com/#/overview) and set AliyunEMRFullAccess or AliyunEMRDevelopAccess policies to grant user accounts access to E-MapReduce.

**Note:** 

-   The AliyunEMRFullAccess policy grants user accounts with full access permissions to E-MapReduce and E-MapReduce resources.
-   The AliyunEMRDevelopAccess policy grants user accounts the E-MapReduce Developer permission, but, unlike AliyunEMRFullAccess, it does not grant users with other access permissions, such as the permissions for creating or releasing E-MapReduce clusters.

    For information about RAM user accounts, policies, and roles, see [RAM](../../../../../reseller.en-US/API Reference/Introduction/RAM introduction.md#).


