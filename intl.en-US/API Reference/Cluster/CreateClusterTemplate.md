# CreateClusterTemplate {#concept_k3m_qz5_cgb .concept}

You can call this operation to create a cluster template.

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|ClusterType|String|Yes|HADOOP|The type of the cluster.|
|Configurations|String|No|\[\{"classification": "core-site","properties": \{"fs.trash.interval": "61"\}\},\{"classification": "hadoop-log4j","properties": \{"hadoop.log.file": "hadoop1.log","hadoop.root.logger": "INFO","a.b.c": "ABC"\}\}\]|Custom software configuration. Before a cluster starts, you can specify a JSON file and modify software configuration.|
|TemplateName|String|Yes|templateName2|The name of the template.|
|EasEnable|Boolean|No|true|Indicates whether the cluster is high-security.|
|HighAvailabilityEnable|Boolean|No|true|Indicates whether the cluster is high-availability.|
|InitCustomHiveMetaDb|Boolean|No|true|A value of true indicates that the value of the init.meta.db configuration item in the hive-site file is set to true.|
|InstanceGeneration|String|No|ecs-3|The generation of the ECS instances.|
|AutoRenew|Boolean|No|false|Indicates whether the Subscription cluster is auto-renewed.|
|IoOptimized|Boolean|No|true|Indicates whether I/O optimization is enabled.|
|IsOpenPublicIp|Boolean|No|true|Indicates whether the public IP address is assigned.|
|LogPath|String|No|oss://bucket/path|The log path in OSS.|
|MachineType|String|No|ECS|Not required. Valid value: ECS.|
|MasterPwd|String|No|pwd|The password for the master instance.|
|NetType|String|No|vpc|The network type.|
|OptionSoftWareList.N|RepeatList|No|\["HBASE","FLINK"\]|The list of optional services.|
|Period|Integer|No|36|The length of the subscription.|
|SecurityGroupId|String|No|sg-bp1id7ajv83kmqwq\*\*\*\*|The ID of the security group.|
|SecurityGroupName|String|No|sg-name|The name of the security group to create.|
|SshEnable|Boolean|No|true|Indicates whether SSH is enabled for the instances in the cluster.|
|UseCustomHiveMetaDb|Boolean|No|false|Indicates whether unified Hive metadata is used.|
|UseLocalMetaDb|Boolean|No|false|Indicates whether local Hive metadatabase is used.|
|UserDefinedEmrEcsRole|String|No|AliyunEmrEcsDefaultRole|The name of the role used to call ECS resources.|
|VpcId|String|No|vpc-bp1l4urd87xlh7i4b\*\*\*\*|The ID of the VPC.|
|VSwitchId|String|No|vsw-bp10tvjyc77psy0z5\*\*\*\*|The ID of the VSwitch.|
|ZoneId|String|Yes|cn-hangzhou-b|The zone ID.|
|BootstrapAction.N.Arg|String|No|--a|The argument that you pass into the bootstrap action.|
|BootstrapAction.N.Name|String|No|action\_name|The name of the bootstrap action.|
|BootstrapAction.N.Path|String|No|oss://bucket/path|The path where the bootstrap action script is stored.|
|Config.N.ConfigKey|String|No|fs.trash.interval|The key of the custom configuration item.|
|Config.N.ConfigValue|String|No|60|The value of the custom configuration item.|
|Config.N.Encrypt|String|No|0|A reserved parameter. Not required.|
|Config.N.FileName|String|No|yarn-site|The name of the file that contains the configuration item.|
|Config.N.Replace|String|No|0|A reserved parameter. Not required.|
|Config.N.ServiceName|String|No|YARN|The name \(capitalized\) of the service that is configured by using the custom configuration item.|
|HostGroup.N.AutoRenew|Boolean|No|false|Indicates whether the cluster is auto-scaled.|
|HostGroup.N.ChargeType|String|Yes|PostPaid|The billing method for the node group.|
|HostGroup.N.ClusterId|String|No|0|Not required.|
|HostGroup.N.Comment|String|No|header|The comment on the node group.|
|HostGroup.N.CreateType|String|No|ON-DEMAND|The creation type of the node group.|
|HostGroup.N.DiskCapacity|Integer|Yes|80|The data disk capacity of the node group.|
|HostGroup.N.DiskCount|Integer|Yes|1|The data disk number of the node group.|
|HostGroup.N.DiskType|String|Yes|CLOUD\_EFFICIENCY|The data disk type of the node group.|
|HostGroup.N.HostGroupId|String|No|0|Not required.|
|HostGroup.N.HostGroupName|String|Yes|Master Instance Group|The name of the node group.|
|HostGroup.N.HostGroupType|String|Yes|MASTER|The type of the node group.|
|HostGroup.N.InstanceType|String|Yes|ecs.g5.xlarge|The instance type of the node group.|
|HostGroup.N.MultiInstanceTypes|String|No|ecs.sn1.xlarge,ecs.sn2.xlarge|The instance types of the node group, separated with commas \(,\).|
|HostGroup.N.NodeCount|Integer|No|2|The node number in the node group.|
|HostGroup.N.Period|Integer|No|30|The length of the subscription. Unit: days. A value is required when HostGroup.n.ChargeType=PrePaid.|
|HostGroup.N.SysDiskCapacity|Integer|Yes|80|The system disk capacity of the node group.|
|HostGroup.N.SysDiskType|String|Yes|CLOUD\_SSD|The system disk type of the node group.|
|HostGroup.N.VSwitchId|String|No|vsw-bp10tvjyc77psy0z5\*\*\*\*|The ID of the VSwitch.|
|EmrVer|String|Yes|EMR-3.15.0|The version of EMR.|
|RegionId|String|Yes|cn-hangzhou|The region ID.|
|AccessKeyId|String|Yes|LTAI8ljWyu7y\*\*\*\*|The AccessKey ID.|
|AutoRenew|Boolean|No|false|Indicates whether the cluster is auto-renewed.|
|DepositType|String|No|HALF\_MANAGED|The hosting type of the cluster.|

