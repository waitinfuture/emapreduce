# ListClusterTemplates {#concept_lsy_rmw_dgb .concept}

You can call this operation to view the list of cluster templates.

## Request parameters {#section_pdw_lv4_dgb .section}

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|PageNumber|Integer|Yes|1|The page number. The minimum value is 1.|
|PageSize|Integer|Yes|10|The number of records that are displayed on each page.|
|AccessKeyId|String|Yes|LTAI8ljWyu7y\*\*\*\*|The AccessKey ID.|
|BizId|String|No|CT-4A6799A79D73385B|The ID of the cluster template.|
|RegionId|String|No|cn-hangzhou|The region ID.|

## Response parameters {#section_vdw_lv4_dgb .section}

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|BF4FBAC6-B03E-4BFB-B6DB-EB53C34F2E22|The ID of the request.|
|TotalCount|Integer|12|The total number of cluster templates.|
|PageNumber|Integer|1|The page number. The minimum number is 1.|
|PageSize|Integer|10|The number of records that are displayed on each page.|
|TemplateInfoList| | |The list of cluster templates.|
|Id|String|CT-4A6799A79D73385B|The ID of the cluster template.|
|TemplateName|String|template\_name\_2|The name of the cluster template.|
|LogEnable|Boolean|true|Indicates whether OSS logging is enabled.|
|LogPath|String|oss//bucketname/path|The log path in OSS.|
|UserId|String|125046002175\*\*\*\*|The ID of the user that creates the cluster.|
|UserDefinedEmrEcsRole|String|AliyunEmrEcsDefaultRole|The role that is assigned to EMR for calling ECS resources.|
|MasterNodeTotal|Integer|1|The number of master nodes.|
|VpcId|String|vpc-bp1l4urd87xlh7i4bju4h|The ID of the VPC.|
|VSwitchId|String|vsw-bp10tvjyc77psy0z5h0ni|The ID of the VSwitch.|
|NetType|String|vpc|The network type.|
|IoOptimized|Boolean|true|Indicates whether I/O optimization is enabled.|
|InstanceGeneration|String|ecs-3|The generation of ECS instances.|
|HighAvailabilityEnable|Boolean|true|Indicates whether the cluster is high-availability.|
|EasEnable|Boolean|true|Indicates whether the cluster is high-security.|
|GmtCreate|Long|1543765033000|The creation time of the cluster template.|
|GmtModified|Long|1543765033000|The modification time of the cluster template.|
|ZoneId|String|cn-hangzhou-b|The zone ID.|
|ClusterType|String|HADOOP|The type of the cluster.|
|SecurityGroupId|String|sg-bp1id7ajv83kmqwq2isx|The ID of the security group.|
|SecurityGroupName|String|emr\_sg|The name of the security group to create.|
|Configurations|String|\[\{"classification": "core-site","properties": \{"fs.trash.interval": "61"\}\},\{"classification": "hadoop-log4j","properties": \{"hadoop.log.file": "hadoop1.log","hadoop.root.logger": "INFO","a.b.c": "ABC"\}\}\]|Custom software configuration Before a cluster starts, you can specify a JSON file and modify software configuration.|
|AllowNotebook|Boolean|true|A reserved parameter.|
|CreateSource|String|2|The method used to create the cluster template.|
|UseLocalMetaDb|Boolean |false|Indicates whether local Hive metadata is used.|
|SshEnable|Boolean|true|Indicates whether SSH is enabled.|
|IsOpenPublicIp|Boolean|true|Indicates whether the public IP address is assigned.|
|DepositType|String|HALF\_MANAGED|The hosting type.|
|MachineType|String|ECS|The type of the machine.|
|UseCustomHiveMetaDb|Boolean|false|A reserved parameter.|
|InitCustomHiveMetaDb|Boolean|true|A value of true indicates that the value of the init.meta.db configuration item in the hive-site file is set to true.|
|BootstrapActionList| | |The list of bootstrap actions.|
|Name|String|create|The name of the bootstrap action.|
|Path|String|oss://bucket/path|The path where the bootstrap action is stored.|
|Arg|String|---a|The argument that you pass to the bootstrap action.|
|HostGroupList| | |The list of the host groups.|
|HostGroupId|String|0|A reserved parameter.|
|HostGroupName|String|Master instance group|The name of the host group.|
|HostGroupType|String|MASTER|The type of the host group.|
|ChargeType|String|PostPaid|The billing method.|
|Period|String|36|The length of the subscription. Unit: months. Valid values: 1, 2, 3, 4, 5, 6, 7, 8, 9, 12, 24, and 36.|
|NodeCount|Integer|2|The number of nodes in the host group.|
|InstanceType|String|ecs.mn4.2xlarge|The instance type of the host group.|
|DiskType|String|CLOUD\_SSD|The data disk type of the host group.|
|DiskCapacity|Integer|80|The data disk capacity of the host group.|
|DiskCount|Integer|4|The data disk number of the host group.|
|SysDiskType|String|CLOUD\_SSD|The system disk type of the host group.|
|SysDiskCapacity|Integer|40|The system disk capacity of the host group.|
|MultiInstanceTypes|String|ecs.sn1.xlarge,ecs.sn2.xlarge|The instance types, separated with commas \(,\).|
|ConfigList| | |The list of custom configuration items.|
|ServiceName|String|YARN|The name \(capitalized\) of the custom configuration item.|
|FileName|String|yarn-site|The name of the file that contains the custom configuration item.|
|ConfigKey|String|fs.trash.interval|The key of the custom configuration item.|
|ConfigValue|String|60|The value of the custom configuration item.|
|Encrypt|String|0|A reserved parameter.|
|Replace|String|0|A reserved parameter.|
|SoftwareInfoList|String|\["ZOOKEEPER"\]|The list of optional services.|

