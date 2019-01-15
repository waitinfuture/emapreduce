# Role authorization {#concept_ykn_bd3_y2b .concept}

When a user activates the E-MapReduce service, a default system role named AliyunEMRDefaultRole must be granted to the E-MapReduce service account. Once assigned, E-MapReduce can call the relevant services \(such as ECS and OSS\), create clusters, save logs, and perform other related tasks.

## Role authorization process {#section_ktk_jh3_y2b .section}

1.  When you create a cluster or an on-demand execution plan, if a default role is not authorized to the E-MapReduce service account, the following prompt is displayed. Click **Go to RAM for authorization** to authorize the role.

    ![RAM authorization](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17844/154754567410342_en-US.jpg)

2.  On the RAM authorization page, click **Confirm Authorization Policy** to authorize the default role AliyunEMRDefaultRole to the E-MapReduce service account.

    ![Confirm Authorization Policy](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17844/154754567410343_en-US.jpg)

3.  Refresh the E-MapReduce console and perform the relevant operations. If you want to view detailed policy information about AliyunEMRDefaultRole, log on to the RAM console, or click [View Link](https://ram.console.aliyun.com/?spm=a2c4g.11186623.2.6.77bd72fe3PD5pf#/policy/detail/system/AliyunEMRRolePolicy/info).

## Default role permissions {#section_vpj_th3_y2b .section}

The default role AliyunEMRDefaultRole has the following permissions:

-   ECS-related permissions:

    |Permission Name \(Action\)|Description|
    |:-------------------------|:----------|
    |ecs: CreateInstance|Create ECS instances.|
    |ecs: RenewInstance|Renew ECS instances.|
    |ecs: DescribeRegions|Query ECS region information.|
    |ecs: DescribeZones|Query zone information.|
    |ecs: DescribeImages|Query image information.|
    |ecs: CreateSecurityGroup|Create security groups.|
    |ecs: AllocatePublicIpAddress|Allocate a public network IP address.|
    |ecs: DeleteInstance|Delete machine instances.|
    |ecs:StartInstance|Start machine instances.|
    |ecs: StopInstance|Stop machine instances.|
    |ecs: DescribeInstances|Query machine instances.|
    |ecs: DescribeDisks|Query the machine's relevant disk information.|
    |ecs: AuthorizeSecurityGroup|Set security group input rules.|
    |ecs: AuthorizeSecurityGroupEgress|Set security group output rules.|
    |ecs: DescribeSecurityGroupAttribute|Query security group details.|
    |ecs: DescribeSecurityGroups|Query security group list information.|

-   OSS-related permissions

    |Permission Name \(Action\)|Description|
    |:-------------------------|:----------|
    |oss: PutObject|Upload file or folder objects.|
    |oss: GetObject|Get file or folder objects.|
    |oss: ListObjects|Query file list information.|


## Grant the PassRole permission to a sub-account {#section_dhl_1fr_bgb .section}

To ensure that sub-accounts can use the E-MapReduce feature, in addition to granting the E-MapReduce access permission, you must grant the PassRole permission to sub-accounts.

If the primary account user does not grant the PassRole permission to a sub-account, the sub-account user is prompted by the following message after logging on to the E-MapReduce console:![prompted message](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17844/154754567433938_en-US.png)

The following example shows how to grant the PassRole permission:

```
{
    "Version": "1",
    "Statement": [
        {
            "Action": [
                "oss:PutObject",
                "oss:GetObject",
                "oss:ListObjects"
            ],
            "Resource": "*",
            "Effect": "Allow"
        },
        {
            "Action": [
                "ecs:CreateInstance",
                "ecs:RunInstances",
                "ecs:RenewInstance",
                "ecs:DescribeRegions",
                "ecs:DescribeZones",
                "ecs:DescribeImages",
                "ecs:CreateSecurityGroup",
                "ecs:AllocatePublicIpAddress",
                "ecs:DeleteInstance",
                "ecs:StartInstance",
                "ecs:StopInstance",
                "ecs:DescribeInstances",
                "ecs:DescribeDisks",
                "ecs:AuthorizeSecurityGroup",
                "ecs:RevokeSecurityGroup",
                "ecs:AuthorizeSecurityGroupEgress",
                "ecs:DescribeSecurityGroupAttribute",
                "ecs:DescribeSecurityGroups",
                "ecs:DescribeInstanceHistoryEvents",
                "ecs:DescribeInstancesFullStatus",
                "ecs:DescribeDisksFullStatus",
                "ecs:ModifyInstanceChargeType",
                "ecs:ModifyPrepayInstanceSpec",
                "ecs:DescribeResourcesModification",
                "ecs:DescribeAvailableResource",
                "ecs:DescribeBandwidthLimitation",
                "ecs:CreateNetworkInterface",
                "ecs:DeleteNetworkInterface",
                "ecs:DescribeNetworkInterfaces",
                "ecs:CreateNetworkInterfacePermission",
                "ecs:DescribeNetworkInterfacePermissions",
                "ecs:DeleteNetworkInterfacePermission",
                "ecs:DescribeKeyPairs",
                "ecs:RebootInstance"
            ],
            "Resource": "*",
            "Effect": "Allow"
        },
        {
            "Action": [
                "vpc:DescribeVSwitches",
                "vpc:DescribeVpcs",
                "vpc:AllocateEipAddress",
                "vpc:AssociateEipAddress",
                "vpc:UnassociateEipAddress",
                "vpc:ReleaseEipAddress"
            ],
            "Resource": "*",
            "Effect": "Allow"
        },
        {
            "Action": [
                "cms:CreateAlarm",
                "cms:DeleteAlarm",
                "cms:QueryAlarm",
                "cms:QueryAlarmHistory",
                "cms:QueryMetricList",
                "cms:CreateAlert",
                "cms:CreateDimensions",
                "cms:DeleteAlert",
                "cms:QueryAlert",
                "cms:QueryNotifyHistory"
            ],
            "Resource": "*",
            "Effect": "Allow"
        },
        {
            "Action": [
                "ess:CreateScalingGroup",
                "ess:ModifyScalingGroup",
                "ess:EnableScalingGroup",
                "ess:DisableScalingGroup",
                "ess:DeleteScalingGroup",
                "ess:DescribeScalingGroups",
                "ess:DescribeScalingInstances",
                "ess:DescribeScalingActivities",
                "ess:CreateScalingConfiguration",
                "ess:DescribeScalingConfigurations",
                "ess:DeleteScalingConfiguration",
                "ess:CreateScalingRule",
                "ess:ModifyScalingRule",
                "ess:DescribeScalingRules",
                "ess:DeleteScalingRule",
                "ess:CreateScheduledTask",
                "ess:ModifyScheduledTask",
                "ess:DescribeScheduledTasks",
                "ess:DeleteScheduledTask",
                "ess:EnableScheduledTask",
                "ess:DisableScheduledTask",
                "ess:RemoveInstances",
                "ess:CreateLifecycleHook",
                "ess:DescribeLifecycleHooks",
                "ess:ModifyLifecycleHook",
                "ess:DeleteLifecycleHook",
                "ess:CompleteLifecycleAction",
                "ess:RecordLifecycleActionHeartbeat",
                "ess:CreateNotificationConfiguration",
                "ess:DescribeNotificationConfigurations",
                "ess:VerifyAuthentication",
                "ess:DescribeRegions",
                "ess:SetInstancesProtection",
                "ecs:ResizeDisk",
                "ess:ExecuteScalingRule",
                "ess:DetachInstances"
            ],
            "Resource": "*",
            "Effect": "Allow"
        }
    ]
}
```

After a sub-account has a permission to access PassRole, the sub-account has access to the E-MapReduce console.

