# ModifyClusterTemplate {#concept_shb_s2y_dgb .concept}

You can call this operation to modify a cluster template.

## Request parameters {#section_pdw_lv4_dgb .section}

|Parameter|Type|Required|Example|Description |
|---------|----|--------|-------|------------|
|BizId|String|Yes|CT-4A6799A79D73385B|The ID of the cluster template.|
|ClusterType|String|Yes|HADOOP|The type of the cluster.|
|EmrVer|String|Yes|EMR-3.15.0|The version of EMR.|
|RegionId|String|Yes|cn-hangzhou|The region ID.|
|TemplateName|String|Yes|new\_template\_name|The name of the cluster template.|
|AccessKeyId|String|Yes|LTAI8ljWyu7y\*\*\*\*|The AccessKey ID.|
|AutoRenew|Boolean|No|false|Indicates whether the subscription cluster is automatically renewed.|
|BootstrapAction.N.Arg|String|No|--a|The argument that you pass into the bootstrap action.|
|BootstrapAction.N.Name|String|No|action\_name|The name of the bootstrap action.|
|BootstrapAction.N.Path|String|No|oss://bucket/path|The path where the bootstrap action script is stored.|
|ChargeType|String|No|PostPaid|The billing method.|
|Config.N.ConfigKey|String|No|fs.trash.interval|The key of the custom configuration item.|
|Config.N.ConfigValue|String|No|60|The value of the custom configuration item.|
|Config.N.Encrypt|String|No|0|A reserved parameter. Not required.|
|Config.N.FileName|String|No|yarn-site|The name of the file that contains the custom configuration item.|
|Config.N.Replace|String|No|0|A reserved parameter. Not required.|
|Config.N.ServiceName|String|No|YARN|The name \(capitalized\) of the service that is configured by using the custom configuration item.|
|Configurations|String|No|\[\{"classification": "core-site","properties": \{"fs.trash.interval": "61"\}\},\{"classification": "hadoop-log4j","properties": \{"hadoop.log.file": "hadoop1.log","hadoop.root.logger": "INFO","a.b.c": "ABC"\}\}\]|Custom software configuration. Before a cluster starts, you can specify a JSON file and modify software configuration.|
|DepositType|String|No|HALF\_MANAGED|The hosting type.|
|EasEnable|Boolean|No|true|Indicates whether the cluster is a high-security cluster.|
|HighAvailabilityEnable|Boolean|No|true|Indicates whether the cluster is a high-availability cluster.|
|HostGroup.N.AutoRenew|Boolean|No|true|Indicates whether the host group is automatically renewed.|
|HostGroup.N.ChargeType|String|No|PostPaid|The billing method.|
|HostGroup.N.ClusterId|String|No|0|A reserved parameter.|
|HostGroup.N.Comment|String|No|comment|A reserved parameter.|
|HostGroup.N.CreateType|String|No|ON\_DEMAND|A reserved parameter.|
|HostGroup.N.DiskCapacity|Integer|No|80|The data disk capacity of the host group.|
|HostGroup.N.DiskCount|Integer|No|4|The data disk number of the host group.|
|HostGroup.N.DiskType|String|No|CLOUD\_SSD|The data disk type of the host group.|
|HostGroup.N.HostGroupId|String|No|0|A reserved parameter.|
|HostGroup.N.HostGroupName|String|No|Master instance group|The name of the host group.|
|HostGroup.N.HostGroupType|String|No|MASTER|The type of the host group.|
|HostGroup.N.InstanceType|String|No|ecs.mn4.2xlarge|The instance type of the host group.|
|HostGroup.N.MultiInstanceTypes|String|No|ecs.sn1.xlarge,ecs.sn2.xlarge|The instance types, separated with commas \(,\).|
|HostGroup.N.NodeCount|Integer|No|4|The number of nodes in the host group.|
|HostGroup.N.Period|Integer|No|36|The length of the subscription. Unit: months. Valid values: 1, 2, 3, 4, 5, 6, 7, 8, 9, 12, 24, and 36.|
|HostGroup.N.SysDiskCapacity|Integer|No|80|The system disk capacity of the host group.|
|HostGroup.N.SysDiskType|String|No|CLOUD\_SSD|The system disk type of the host group.|
|HostGroup.N.VSwitchId|String|No|vsw-bp10tvjyc77psy0z5h0ni|The ID of the VSwitch.|
|InitCustomHiveMetaDb|Boolean|No|false|A reserved parameter. Not required.|
|InstanceGeneration|String|No|ecs-3|The generation of ECS instances.|
|IoOptimized|Boolean|No|true|Indicates whether I/O optimization is enabled.|
|IsOpenPublicIp|Boolean|No|true|Indicates whether the public IP address is used.|
|LogPath|String|No|oss//bucketname/path|The log path in OSS.|
|MachineType|String|No|ECS|The type of the hosts.|
|MasterPwd|String|No|pwd|The SSH password used to connect to the master node.|
|NetType|String|Yes|vpc|The type of the network.|
|OptionSoftWareList|RepeatList|No|\["ZOOKEEPER","LIVY"\]|The list of optional services.|
|Period|Integer|No|36|The length of the subscription. Unit: months. Valid values: 1, 2, 3, 4, 5, 6, 7, 8, 9, 12, 24, and 36.|
|SecurityGroupId|String|No|sg-bp1id7ajv83kmqwq2isx|The ID of the security group.|
|SecurityGroupName|String|No|emr\_sg|The name of the security group,|
|SshEnable|Boolean|No|true|Indicates whether SSH is enabled.|
|UseCustomHiveMetaDb|Boolean|No|false|false|
|UseLocalMetaDb|Boolean|No|true|Indicates whether to use the local Hive metadatabase.|
|UserDefinedEmrEcsRole|String|No|AliyunEmrEcsDefaultRole|The role that is assigned to EMR for calling ECS resources.|
|VSwitchId|String|No|vsw-bp10tvjyc77psy0z5h0ni|The ID of the VSwitch.|
|VpcId|String|No|vpc-bp1l4urd87xlh7i4bju4h|The ID of the VPC.|
|ZoneId|String|No|cn-hangzhou-b|The zone ID.|

