# Role authorization {#concept_ykn_bd3_y2b .concept}

When a user activates the E-MapReduce service, a system default role named AliyunEMRDefaultRole must be granted to the E-MapReduce service account. If the role is assigned correctly, E-MapReduce can then properly call relevant services \(such as ECS and OSS\), create the clusters, save the logs, and perform other related tasks.

## Role authorization process {#section_ktk_jh3_y2b .section}

1.  When you create a cluster or an on-demand execution plan, if no default role is authorized correctly to the E-MapReduce service account, the prompt is displayed. Click **Go to RAM for authorization** to perform role authorization.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17844/153959026310342_en-US.jpg)

2.  You are directed to RAM’s authorization page. Click **Confirm Authorization Policy** to authorize the default role AliyunE-MapReduceDefaultRole to E-MapReduce service account.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17844/153959026310343_en-US.jpg)

3.  Refresh the E-MapReduce console, and then perform relevant operations. If you want to view relevant detailed policy information of AliyunE-MapReduceDefaultRole, you can log on to the RAM console, or click [View Link](https://ram.console.aliyun.com/?spm=a2c4g.11186623.2.6.77bd72fe3PD5pf#/policy/detail/system/AliyunEMRRolePolicy/info).

## Default role permissions {#section_vpj_th3_y2b .section}

The permissions of default role, AliyunEMRDefaultRole, include the following:

-   ECS related permissions:

    |Permission name \(Action\)|Permission description|
    |:-------------------------|:---------------------|
    |ecs:CreateInstance|Create ECS instances.|
    |ecs:RenewInstance|Renew ECS instances.|
    |ecs:DescribeRegions|Query ECS region information.|
    |ecs:DescribeZones|Query Zone information.|
    |ecs:DescribeImages|Query image information.|
    |ecs:CreateSecurityGroup|Create security groups.|
    |ecs:AllocatePublicIpAddress|Allocate public network IP address.|
    |ecs:DeleteInstance|Delete machine instances.|
    |ecs:StartInstance|Start machine instances.|
    |ecs:StopInstance|Stop machine instances.|
    |ecs:DescribeInstances|Query machine instances.|
    |ecs:DescribeDisks|Query relevant disk information of the machine.|
    |ecs:AuthorizeSecurityGroup|Set security group input rules.|
    |ecs:AuthorizeSecurityGroupEgress|Set security group output rules.|
    |ecs:DescribeSecurityGroupAttribute|Query the security group details.|
    |ecs:DescribeSecurityGroups|Query security group list information.|

-   OSS related permissions

    |Permission name \(Action\)|Permission description|
    |:-------------------------|:---------------------|
    |oss: PutObject|Upload file or folder objects.|
    |oss: GetObject|Get file or folder objects.|
    |oss: ListObjects|Query file list information.|


