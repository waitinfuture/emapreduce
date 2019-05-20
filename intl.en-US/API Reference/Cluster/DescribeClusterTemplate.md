# DescribeClusterTemplate {#concept_vhx_bv4_dgb .concept}

You can call this opertion to view the template of a cluster.

## Request parameters {#section_pdw_lv4_dgb .section}

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|BizId|String|Yes|CT-35498C56B3F12002|The ID of the cluster template.|
|AccessKeyId|String|No|LTAI8ljWyu7yj9yG|The AccessKey ID.|

## Response parameters {#section_vdw_lv4_dgb .section}

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|F2BF8586-045D-4104-B00C-44A4AA619C05|The ID of the request.|
|TemplateInfo| | |The details of the cluster template.|
|Id|String|CT-4A6799A79D73385B|The ID of the cluster template.|
|TemplateName|String|template\_name\_2|The name of the cluster.|
|EmrVer|String|EMR-3.16.0|The version of EMR.|
|LogEnable|Boolean|true|Indicates whether OSS logging is enabled.|
|LogPath|String|oss//bucketname/path|OSS file path|
|UserId|String|12345|The ID of the user that created the cluster template.|
|UserDefinedEmrEcsRole|String|AliyunEmrEcsDefaultRole|The role that is assigned to EMR for calling ECS resources.|
|MasterNodeTotal|Integer|1|The number of master nodes.|
|VpcId|String|vpc-bp1l4urd87xlh7i4bju4h|The ID of the VPC.|
|VSwitchId|String|vsw-bp10tvjyc77psy0z5h0ni|The ID of the VSwitch.|
|NetType|String|vpc|The type of the network.|
|IoOptimized|Boolean|true|Indicates whether to enable IO optimization.|
|InstanceGeneration|String|ECS-3|The generation of the ECS instances.|
|HighAvailabilityEnable|Boolean|true|Indicates whether the cluster is high-availability.|
|EasEnable|Boolean|true|Indicates whether the cluster is high-security.|
|GmtCreate|Long|1543765033000|The creation time of the cluster template.|
|GmtModified|Long|1543765033000|The modification time of the cluster template.|
|ZoneId|String|cn-hangzhou-b|The region location ID.|
|ClusterType|String|HADOOP|The type of the cluster.|
|SecurityGroupId|String|sg-bp1id7ajv83kmqwq2isx|安全组ID|
|SecurityGroupName|String|emr\_sg|需要新创建的安全组的名字|
|Configurations|String|\[\{"classification": "core-site","properties": \{"fs.trash.interval": "61"\}\},\{"classification": "hadoop-log4j","properties": \{"hadoop.log.file": "hadoop1.log","hadoop.root.logger": "INFO","a.b.c": "ABC"\}\}\]|软件自定义配置。（ 集群启动前，可以指定一个json文件修改软件配置。）|
|AllowNotebook|Boolean|false|Indicates whether Notebook is allowed.|
|CreateSource|String|2|模版通过何种方式创建|
|UseLocalMetaDb|Boolean|true|是否使用Hive本地元数据|
|SshEnable|Boolean|true|是否开启SSH|
|IsOpenPublicIp|Boolean|true|是否开放公网IP|
|DepositType|String|FULL\_MANAGED|托管类型|
|MachineType|String|ECS|机器类型|
|UseCustomHiveMetaDb|Boolean|false|保留字段|
|InitCustomHiveMetaDb|Boolean|true|保留字段|
|BootstrapActionList| | |引导操作列表|
|Name|String|action\_name|操作名|
|Path|String|oss://bucket/path|The path where the script is stored.|
|Arg|String|--a|参数|
|HostGroupList| | |机器组列表|
|HostGroupId|String|0|保留字段|
|HostGroupName|String|主实例组|机器组名字|
|HostGroupType|String|MASTER|机器组类型|
|ChargeType|String|PostPaid|付费方式|
|Period|String|36|包年包月时间（天）|
|NodeCount|Integer|2|机器组节点数|
|InstanceType|String|ecs.mn4.2xlarge|机器组型号|
|DiskType|String|CLOUD\_SSD|机器组的数据盘类型|
|DiskCapacity|Integer|80|机器组的数据盘容量|
|DiskCount|Integer|4|机器组的数据盘数量|
|SysDiskType|String|CLOUD\_SSD|机器组的系统盘类型|
|SysDiskCapacity|Integer|40|机器组的系统盘容量|
|MultiInstanceTypes|String|ecs.sn1.xlarge,ecs.sn2.xlarge|多规格机器型号列表，逗号隔开|
|ConfigList| | |自定义配置项列表|
|ServiceName|String|YARN|自定义配置项服务名（大写）|
|FileName|String|yarn-site|自定义配置项所属文件名|
|ConfigKey|String|fs.trash.interval|自定义配置项的key|
|ConfigValue|String|60|自定义配置项的值|
|Encrypt|String|0|保留字段|
|Replace|String|0|保留字段|
|SoftwareInfoList|String|\["ZOOKEEPER"\]|可选软件列表|

## 示例 {#section_ydw_lv4_dgb .section}

-   请求示例

    ```
    /? BizId=CT-4A6799A79D73385B
    &AccessKeyId=LTAI8ljWyu7yj9yG
    &<公共请求参数>
    ```

-   正常返回示例

    JSON 格式

    ```
    {
    	"RequestId": "29a1d3b7661c-4fcd-8577-de93c6f6ca55 ",
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
    				"hostGroupName":"主实例组",
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
    				"hostGroupName":"核心实例组",
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
    		"userId":"1250460021754461",
    		"vSwitchId":"vsw-bp10tvjyc77psy0z5h0ni",
    		"vpcId":"vpc-bp1l4urd87xlh7i4bju4h",
    		"zoneId":"cn-hangzhou-b"
    	}
    }
    ```

-   异常返回示例

    JSON 格式

    ```
    {
    	"code":"RAM.Permission.NotAllow",
    	"message":"It is not allow to execute this operation,please use RAM to authorize!",
    	"requestId":"9AEDC439-1F63-491D-B8C6-9737C372BF3A",
    	"successResponse":false
    }
    ```


## 错误码 {#section_a2w_lv4_dgb .section}