## Response parameters {#section_kg2_c2v_cgb .section}

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|BF4FBAC6-B03E-4BFB-B6DB-EB53C34F2E22|The ID of the request.|
|ClusterTemplateId|String|CT-35498C56B3F12002|The ID of the cluster template.|

## Examples {#section_yr3_jz1_kfb .section}

-   Sample requests

    ```
    /? ClusterType=HADOOP
    &EmrVer=EMR-3.15.0
    &RegionId=cn-hangzhou
    &TemplateName=templateName2
    &AccessKeyId=LTAI8ljWyu7y****
    &AutoRenew=false
    &Configurations=[{"classification": "core-site","properties": {"fs.trash.interval": "61"}},{"classification": "hadoop-log4j","properties": {"hadoop.log.file": "hadoop1.log","hadoop.root.logger": "INFO","a.b.c": "ABC"}}]
    &DepositType=HALF_MANAGED
    &EasEnable=true
    &HighAvailabilityEnable=true
    &InitCustomHiveMetaDb=
    &InstanceGeneration=
    &IoOptimized=true
    &IsOpenPublicIp=true
    &LogPath=oss://bucket/path
    &MachineType=
    &MasterPwd=pwd
    &NetType=vpc
    &OptionSoftWareList. 1=["HBASE","FLINK"]
    &Period=36
    &SecurityGroupId=sg-bp1id7ajv83kmqwq****
    &SecurityGroupName=sg-name
    &SshEnable=true
    &UseCustomHiveMetaDb=false
    &UseLocalMetaDb=false
    &UserDefinedEmrEcsRole=AliyunEmrEcsDefaultRole
    &VpcId=vpc-bp1l4urd87xlh7i4b****
    &VSwitchId=vsw-bp10tvjyc77psy0z5****
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
    &HostGroup. 1.AutoRenew=false
    &HostGroup. 1.ChargeType=PostPaid
    &HostGroup. 1.ClusterId=
    &HostGroup. 1.Comment=
    &HostGroup. 1.CreateType=
    &HostGroup. 1.DiskCapacity=80
    &HostGroup. 1.DiskCount=1
    &HostGroup. 1.DiskType=CLOUD_EFFICIENCY
    &HostGroup. 1.HostGroupId=
    &HostGroup. 1.HostGroupName=Master Instance Group
    &HostGroup. 1.HostGroupType=MASTER
    &HostGroup. 1.InstanceType=ecs.g5.xlarge
    &HostGroup. 1.MultiInstanceTypes=
    &HostGroup. 1.1odeCount=2
    &HostGroup. 1.Period=30
    &HostGroup. 1.SysDiskCapacity=80
    &HostGroup. 1.SysDiskType=CLOUD_SSD
    &HostGroup. 1.VSwitchId=vsw-bp10tvjyc77psy0z5h0ni
    &<Common request parameters>
    ```

-   Successful response examples

    JSON format

    ```
    {
    	"code":"200",
    	"data":{
    		"ClusterTemplateId":"CT-35498C56B3F12002",
    		"RequestId":"8CA40D40-2092-4A09-9F07-2F9C1399FB11"
    	},
    	"requestId":"8CA40D40-2092-4A09-9F07-2F9C1399FB11",
    	"successResponse":true
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

