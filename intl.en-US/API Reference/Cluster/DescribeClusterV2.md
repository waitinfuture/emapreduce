# DescribeClusterV2 {#concept_gcm_ydb_kfb .concept}

You can call this operation to view the basic information of a cluster, including the billing methods, ECS instance overview, and E-MapReduce service list.

## Request parameters {#section_u3c_m2b_kfb .section}

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Id|String|Yes|C-E331B8AC12BF5DB4|The ID of the cluster.|
|RegionId|String|Yes|cn-hangzhou|The region ID.|
|AccessKeyId|String|Yes|LTAI8ljWyu7y\*\*\*\*|The AccessKey ID.|

## Response parameters {#section_vdw_lv4_dgb .section}

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|14E9C045-9B8D-4D1E-8D23-FC0027B6D947|The ID of the request.|
|ClusterInfo| | |The details of the cluster.|
|Id|String|C-E331B8AC12BF5DB4|The ID of the cluster.|
|RegionId|String|cn-hangzhou|The region ID.|
|ZoneId|String|cn-hangzhou-b|The zone ID.|
|Name|String|cluster\_name|The name of the cluster.|
|RelateClusterId|String|C-D7958B72E59BAB88|The ID of the cluster associated by the Gateway.|
|GatewayClusterIds|String|C-D7958B72E59BAB88|The IDs of the Gateway clusters.|
|CreateType|String|MANUAL|The creation method for the cluster.|
|StartTime|Long|1543804234000|The start time of the cluster \(timestamp\).|
|ExpiredTime|Long|1544076205000|The expiration time of the subscription cluster.|
|StopTime|Long|1543804234000|The stop time of the cluster.|
|LogEnable|Boolean|true|Indicates whether OSS logging is enabled.|
|LogPath|String|oss://bucketname/path|The log path in OSS.|
|UserId|String|125046002175\*\*\*\*|The ID of the user.|
|Status|String|IDLE|The status of the cluster.|
|HighAvailabilityEnable|Boolean|true|Indicates whether the cluster is a high-availability cluster.|
|LocalMetaDb|Boolean|true|Indicates whether the local Hive metadatabase is used.|
|ChargeType|String|PostPaid|The billing method.|
|DepositType|String|HALF\_MANAGED|The hosting type of the cluster.|
|Period|Integer|36|The length of the subscription. Unit: days.|
|RunningTime|Integer|1102|The runtime. Unit: seconds.|
|MasterNodeTotal|Integer|2|The number of master nodes.|
|MasterNodeInService|Integer|2|The number of master nodes that are in use.|
|CoreNodeTotal|Integer|3|The number of core nodes.|
|CoreNodeInService|Integer|3|The number of core nodes that are in use.|
|TaskNodeTotal|Integer|2|The number of tasks nodes.|
|TaskNodeInService|Integer|2|The number of tasks nodes that are in use.|
|ShowSoftwareInterface|Boolean|true|Indicates whether the quick link page is displayed.|
|CreateResource|String|ECM\_EMR|The label of the cluster.|
|VpcId|String|vpc-bp1l4urd87xlh7i4bju4h|The ID of the VPC.|
|VSwitchId|String|vsw-bp11qjbyggil4pow064fg|The ID of the VSwitch.|
|NetType|String|vpc|The network type of the cluster.|
|UserDefinedEmrEcsRole|String|AliyunEmrEcsDefaultRole|The name of the EMR role.|
|IoOptimized|Boolean|true|Indicates whether I/O optimization is enabled.|
|InstanceGeneration|String|ecs-3|The generation of ECS instances.|
|ImageId|String|m-bp118knl07yk88y8s6cj|The ID of the image used to create the cluster.|
|SecurityGroupId|String|sg-bp1bvompzxgx7q0lg7oy|The ID of the security group.|
|SecurityGroupName|String|emr-default-securitygroup|The name of the security group.|
|BootstrapFailed|Boolean|false|Indicates whether the bootstrap action fails.|
|Configurations|String|\[\]|Not required.|
|EasEnable|Boolean|true|Indicates whether the cluster is a high-security cluster.|
|AutoScalingEnable|Boolean|false|Indicates whether auto-scaling is enabled.|
|AutoScalingAllowed|Boolean|false|Indicates whether auto-scaling is allowed.|
|AutoScalingSpotWithLimitAllowed|Boolean|false|Indicates whether using auto-scaling spot instances is allowed.|
|ResizeDiskEnable|Boolean|true|Indicates whether scaling out disks is allowed.|
|GatewayClusterInfoList| | |The information of the Gateway clusters.|
|ClusterId|String|C-D7958B72E59BAB88|The ID of the Gateway cluster.|
|ClusterName|String|gateway-name|The name of the Gateway cluster.|
|HostGroupList| | |The list of node groups.|
|HostGroupId|String|G-9D08642FB8CE8CFE|The ID of the node group.|
|HostGroupName|String|Master instance group|The name of the node group.|
|HostGroupType|String|CORE|The role of the node group.|
|HostGroupSubType|String|0|A reserved parameter.|
|HostGroupChangeType|String|PostPaid|The biling method for the node group.|
|hostGroupChangeStatus|String|0|A reserved parameter.|
|ChargeType|String|PostPaid|The billing method.|
|Period|String|30|The length of the subscription. Unit: months. Valid values: 1, 2, 3, 4, 5, 6, 7, 8, 9, 12, 24, and 36.|
|NodeCount|Integer|4|The number of nodes in the node group.|
|InstanceType|String|ecs.n4.2xlarge|The instance type of the node group.|
|CpuCore|Integer|16|The number of vCPUs.|
|MemoryCapacity|Integer|16|The memory size.|
|DiskType|String|CLOUD\_SSD|The type of the data disks.|
|DiskCapacity|Integer|120|The capacity of each data disk.|
|DiskCount|Integer|4|The number of data disks.|
|BandWidth|String|10|The bandwidth.|
|LockType|String|0|A reserved parameter.|
|LockReason|String|0|A reserved parameter.|
|Nodes| | |The list of nodes.|
|ZoneId|String|cn-hangzhou-e|The zone ID.|
|InstanceId|String|i-bp1ftve3lzvpm16hp7lo|The instance ID.|
|Status|String|NORMAL|The status.|
|PubIp|String|47.99.162.57|The public IP address.|
|InnerIp|String|192.168.128.236|The internal IP address.|
|ExpiredTime|String|2099-12-31T15:59Z|The expiration time of the subscription cluster.|
|CreateTime|String|1543804242000|The creation time.|
|EmrExpiredTime|String|2099-12-31T15:59Z|The expiration time of the ECS instance.|
|DaemonInfos| | |A reserved parameter.|
|Name|String|0|A reserved parameter.|
|DiskInfos| | |The list of disk information.|
|Device|String|/dev/xvdb|The device information of the disk.|
|DiskName|String1|disk1|The name of the disk.|
|DiskId|String|d-bp15vg2nr3x2t0f37ko9|The ID of the disk.|
|Type|String|data|The type of the disk.|
|Size|Integer|80|The capacity of the disk. Unit: gibibytes.|
|BootstrapActionList| | |The list of bootstrap actions.|
|Name|String|action\_name|The name of the bootstrap action.|
|Path|String|oss://bucket/path|The path where the bootstrap action script is stored.|
|Arg|String|--a|The argument that you pass into the bootstrap action.|
|RelateClusterInfo| | |The information of the cluster associated by the Gateway.|
|ClusterId|String|C-D7958B72E59BAB88|The ID of the associated cluster.|
|ClusterName|String|main|The name of the associated cluster.|
|FailReason| | |The cause of the cluster creation failure.|
|ErrorCode|String|InvalidImageId.NotFound|The error code.|
|ErrorMsg|String|The specified ImageId does not exist.|The error message.|
|RequestId|String|B8DC3A91-3953-4444-92BB-DBC29C47EC1A|The ID of the request.|
|SoftwareInfo| | |The list of service information.|
|EmrVer|String|EMR-3.16.0|The version of EMR.|
|ClusterType|String|HADOOP|The type of the cluster.|
|Softwares| | |The list of services.|
|DisplayName|String|Hive|The name of the service.|
|Name|String|HIVE|The internal name of the service.|
|OnlyDisplay|Boolean|false|Indicates whether to display the service name.|
|StartTpe|Integer|1|The start type.|
|Version|String|2.3.3|The version of the service.|
|AccessInfo| | |The access information.|
|ZKLinks| | |The access information of ZooKeeper.|
|Link|String|emr-worker-1,emr-header-2,emr-header-1|The nodes where ZooKeeper is deployed.|
|Port|String|2181|The port used by ZooKeeper.|

