# DescribeClusterTemplate {#concept_vhx_bv4_dgb .concept}

You can call this operation to view the details of a cluster template.

## Request parameters {#section_pdw_lv4_dgb .section}

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|BizId|String|Yes|CT-35498C56B3F12002|The ID of the cluster template.|
|AccessKeyId|String|Yes|LTAI8ljWyu7y\*\*\*\*|The AccessKey ID.|

## Response parameters {#section_vdw_lv4_dgb .section}

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|F2BF8586-045D-4104-B00C-44A4AA619C05|The ID of the request.|
|TemplateInfo| | |The details of the cluster template.|
|Id|String|CT-4A6799A79D73385B|The ID of the cluster template.|
|TemplateName|String|template\_name\_2|The name of the cluster template.|
|EmrVer|String|EMR-3.16.0|The EMR version.|
|LogEnable|Boolean|true|Indicates whether OSS logging is enabled.|
|LogPath|String|oss//bucketname/path|The log path in OSS.|
|UserId|String|125046002175\*\*\*\*|The ID of the user that creates the cluster template.|
|UserDefinedEmrEcsRole|String|AliyunEmrEcsDefaultRole|The role that is assigned to EMR for calling ECS resources.|
|MasterNodeTotal|Integer|1|The number of master nodes.|
|VpcId|String|vpc-bp1l4urd87xlh7i4bju4h|The ID of the VPC.|
|VSwitchId|String|vsw-bp10tvjyc77psy0z5h0ni|The ID of the VSwitch.|
|NetType|String|vpc|The type of the network.|
|IoOptimized|Boolean|true|Indicates whether I/O optimization is enabled.|
|InstanceGeneration|String|ecs-3|The generation of ECS instances.|
|HighAvailabilityEnable|Boolean|true|Indicates whether the cluster is a high-availability cluster.|
|EasEnable|Boolean|true|Indicates whether the cluster is a high-security cluster.|
|GmtCreate|Long|1543765033000|The creation time.|
|GmtModified|Long|1543765033000|The modification time.|
|ZoneId|String|cn-hangzhou-b|The zone ID.|
|ClusterType|String|HADOOP|The type of the cluster.|
|SecurityGroupId|String|sg-bp1id7ajv83kmqwq2isx|The ID of the security group.|
|SecurityGroupName|String|emr\_sg|The name of the security group to be created.|
|Configurations|String|\[\{"classification": "core-site","properties": \{"fs.trash.interval": "61"\}\},\{"classification": "hadoop-log4j","properties": \{"hadoop.log.file": "hadoop1.log","hadoop.root.logger": "INFO","a.b.c": "ABC"\}\}\]|The custom software configurations. Before a cluster starts, you can specify a JSON file and modify software configuration.|
|AllowNotebook|Boolean|false|Indicates whether Notebook is allowed.|
|CreateSource|String|2|The method used to create the cluster template.|
|UseLocalMetaDb|Boolean|true|Indicates whether the local Hive metadatabase is used.|
|SshEnable|Boolean|true|Indicates whether SSH is enabled.|
|IsOpenPublicIp|Boolean|true|Indicates whether the public IP address is assigned.|
|DepositType|String|FULL\_MANAGED|The hosting type.|
|MachineType|String|ECS|The machine type.|
|UseCustomHiveMetaDb|Boolean|false|A reserved parameter.|
|InitCustomHiveMetaDb|Boolean|true|A reserved parameter.|
|BootstrapActionList| | |The list of bootstrap actions.|
|Name|String|action\_name|The name of the action.|
|Path|String|oss://bucket/path|The path where the bootstrap action script is stored.|
|Arg|String|--a|The argument that you pass into the bootstrap action.|
|HostGroupList| | |The list of node groups.|
|HostGroupId|String|0|A reserved parameter.|
|HostGroupName|String|Master instance group|The name of the node group.|
|HostGroupType|String|MASTER|The type of the node group.|
|ChargeType|String|PostPaid|The billing method.|
|Period|String|36|The length of the subscription. Unit: days.|
|NodeCount|Integer|2|The number of nodes in the node group.|
|InstanceType|String|ecs.mn4.2xlarge|The instance type of the node group.|
|DiskType|String|CLOUD\_SSD|The data disk type of the node group.|
|DiskCapacity|Integer|80|The data disk capacity of the node group.|
|DiskCount|Integer|4|The data disk number of the node group.|
|SysDiskType|String|CLOUD\_SSD|The system disk type of the node group.|
|SysDiskCapacity|Integer|40|The system disk capacity of the node group.|
|MultiInstanceTypes|String|ecs.sn1.xlarge,ecs.sn2.xlarge|The instance types, separated with commas \(,\).|
|ConfigList| | |The list of custom configuration items.|
|ServiceName|String|YARN|The name \(capitalized\) of the service that is configured by using the custom configuration item.|
|FileName|String|yarn-site|The name of the file that contains the custom configuration item.|
|ConfigKey|String|fs.trash.interval|The key of the custom configuration item.|
|ConfigValue|String|60|The value of the custom configuration item.|
|Encrypt|String|0|A reserved parameter.|
|Replace|String|0|A reserved parameter.|
|SoftwareInfoList|String|\["ZOOKEEPER"\]|The optional services.|

## Examples {#section_ydw_lv4_dgb .section}

-   Sample requests

    ```
    /? BizId=CT-4A6799A79D73385B
    &AccessKeyId=LTAI8ljWyu7y****
    &<Common request parameters>
    ```

-   Successful response examples

    JSON format

    ```
    {
    	"requestId":"29A1D3B7-661C-4FCD-8577-DE93C8F6CA55",
    	"templateInfo":{
    		"bootstrapActionList":[],
    		"clusterType":"HADOOP",
    		"configList":[],
    		"createSource":"2",
    		"depositType":"HALF_MANAGED",
    		"easEnable":false,
    		"emrVer":"EMR-3.16.0",
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
    	}
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