## Examples {#section_ydw_lv4_dgb .section}

-   Sample requests

    ```
    /? PageNumber=1
    &PageSize=10
    &AccessKeyId=LTAI8ljWyu7y**** 
    &BizId=CT-4A6799A79D73385B 
    &RegionId=cn-hangzhou
    &<Common request parameters>
    ```

-   Successful response examples

    JSON format

    ```
    {
    	"pageNumber":1,
    	"pageSize":10,
    	"requestId":"EC911E1F-8F04-4059-A7A2-6B97EE46A943",
    	"templateInfoList":[{
    		"bootstrapActionList":[],
    		"clusterType":"HADOOP",
    		"configList":[],
    		"createSource":"2",
    		"depositType":"HALF_MANAGED",
    		"easEnable":false,
    		"gmtCreate":1543765033000,
    		"gmtModified":1543765033000,
    		"highAvailabilityEnable":false,
    		"hostGroupList":[
    			{
    				"chargeType":"PostPaid",
    				"diskCapacity":80,
    				"diskCount":1,
    				"diskType":"CLOUD_EFFICIENCY",
    				"hostGroupId":"",
    				"hostGroupName":"Master instance group",
    				"hostGroupType":"MASTER",
    				"instanceType":"ecs.n4.xlarge",
    				"nodeCount":1
    			},
    			{
    				"chargeType":"PostPaid",
    				"diskCapacity":80,
    				"diskCount":4,
    				"diskType":"CLOUD_EFFICIENCY",
    				"hostGroupId":"",
    				"hostGroupName":"Core instance group",
    				"hostGroupType":"CORE",
    				"instanceType":"ecs.n4.xlarge",
    				"nodeCount":2
    			}
    		],
    		"id":"CT-35498C56B3F12002",
    		"ioOptimized":true,
    		"isOpenPublicIp":true,
    		"logEnable":false,
    		"machineType":"ECS",
    		"masterNodeTotal":0,
    		"netType":"vpc",
    		"securityGroupId":"sg-bp1id7ajv83kmqwq2isx",
    		"securityGroupName":"emxxx",
    		"softwareInfoList":[
    			"FLUME",
    			"FLINK"
    		],
    		"sshEnable":false,
    		"templateName":"template_name_2",
    		"useLocalMetaDb":true,
    		"userDefinedEmrEcsRole":"AliyunEmrEcsDefaultRole",
    		"userId":125046002175****,
    		"vSwitchId":"vsw-bp10tvjyc77psy0z5h0ni",
    		"vpcId":"vpc-bp1l4urd87xlh7i4bju4h",
    		"zoneId":"cn-hangzhou-b"
    	}],
    	"totalCount":1
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

