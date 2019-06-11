# CreateClusterV2 {#concept_ahy_wy1_kfb .concept}

You can call this operation to create an EMR cluster.

## Request parameters {#section_wbp_dz1_kfb .section}

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|ClusterType|String|Yes|HADOOP|The type of the cluster.|
|EmrVer|String|Yes|EMR-3.15.0|The version of EMR.|
|Name|String|Yes|bi\_hadoop|The name of the cluster. The name can be 1 to 64 characters in length and only contain Chinese characters, letters, numbers, hyphens \(-\), and underscores \(\_\).|
|RegionId|String|Yes|cn-hangzhou|The region ID. Currently, the China \(Hangzhou\), China \(Shanghai\), China \(Shenzhen\), China \(Beijing\), China \(Zhangjiakou-Beijing Winter Olympics\), US \(Silicon Valley\), Singapore, and Germany \(Frankfurt\) regions are supported.|
|ZoneId|String|Yes|cn-hangzhou-e|The zone ID. For the China \(Hangzhou\) region, the cn-hangzhou-b zone is supported. For the China \(Beijing\) region, the cn-beijing-a, cn-beijing-b, and cn-beijing-c zones are supported.|
|AccessKeyId|String|Yes|LTAI8ljWyu7y\*\*\*\*|The Accesskey ID.|
|AuthorizeContent|String|No|0|Not required.|
|AutoRenew|Boolean|No|false|Indicates whether the subscription cluster is auto-renewed.|
|BootstrapAction.N.Arg|String|No|--a=b|The argument that you pass into the bootstrap action.|
|BootstrapAction.N.Name|String|No|name|The name of the bootstrap action.|
|BootstrapAction.N.Path|String|No|oss://bucket/path|The path where the bootstrap action script is stored.|
|Config.N.ConfigKey|String|No|fs.trash.interval|The key of the custom configuration item.|
|Config.N.ConfigValue|String|No|60|The value of the custom configuration item.|
|Config.N.Encrypt|String|No|0|A reserved parameter.|
|Config.N.FileName|String|No|yarn-site|The name of the file that contains the configuration item.|
|Config.N.Replace|String|No|0|A reserved parameter.|
|Config.N.ServiceName|String|No|YARN|The name \(capitalized\) of the service that is configured by using the custom configuration item.|
|DepositType|String|No|HALF\_MANAGED|The hosting type.|
|EasEnable|Boolean|No|false|Indicates whether the cluster is a high-security cluster.|
|HighAvailabilityEnable|Boolean|No|false|Indicates whether the cluster is a high-availability cluster. A value of true indicates that two master nodes are required.|
|HostGroup.N.AutoRenew|Boolean|No|false|Indicates whether the instance group is auto-renewed.|
|HostGroup.N.ChargeType|String|Yes|PostPaid|The billing method for the instance group.|
|HostGroup.N.ClusterId|String|No|0|A reserved parameter. Not required.|
|HostGroup.N.Comment|String|No|0|A reserved parameter. Not required.|
|HostGroup.N.CreateType|String|No|0|A reserved parameter. Not required.|
|HostGroup.N.DiskCapacity|Integer|Yes|80|The data disk capacity of the instance group.|
|HostGroup.N.DiskCount|Integer|Yes|4|The data disk number of the instance group.|
|HostGroup.N.DiskType|String|Yes|CLOUD\_SSD|The data disk type of the instance group.|
|HostGroup.N.GpuDriver|String|No|cuda9|The GPU driver.|
|HostGroup.N.HostGroupId|String|No|0|A reserved parameter.|
|HostGroup.N.HostGroupName|String|Yes|Master Instance Group|The name of the instance group.|
|HostGroup.N.HostGroupType|String|Yes|MASTER|The type of the instance group. Valid values: MASTER, CORE, and TASK. Currently, you can only create a maximum of one master instance group and core instance group.|
|HostGroup.N.InstanceType|String|Yes|ecs.mn4.2xlarge|The instance type of the instance group.|
|HostGroup.N.NodeCount|Integer|Yes|1|The number of nodes in the node group.|
|HostGroup.N.Period|Integer|No|1|The length of the subscription. Unit: months. Valid values: 1, 2, 3, 4, 5, 6, 7, 8, 9, 12, 24, and 36. A value is required when HostGroup.n.ChargeType=PrePaid.|
|HostGroup.N.SysDiskCapacity|Integer|Yes|80|The system disk capacity of the instance group.|
|HostGroup.N.SysDiskType|String|Yes|CLOUD\_SSD|The system disk type of the instance group.|
|HostGroup.N.VSwitchId|String|No|vsw-bp10tvjyc77psy0z5\*\*\*\*|The ID of the VSwitch. A value is required when NetType=vpc.|
|InitCustomHiveMetaDB|Boolean|No|false|A reserved parameter. Not required.|
|InstanceGeneration|String|No|ecs-3|The generation of the ECS instances.|
|IoOptimized|Boolean|No|true|Indicates wether I/O optimization is enabled. Default value: true.|
|IsOpenPublicIp|Boolean|No|true|Indicates whether a public IP address is assigned. A value of true indicates that a bandwidth of 8 MB is set by default.|
|KeyPairName|String|No|test\_pair|The name of the key pair.|
|LogPath|String|No|oss//bucketname/path|The log path in OSS.|
|MachineType|String|No|ECS|The type of the machine.|
|OptionSoftWareList.N|RepeatList|No|\["ZOOKEEPER","LIVY"\]|The list of optional services.|
|SecurityGroupId|String|No|sg-bp1id7ajv83kmqwq\*\*\*\*|The ID of the security group. You can create a security group in the ECS console and use it. Note: If you use an existing security group, the default security group policy is applied to this security group: Only port 22 is open at the inbound and all ports are open at the outbound. You need to specify either SecurityGroupId or SecurityGroupName.|
|SecurityGroupName|String|No|emr-sg|The name of the security group to create. If the ID of the security group is not specified, this name is used to create a new security group. After the cluster is created, you can view the ID of the security group on the Cluster Management page. The default security group policy is applied to this security group: Only port 22 is open at the inbound and all ports are open at the outbound. You need to specify either SecurityGroupId or SecurityGroupName.|
|SshEnable|Boolean|No|false|Indicates whether SSH is enabled.|
|UseCustomHiveMetaDB|Boolean|No|false|A reserved parameter. Not required.|
|UseLocalMetaDb|Boolean|No|true|Indicates whether the local Hive metadatabase is used.|
|UserDefinedEmrEcsRole|String|No|AliyunEmrEcsDefaultRole|The role that is assigned to EMR for calling ECS resources.|
|UserInfo.N.Password|String|No|pwd|The password of the cluster.|
|UserInfo.N.UserId|String|No|12345|The ID of the Alibaba Cloud account for Knox.|
|UserInfo.N.UserName|String|No|tom|The username for Knox.|
|MasterPwd|String|No|pwd|The SSH password for the master node. The password must meet the following requirements. Length constraints: Minimum length of 8 characters. Maximum length of 30 characters. It must contain three types of characters \(uppercase letters, lowercase letters, numbers, and special symbols\).|
|ChargeType|String|Yes|PostPaid|The billing method. Valid values: PostPaid and PrePaid. PostPaid: pay-as-you-go. PrePaid: subscription.|
|Period|Integer|No|1|The length of the subscription. Unit: months. Valid values: 1, 2, 3, 4, 5, 6, 7, 8, 9, 12, 24, and 36. A value is required when ChargeType=PrePaid.|
|RelatedClusterId|String|No|C-D7958B72E59B\*\*\*\*|The ID of the primary cluster \(when the cluster that you create is a Gateway cluster\).|
|Configurations|String|No|0|Not required.|
|VpcId|String|No|vpc-bp1l4urd87xlh7i4b\*\*\*\*|The ID of the VPC. A value is required when NetType=vpc.|
|VSwitchId|String|No|vsw-bp10tvjyc77psy0z5\*\*\*\*|The ID of the Vswitch. A value is required when NetType=vpc.|
|WhiteListType|String|No|""|Not required.|
|NetType|String|Yes|vpc|The type of the network.|

