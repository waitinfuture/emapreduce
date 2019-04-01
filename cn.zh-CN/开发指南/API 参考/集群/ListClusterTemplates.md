# ListClusterTemplates {#concept_lsy_rmw_dgb .concept}

查询集群模版列表参数以及示例。

## 请求参数 {#section_pdw_lv4_dgb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|PageNumber|Integer|是|1|页数，从1开始|
|PageSize|Integer|是|10|分页大小|
|AccessKeyId|String|否|LTAI8ljWyu7y\*\*\*\*|AccessKeyId|
|BizId|String|否|CT-4A6799A79D73385B|集群模版ID|
|RegionId|String|否|cn-hangzhou|区域ID|

## 返回参数 {#section_vdw_lv4_dgb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|BF4FBAC6-B03E-4BFB-B6DB-EB53C34F2E22|请求ID|
|TotalCount|Integer|12|集群模版总数|
|PageNumber|Integer|1|页数，从1开始|
|PageSize|Integer|10|分页大小|
|TemplateInfoList| | |集群模版列表|
|Id|String|CT-4A6799A79D73385B|集群模版的ID|
|TemplateName|String|template\_name\_2|集群模版名|
|LogEnable|Boolean|true|是否开启OSS 日志|
|LogPath|String|oss//bucketname/path|OSS 文件路径|
|UserId|String|125046002175\*\*\*\*|创建用户ID|
|UserDefinedEmrEcsRole|String|AliyunEmrEcsDefaultRole|用于 ECS 调用的 EMR 权限名|
|MasterNodeTotal|Integer|1|Master 节点数量|
|VpcId|String|vpc-bp1l4urd87xlh7i4bju4h|VPC ID|
|VSwitchId|String|vsw-bp10tvjyc77psy0z5h0ni|交换机 ID|
|NetType|String|vpc|网络类型|
|IoOptimized|Boolean|true|是否 IO 优化|
|InstanceGeneration|String|ecs-3|ECS 实例分代|
|HighAvailabilityEnable|Boolean|true|是否高可用集群|
|EasEnable|Boolean|true|是否高安全|
|GmtCreate|Long|1543765033000|创建时间|
|GmtModified|Long|1543765033000|修改时间|
|ZoneId|String|cn-hangzhou-b|可用区|
|ClusterType|String|HADOOP|集群类型|
|SecurityGroupId|String|sg-bp1id7ajv83kmqwq2isx|安全组 ID|
|SecurityGroupName|String|emr\_sg|需要新创建的安全组的名字|
|Configurations|String|\[\{"classification": "core-site","properties": \{"fs.trash.interval": "61"\}\},\{"classification": "hadoop-log4j","properties": \{"hadoop.log.file": "hadoop1.log","hadoop.root.logger": "INFO","a.b.c": "ABC"\}\}\]|软件自定义配置。（集群启动前，可以指定一个 json 文件修改软件配置。）|
|AllowNotebook|Boolean|true|保留字段|
|CreateSource|String|2|模版通过何种方式创建|
|UseLocalMetaDb|Boolean|false|是否使用 Hive 本地元数据|
|SshEnable|Boolean|true|是否开启 SSH|
|IsOpenPublicIp|Boolean|true|是否开放公网 IP|
|DepositType|String|HALF\_MANAGED|托管类型|
|MachineType|String|ECS|机器类型|
|UseCustomHiveMetaDb|Boolean|false|保留字段|
|InitCustomHiveMetaDb|Boolean|true|如果指定为 true，Hive 的hive-site 配置项init.meta.db 会被设置为 true|
|BootstrapActionList| | |引导操作列表|
|Name|String|create|操作名|
|Path|String|oss://bucket/path|路径|
|Arg|String|---a|参数|
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
    /?PageNumber=1
    &PageSize=10
    &AccessKeyId=LTAI8ljWyu7y****
    &BizId=CT-4A6799A79D73385B
    &RegionId=cn-hangzhou
    &<公共请求参数>
    ```

-   正常返回示例

    JSON 格式

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
    		"userId":125046002175****,
    		"vSwitchId":"vsw-bp10tvjyc77psy0z5h0ni",
    		"vpcId":"vpc-bp1l4urd87xlh7i4bju4h",
    		"zoneId":"cn-hangzhou-b"
    	}],
    	"totalCount":1
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

[查看本产品错误码](https://error-center.alibabacloud.com/status/product/Emr)