## Examples {#section_ydw_lv4_dgb .section}

-   Sample requets

    ```
    /? Id=C-E331B8AC12BF5DB4
    &RegionId=cn-hangzhou
    &AccessKeyId=LTAI8ljWyu7y****
    &<Commin request parameters>
    ```

-   Successful response examples

    JSON format

    ```
    {
    	"code":"200",
    	"data":{
    		"ClusterInfo":{
    			"AutoScalingAllowed":true,
    			"AutoScalingEnable":false,
    			"AutoScalingSpotWithLimitAllowed":true,
    			"BootstrapActionList":{
    				"BootstrapAction":[]
    			},
    			"BootstrapFailed":false,
    			"ChargeType":"PostPaid",
    			"Configurations":"",
    			"CoreNodeInService":2,
    			"CoreNodeTotal":2,
    			"CreateResource":"ECM_EMR",
    			"CreateType":"MANUAL",
    			"EasEnable":true,
    			"GatewayClusterInfoList":{
    				"GatewayClusterInfo":[]
    			},
    			"HighAvailabilityEnable":true,
    			"HostGroupList":{
    				"HostGroup":[
    					{
    						"ChargeType":"PostPaid",
    						"CpuCore":8,
    						"DiskCapacity":80,
    						"DiskCount":1,
    						"DiskType":"CLOUD_SSD",
    						"HostGroupId":"G-79AA94922DFBE2E2",
    						"HostGroupName":"Master instance group",
    						"HostGroupType":"MASTER",
    						"InstanceType":"ecs.n4.2xlarge",
    						"MemoryCapacity":16,
    						"NodeCount":2,
    						"Nodes":{
    							"Node":[
    								{
    									"CreateTime":"1543804242000",
    									"DaemonInfos":{
    										"DaemonInfo":[]
    									},
    									"DiskInfos":{
    										"DiskInfo":[
    											{
    												"Device":"/dev/xvdb",
    												"DiskId":"d-bp15vg2nr3x2t0f37ko9",
    												"Size":80,
    												"Type":"data"
    											},
    											{
    												"Device":"/dev/xvda",
    												"DiskId":"d-bp16c7qipsxtrq97mybg",
    												"Size":120,
    												"Type":"system"
    											}
    										]
    									},
    									"EmrExpiredTime":"null",
    									"ExpiredTime":"2099-12-31T15:59Z",
    									"InnerIp":"192.168.128.235",
    									"InstanceId":"i-bp16c7qipsxtrq99ixt2",
    									"PubIp":"47.97.214.105",
    									"Status":"NORMAL",
    									"ZoneId":"cn-hangzhou-b"
    								},
    								{
    									"CreateTime":"1543804242000",
    									"DaemonInfos":{
    										"DaemonInfo":[]
    									},
    									"DiskInfos":{
    										"DiskInfo":[
    											{
    												"Device":"/dev/xvdb",
    												"DiskId":"d-bp1568nnv4ev81672qe6",
    												"Size":80,
    												"Type":"data"
    											},
    											{
    												"Device":"/dev/xvda",
    												"DiskId":"d-bp16z8pr0y2vgfkghuba",
    												"Size":120,
    												"Type":"system"
    											}
    										]
    									},
    									"EmrExpiredTime":"null",
    									"ExpiredTime":"2099-12-31T15:59Z",
    									"InnerIp":"192.168.128.236",
    									"InstanceId":"i-bp1ftve3lzvpm16hp7lo",
    									"PubIp":"47.99.162.57",
    									"Status":"NORMAL",
    									"ZoneId":"cn-hangzhou-b"
    								}
    							]
    						}
    					},
    					{
    						"ChargeType":"PostPaid",
    						"CpuCore":8,
    						"DiskCapacity":80,
    						"DiskCount":4,
    						"DiskType":"CLOUD_SSD",
    						"HostGroupId":"G-9D08642FB8CE8CFE",
    						"HostGroupName":"Core instance group",
    						"HostGroupType":"CORE",
    						"InstanceType":"ecs.n4.2xlarge",
    						"MemoryCapacity":16,
    						"NodeCount":2,
    						"Nodes":{
    							"Node":[
    								{
    									"CreateTime":"1543804244000",
    									"DaemonInfos":{
    										"DaemonInfo":[]
    									},
    									"DiskInfos":{
    										"DiskInfo":[
    											{
    												"Device":"/dev/xvde",
    												"DiskId":"d-bp1568nnv4ev81672qe5",
    												"Size":80,
    												"Type":"data"
    											},
    											{
    												"Device":"/dev/xvdd",
    												"DiskId":"d-bp1967p3xp86y3wiudgq",
    												"Size":80,
    												"Type":"data"
    											},
    											{
    												"Device":"/dev/xvdc",
    												"DiskId":"d-bp11qsh4zwrwup8jm9rr",
    												"Size":80,
    												"Type":"data"
    											},
    											{
    												"Device":"/dev/xvdb",
    												"DiskId":"d-bp1d1juyl8z8aeuza9co",
    												"Size":80,
    												"Type":"data"
    											},
    											{
    												"Device":"/dev/xvda",
    												"DiskId":"d-bp1ftve3lzvpm16i37y1",
    												"Size":80,
    												"Type":"system"
    											}
    										]
    									},
    									"EmrExpiredTime":"null",
    									"ExpiredTime":"2099-12-31T15:59Z",
    									"InnerIp":"192.168.128.237",
    									"InstanceId":"i-bp1gyhphkjplgljrsw5f",
    									"PubIp":"",
    									"Status":"NORMAL",
    									"ZoneId":"cn-hangzhou-b"
    								},
    								{
    									"CreateTime":"1543804247000",
    									"DaemonInfos":{
    										"DaemonInfo":[]
    									},
    									"DiskInfos":{
    										"DiskInfo":[
    											{
    												"Device":"/dev/xvde",
    												"DiskId":"d-bp12zhcmd96101qhz8ko",
    												"Size":80,
    												"Type":"data"
    											},
    											{
    												"Device":"/dev/xvdd",
    												"DiskId":"d-bp13rttoh3l1gj84gcas",
    												"Size":80,
    												"Type":"data"
    											},
    											{
    												"Device":"/dev/xvdc",
    												"DiskId":"d-bp1cnruigppe54k6fx18",
    												"Size":80,
    												"Type":"data"
    											},
    											{
    												"Device":"/dev/xvdb",
    												"DiskId":"d-bp1967p3xp86y3wiudgr",
    												"Size":80,
    												"Type":"data"
    											},
    											{
    												"Device":"/dev/xvda",
    												"DiskId":"d-bp1ik6bdp8fpsr8zsebj",
    												"Size":80,
    												"Type":"system"
    											}
    										]
    									},
    									"EmrExpiredTime":"null",
    									"ExpiredTime":"2099-12-31T15:59Z",
    									"InnerIp":"192.168.128.238",
    									"InstanceId":"i-bp1ftve3lzvpm16hp7lp",
    									"PubIp":"",
    									"Status":"NORMAL",
    									"ZoneId":"cn-hangzhou-b"
    								}
    							]
    						}
    					}
    				]
    			},
    			"Id":"C-E331B8AC12BF5DB4",
    			"ImageId":"m-bp118knl07yk88y8s6cj",
    			"IoOptimized":true,
    			"LocalMetaDb":true,
    			"MasterNodeInService":2,
    			"MasterNodeTotal":0,
    			"Name":"df-test-safe",
    			"NetType":"vpc",
    			"RegionId":"cn-hangzhou",
    			"ResizeDiskEnable":true,
    			"RunningTime":1102,
    			"SecurityGroupId":"sg-bp1bvompzxgx7q0lg7oy",
    			"SecurityGroupName":"emr-default-securitygroup",
    			"ShowSoftwareInterface":false,
    			"SoftwareInfo":{
    				"ClusterType":"HADOOP",
    				"EmrVer":"EMR-3.16.0_pre",
    				"Softwares":{
    					"Software":[
    						{
    							"DisplayName":"HDFS",
    							"Name":"HDFS",
    							"OnlyDisplay":false,
    							"StartTpe":1,
    							"Version":"2.7.2-1.3.2"
    						},
    						{
    							"DisplayName":"YARN",
    							"Name":"YARN",
    							"OnlyDisplay":false,
    							"StartTpe":1,
    							"Version":"2.7.2-1.3.2"
    						},
    						{
    							"DisplayName":"Hive",
    							"Name":"HIVE",
    							"OnlyDisplay":false,
    							"StartTpe":1,
    							"Version":"2.3.3-1.0.3"
    						},
    						{
    							"DisplayName":"Ganglia",
    							"Name":"GANGLIA",
    							"OnlyDisplay":false,
    							"StartTpe":1,
    							"Version":"3.7.2"
    						},
    						{
    							"DisplayName":"Zookeeper",
    							"Name":"ZOOKEEPER",
    							"OnlyDisplay":false,
    							"StartTpe":1,
    							"Version":"3.4.13"
    						},
    						{
    							"DisplayName":"Spark",
    							"Name":"SPARK",
    							"OnlyDisplay":false,
    							"StartTpe":1,
    							"Version":"2.3.2-1.0.1"
    						},
    						{
    							"DisplayName":"HBase",
    							"Name":"HBASE",
    							"OnlyDisplay":false,
    							"StartTpe":1,
    							"Version":"1.1.1-1.0.2"
    						},
    						{
    							"DisplayName":"HUE",
    							"Name":"HUE",
    							"OnlyDisplay":false,
    							"StartTpe":1,
    							"Version":"4.1.0"
    						},
    						{
    							"DisplayName":"Zeppelin",
    							"Name":"ZEPPELIN",
    							"OnlyDisplay":false,
    							"StartTpe":1,
    							"Version":"0.8.0"
    						},
    						{
    							"DisplayName":"Tez",
    							"Name":"TEZ",
    							"OnlyDisplay":false,
    							"StartTpe":1,
    							"Version":"0.9.1-1.0.2"
    						},
    						{
    							"DisplayName":"Presto",
    							"Name":"PRESTO",
    							"OnlyDisplay":false,
    							"StartTpe":1,
    							"Version":"0.208"
    						},
    						{
    							"DisplayName":"Sqoop",
    							"Name":"SQOOP",
    							"OnlyDisplay":false,
    							"StartTpe":1,
    							"Version":"1.4.7-1.0.0"
    						},
    						{
    							"DisplayName":"Pig",
    							"Name":"PIG",
    							"OnlyDisplay":false,
    							"StartTpe":1,
    							"Version":"0.14.0"
    						},
    						{
    							"DisplayName":"ApacheDS",
    							"Name":"APACHEDS",
    							"OnlyDisplay":false,
    							"StartTpe":1,
    							"Version":"2.0.0"
    						},
    						{
    							"DisplayName":"HAS",
    							"Name":"HAS",
    							"OnlyDisplay":false,
    							"StartTpe":1,
    							"Version":"1.1.1"
    						},
    						{
    							"DisplayName":"Knox",
    							"Name":"KNOX",
    							"OnlyDisplay":false,
    							"StartTpe":1,
    							"Version":"0.13.0-0.0.3"
    						}
    					]
    				}
    			},
    			"StartTime":1543804234000,
    			"Status":"IDLE",
    			"TaskNodeInService":0,
    			"TaskNodeTotal":0,
    			"UserDefinedEmrEcsRole":"AliyunEmrEcsDefaultRole",
    			"UserId":125046002175****,
    			"VSwitchId":"vsw-bp11qjbyggil4pow064fg",
    			"VpcId":"vpc-bp1l4urd87xlh7i4bju4h",
    			"ZoneId":"cn-hangzhou-b"
    		},
    		"RequestId":"14E9C045-9B8D-4D1E-8D23-FC0027B6D947"
    	},
    	"requestId":"14E9C045-9B8D-4D1E-8D23-FC0027B6D947",
    	"successResponse":true
    }
    ```

-   Error response examples

    JSON format

    ```
    {
    	"code":"ClusterId.NotFound",
    	"message":"ClusterId [null] does not exist.",
    	"requestId":"93EC4B6D-76D2-4CB1-847D-C7A5E5E0206A",
    	"successResponse":false
    }
    ```


## Error codes {#section_a2w_lv4_dgb .section}