## Response parameters {#section_vdw_lv4_dgb .section}

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|BF4FBAC6-B03E-4BFB-B6DB-EB53C34F2E22|The ID of the request.|
|ClusterTemplateId|String|CT-4A6799A79D73385B|The ID of the cluster template.|

## Examples {#section_ydw_lv4_dgb .section}

-   Sample requests

    ```
    /? BizId=CT-4A6799A79D73385B
    &ClusterType=HADOOP
    &EmrVer=EMR-3.15.0
    &RegionId=cn-hangzhou
    &TemplateName=new_template_name
    &AccessKeyId=LTAI8ljWyu7y****
    &AutoRenew=false
    &ChargeType=PostPaid
    &Configurations=
    &DepositType=HALF_MANAGED
    &EasEnable=true
    &HighAvailabilityEnable=true
    &InitCustomHiveMetaDb=
    &InstanceGeneration=
    &IoOptimized=true
    &IsOpenPublicIp=true
    &LogPath=oss//bucketname/path
    &MachineType=ECS
    &MasterPwd=pwd
    &NetType=vpc
    &OptionSoftWareList. 1=["ZOOKEEPER","LIVY"]
    &Period=36
    &SecurityGroupId=sg-bp1id7ajv83kmqwq2isx
    &SecurityGroupName=emr_sg
    &SshEnable=true
    &UseCustomHiveMetaDb=
    &UseLocalMetaDb=true
    &UserDefinedEmrEcsRole=AliyunEmrEcsDefaultRole
    &VpcId=vpc-bp1l4urd87xlh7i4bju4h
    &VSwitchId=vsw-bp10tvjyc77psy0z5h0ni
    &ZoneId=cn-hangzhou-b
    &BootstrapAction. 1.Arg=
    &BootstrapAction. 1.1ame=
    &BootstrapAction. 1.Path=
    &Config. 1.ConfigKey=
    &Config. 1.ConfigValue=
    &Config. 1.Encrypt=
    &Config. 1.FileName=
    &Config. 1.Replace=
    &Config. 1.ServiceName=
    &HostGroup. 1.AutoRenew=true
    &HostGroup. 1.ChargeType=PostPaid
    &HostGroup. 1.ClusterId=
    &HostGroup. 1.Comment=
    &HostGroup. 1.CreateType=
    &HostGroup. 1.DiskCapacity=80
    &HostGroup. 1.DiskCount=4
    &HostGroup. 1.DiskType=CLOUD_SSD
    &HostGroup. 1.HostGroupId=
    &HostGroup. 1.HostGroupName=Master instance group
    &HostGroup. 1.HostGroupType=MASTER
    &HostGroup. 1.InstanceType=ecs.mn4.2xlarge
    &HostGroup. 1.MultiInstanceTypes=
    &HostGroup. 1.1odeCount=4
    &HostGroup. 1.Period=36
    &HostGroup. 1.SysDiskCapacity=80
    &HostGroup. 1.SysDiskType=CLOUD_SSD
    &HostGroup. 1.VSwitchId=vsw-bp10tvjyc77psy0z5h0ni
    &<Common request parameters>
    ```

-   Successful response examples

    JSON format

    ```
    {
    	"RequestId":"5EDD1207-5DAB-42F9-9BF9-7591286A8F3F"
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


## Error codes {#section_a2w_lv4_dgb .section}