## Response parameters {#section_xxl_hz1_kfb .section}

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|BF4FBAC6-B03E-4BFB-B6DB-EB53C34F2E22|The ID of the request.|
|ClusterId|String|C-D7958B72E59BAB88|The ID of the cluster.|

## Examples {#section_yr3_jz1_kfb .section}

-   Sample requests

    ```
    /? ClusterType=HADOOP
    &EmrVer=EMR-3.15.0
    &Name=bi_hadoop
    &RegionId=cn-hangzhou
    &AccessKeyId=LTAI8ljWyu7y****
    &AutoRenew=false
    &ChargeType=PostPaid
    &DepositType=HALF_MANAGED
    &HighAvailabilityEnable=true
    &InstanceGeneration=ecs-3
    &IoOptimized=true
    &IsOpenPublicIp=true
    &KeyPairName=
    &LogPath=oss//bucketname/path
    &MachineType=ECS
    &MasterPwd=pwd
    &NetType=vpc
    &OptionSoftWareList. 1=["ZOOKEEPER","LIVY"]
    &Period=30
    &SecurityGroupId=sg-bp1id7ajv83kmqwq****
    &SecurityGroupName=emr-sg 
    &SshEnable=
    &UseLocalMetaDb=
    &UserDefinedEmrEcsRole=AliyunEmrEcsDefaultRole
    &VpcId=vpc-bp1l4urd87xlh7i4b****
    &VSwitchId=vsw-bp10tvjyc77psy0z5****
    &ZoneId=cn-hangzhou-e
    &BootstrapAction. 1.Arg=--a=b
    &BootstrapAction. 1.1ame=name
    &BootstrapAction. 1.Path=oss://bucket/path
    &Config. 1.ConfigKey=
    &Config. 1.ConfigValue=
    &Config. 1.Encrypt=
    &Config. 1.FileName=
    &Config. 1.Replace=
    &Config. 1.ServiceName=
    &HostGroup. 1.AutoRenew=false
    &HostGroup. 1.ChargeType=PostPaid
    &HostGroup. 1.ClusterId=
    &HostGroup. 1.Comment=
    &HostGroup. 1.CreateType=
    &HostGroup. 1.DiskCapacity=80
    &HostGroup. 1.DiskCount=4
    &HostGroup. 1.DiskType=CLOUD_SSD
    &HostGroup. 1.GpuDriver=
    &HostGroup. 1.HostGroupId=
    &HostGroup. 1.HostGroupName=Master Instance Group
    &HostGroup. 1.HostGroupType=MASTER
    &HostGroup. 1.InstanceType=ecs.mn4.2xlarge
    &HostGroup. 1.1odeCount=1
    &HostGroup. 1.Period=30
    &HostGroup. 1.SysDiskCapacity=80
    &HostGroup. 1.SysDiskType=CLOUD_SSD
    &HostGroup. 1.VSwitchId=vsw-bp10tvjyc77psy0z5****
    &UserInfo. 1.Password=pwd
    &UserInfo. 1.UserId=12345
    &UserInfo. 1.UserName=
    &AuthorizeContent=
    &Configurations=
    &EasEnable=false
    &InitCustomHiveMetaDB=
    &RelatedClusterId=
    &UseCustomHiveMetaDB=
    &WhiteListType=
    &<Common request parameters>
    ```

-   Successful response examples

    JSON format

    ```
    {
    	"ClusterId":"C-4DE6DA872B0E0D58",
    	"RequestId":"F4DE89FB-7054-475C-B7E2-B9A38152DA7E"
    }
    ```

-   Error response examples

    JSON format

    ```
    {
    	"code":"RAM.Permission.NotAllow",
    	"message":"It is not allow to execute this operation,please use RAM to authorize!",
    	"requestId":"9AEDC439-1F63-491D-B8C6-9737C372BF3A",
    	"successResponse":false
    }
    ```


## Error codes {#section_img_my5_cgb .section}

