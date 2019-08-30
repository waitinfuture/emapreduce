# DescribeClusterBasicInfo {#doc_api_Emr_DescribeClusterBasicInfo .reference}

调用DescribeClusterBasicInfo接口查询已创建的集群实例。

## 调试 {#api_explorer .section}

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=DescribeClusterBasicInfo&type=RPC&version=2016-04-08)

## 请求参数 {#parameters .section}

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DescribeClusterBasicInfo|系统规定参数。取值：DescribeClusterBasicInfo

 |
|ClusterId|String|是|C-0EF9B0EC8564\*\*\*\*|目标集群的业务ID。

 |
|RegionId|String|是|cn-huhehaote|集群实例所在的地域，您可以通过调用[DescribeRegions](~~25609~~)接口查询地域ID。

 |
|AccessKeyId|String|否|LTA\*\*\*\*u7gTm\*\*\*\*|阿里云的AccessKey ID，标识访问者身份。

 |

## 返回数据 {#resultMapping .section}

|名称|类型|示例值|描述|
|--|--|---|--|
|ClusterInfo| | |集群详情信息。

 |
|AccessInfo| | |集群的访问地址信息。

 |
|ZKLinks| | |ZK的链接地址信息列表。

 |
|Link|String|192.168.\*.\*|ZK的访问链接地址。

 |
|Port|String|2181|ZK的端口。

 |
|AutoScalingAllowed|Boolean|true|集群是否支持弹性伸缩。

 |
|AutoScalingByLoadAllowed|Boolean|true|集群是否支持按负载伸缩。

 |
|AutoScalingEnable|Boolean|true|集群当前是否开启了弹性伸缩。

 |
|AutoScalingSpotWithLimitAllowed|Boolean|true|弹性伸缩是否允许使用竞价实例（spotInstance）。

 |
|BootstrapActionList| | |集群的引导脚本信息。

 |
|Arg|String|name=NAME|引导脚本参数

 |
|Name|String|boot1|引导脚本名称

 |
|Path|String|oss://script/boot1|引导脚本的oss路径

 |
|BootstrapFailed|Boolean|false|引导操作的执行结果，**true**表示执行成功，**false**表示执行失败。

 |
|ChargeType|String|PostPaid|集群的付费类型，**PostPaid**表示是按量付费集群，**PrePaid**表示是包年包月集群。

 |
|ClusterId|String|C-02582A4A415E\*\*\*\*|集群的ID。

 |
|Configurations|String|""|保留字段，目前该字段无返回值，无需关注。

 |
|CoreNodeInService|Integer|0|已废弃字段，目前默认返回0，后续会移除该字段。

 |
|CoreNodeTotal|Integer|0|已废弃字段，目前默认返回0，后续会移除该字段。

 |
|CreateResource|String|ECM\_EMR|集群的部署方式，目前默认为**ECM\_EMR**。 部分比较旧的EMR主版本，例如1.3.0等创建的集群，该字段为**RAW\_EMR**。

 |
|CreateType|String|MANUAL|集群的创建方式，手动或自动，**MANUAL**表示为手动创建。

 |
|DepositType|String|HALF\_MANAGED|集群的托管类型，分为半托管和全托管，**HALF\_MANAGED**表示为半托管。

 |
|EasEnable|Boolean|false|集群的高安全模式，**false**表示非高安全集群。

 |
|ExpiredTime|Long|1564367083000|付费类型为包年包月的集群的过期时间。

 |
|FailReason| | |集群创建失败时的失败原因。

 |
|ErrorCode|String|ClusterId.NotFound|集群创建失败时的错误码

 |
|ErrorMsg|String|ClusterId \[null\] does not exist.|集群创建失败时的错误信息

 |
|RequestId|String|FDFFCB97-1609-469A-B153-F511CA9FC1F5|集群创建失败时，对应的后台请求ID

 |
|GatewayClusterIds|String|C-xxx|废弃字段，后续版本中将移除该字段。

 |
|GatewayClusterInfoList| | |当前集群关联的Gateway集群信息列表。

 |
|ClusterId|String|C-1AD4D95AF8B6\*\*\*\*|Gateway集群ID

 |
|ClusterName|String|test\_gateway|Gateway集群名称

 |
|Status|String|IDLE|Gateway集群状态

 |
|HighAvailabilityEnable|Boolean|true|集群是否为高可用集群。

 |
|HostPoolInfo| | |集群对应的机器池信息。当集群是通过机器池创建时，该字段会返回对应的机器池信息。

 |
|HpBizId|String|HP-xxx|机器池ID

 |
|HpName|String|test\_hp|机器池名称

 |
|ImageId|String|m-xxx|集群的节点使用的ECS镜像ID。

 |
|InstanceGeneration|String|ecs-2|废弃字段，后续版本中将移除该字段。

 |
|IoOptimized|Boolean|true|废弃字段，后续版本中将移除该字段。

 |
|LocalMetaDb|Boolean|true|集群是否使用的本地HIVE元数据库。**true**表示使用本地元数据库，**false**表示使用的统一元数据库。

 |
|LogEnable|Boolean|false|废弃字段，后续版本中将移除该字段。

 |
|LogPath|String|""|废弃字段，后续版本中将移除该字段。

 |
|MachineType|String|ECS|集群的节点类型，目前默认为ECS。

 |
|MasterNodeInService|Integer|0|废弃字段，后续版本中将移除该字段。

 |
|MasterNodeTotal|Integer|0|废弃字段，后续版本中将移除该字段。

 |
|MetaStoreType|String|local|集群的统一元数据类型。

 |
|Name|String|test\_cluster|集群名称。

 |
|NetType|String|vpc|集群的网络类型，**classic**或者**vpc**。

 |
|Period|Integer|0|废弃字段，后续版本中将移除该字段。

 |
|RegionId|String|cn-huhehaote-a|集群的RegionId信息。

 |
|RelateClusterId|String|C-xxx|废弃字段，后续版本中将移除该字段。

 |
|RelateClusterInfo| | |Gateway集群对应的主集群信息。

 |
|ClusterId|String|C-4A502AECB275\*\*\*\*|主集群的ID

 |
|ClusterName|String|test\_hadoop|主集群的集群名称

 |
|Status|String|IDLE|主集群的集群状态

 |
|ResizeDiskEnable|Boolean|true|当前集群是否支持磁盘扩容功能。

 |
|RunningTime|Integer|1583000|集群当前的运行时长。

 |
|SecurityGroupId|String|sg-xxx|集群的安全组ID。

 |
|SecurityGroupName|String|test\_groupName|集群的安全组名称。

 |
|ShowSoftwareInterface|Boolean|false|废弃字段，后续版本中将移除该字段。

 |
|SoftwareInfo| | |集群上部署的软件服务信息。

 |
|ClusterType|String|HADOOP|集群类型

 |
|EmrVer|String|EMR-3.21.0|EMR主版本信息

 |
|Softwares| | |服务信息列表

 |
|DisplayName|String|HDFS|服务的显示名称

 |
|Name|String|HDFS|服务名称

 |
|OnlyDisplay|Boolean|false|废弃字段，后续版本中将移除该字段

 |
|StartTpe|Integer|1|废弃字段，后续版本中将移除该字段

 |
|Version|String|2.8.5|服务的版本信息

 |
|StartTime|Long|1564367083000|集群的开始时间。

 |
|Status|String|IDLE|集群的状态。

 |
|StopTime|Long|1564367083000|停止时间。

 |
|TaskNodeInService|Integer|0|废弃字段，后续版本中将移除该字段。

 |
|TaskNodeTotal|Integer|0|废弃字段，后续版本中将移除该字段。

 |
|UserDefinedEmrEcsRole|String|AliyunEmrEcsDefaultRole|E-MapReduce的服务角色名称。

 |
|UserId|String|11131321311\*\*\*\*|用户ID。

 |
|VSwitchId|String|vsw-xxx|交换机ID。

 |
|VpcId|String|vpc-xxx|VPC的ID。

 |
|ZoneId|String|cn-huhehaote-a|集群的ZoneID信息。

 |
|RequestId|String|BB81FF02-2FE0-4DB0-A04B-A0876D21EDD7|本次请求的ID。

 |

## 示例 {#demo .section}

请求示例

``` {#request_demo}

http(s)://[Endpoint]/?Action=DescribeClusterBasicInfo
&ClusterId=C-0EF9B0EC8564****
&RegionId=cn-huhehaote
&<公共请求参数>

```

正常返回示例

`XML` 格式

``` {#xml_return_success_demo}
<DescribeClusterBasicInfoResponse>	
	  <ClusterInfo>
		    <ImageId>m-hp37dmahkdx7h2b5qunn</ImageId>
		    <SecurityGroupId>sg-hp360xqw5l179aty****</SecurityGroupId>
		    <DepositType>HALF_MANAGED</DepositType>
		    <CoreNodeInService>0</CoreNodeInService>
		    <ZoneId>cn-huhehaote-a</ZoneId>
		    <AutoScalingSpotWithLimitAllowed>false</AutoScalingSpotWithLimitAllowed>
		    <IoOptimized>true</IoOptimized>
		    <TaskNodeInService>0</TaskNodeInService>
		    <MachineType>ECS</MachineType>
		    <AutoScalingAllowed>false</AutoScalingAllowed>
		    <ShowSoftwareInterface>false</ShowSoftwareInterface>
		    <ResizeDiskEnable>false</ResizeDiskEnable>
		    <SecurityGroupName>sg-hp360xqw5l179aty****</SecurityGroupName>
		    <BootstrapActionList></BootstrapActionList>
		    <CreateResource>ECM_EMR</CreateResource>
		    <RegionId>cn-huhehaote</RegionId>
		    <GatewayClusterInfoList></GatewayClusterInfoList>
		    <NetType>vpc</NetType>
		    <MasterNodeInService>0</MasterNodeInService>
		    <AutoScalingByLoadAllowed>true</AutoScalingByLoadAllowed>
		    <SoftwareInfo>
			      <Softwares>
				        <Software>
					          <Name>HDFS</Name>
					          <OnlyDisplay>false</OnlyDisplay>
					          <Version>2.8.5</Version>
					          <DisplayName>HDFS</DisplayName>
					          <StartTpe>1</StartTpe>
				        </Software>
				        <Software>
					          <Name>YARN</Name>
					          <OnlyDisplay>false</OnlyDisplay>
					          <Version>2.8.5</Version>
					          <DisplayName>YARN</DisplayName>
					          <StartTpe>1</StartTpe>
				        </Software>
				        <Software>
					          <Name>HIVE</Name>
					          <OnlyDisplay>false</OnlyDisplay>
					          <Version>3.1.1</Version>
					          <DisplayName>Hive</DisplayName>
					          <StartTpe>1</StartTpe>
				        </Software>
				        <Software>
					          <Name>GANGLIA</Name>
					          <OnlyDisplay>false</OnlyDisplay>
					          <Version>3.7.2</Version>
					          <DisplayName>Ganglia</DisplayName>
					          <StartTpe>1</StartTpe>
				        </Software>
				        <Software>
					          <Name>ZOOKEEPER</Name>
					          <OnlyDisplay>false</OnlyDisplay>
					          <Version>3.4.13</Version>
					          <DisplayName>Zookeeper</DisplayName>
					          <StartTpe>1</StartTpe>
				        </Software>
				        <Software>
					          <Name>SPARK</Name>
					          <OnlyDisplay>false</OnlyDisplay>
					          <Version>2.4.3</Version>
					          <DisplayName>Spark</DisplayName>
					          <StartTpe>1</StartTpe>
				        </Software>
				        <Software>
					          <Name>HBASE</Name>
					          <OnlyDisplay>false</OnlyDisplay>
					          <Version>1.4.9</Version>
					          <DisplayName>HBase</DisplayName>
					          <StartTpe>1</StartTpe>
				        </Software>
				        <Software>
					          <Name>HUE</Name>
					          <OnlyDisplay>false</OnlyDisplay>
					          <Version>4.4.0</Version>
					          <DisplayName>HUE</DisplayName>
					          <StartTpe>1</StartTpe>
				        </Software>
				        <Software>
					          <Name>ZEPPELIN</Name>
					          <OnlyDisplay>false</OnlyDisplay>
					          <Version>0.8.1</Version>
					          <DisplayName>Zeppelin</DisplayName>
					          <StartTpe>1</StartTpe>
				        </Software>
				        <Software>
					          <Name>OOZIE</Name>
					          <OnlyDisplay>false</OnlyDisplay>
					          <Version>5.1.0</Version>
					          <DisplayName>Oozie</DisplayName>
					          <StartTpe>1</StartTpe>
				        </Software>
				        <Software>
					          <Name>TEZ</Name>
					          <OnlyDisplay>false</OnlyDisplay>
					          <Version>0.9.1</Version>
					          <DisplayName>Tez</DisplayName>
					          <StartTpe>1</StartTpe>
				        </Software>
				        <Software>
					          <Name>PHOENIX</Name>
					          <OnlyDisplay>false</OnlyDisplay>
					          <Version>4.14.1</Version>
					          <DisplayName>Phoenix</DisplayName>
					          <StartTpe>1</StartTpe>
				        </Software>
				        <Software>
					          <Name>PRESTO</Name>
					          <OnlyDisplay>false</OnlyDisplay>
					          <Version>0.213</Version>
					          <DisplayName>Presto</DisplayName>
					          <StartTpe>1</StartTpe>
				        </Software>
				        <Software>
					          <Name>SQOOP</Name>
					          <OnlyDisplay>false</OnlyDisplay>
					          <Version>1.4.7</Version>
					          <DisplayName>Sqoop</DisplayName>
					          <StartTpe>1</StartTpe>
				        </Software>
				        <Software>
					          <Name>PIG</Name>
					          <OnlyDisplay>false</OnlyDisplay>
					          <Version>0.14.0</Version>
					          <DisplayName>Pig</DisplayName>
					          <StartTpe>1</StartTpe>
				        </Software>
				        <Software>
					          <Name>STORM</Name>
					          <OnlyDisplay>false</OnlyDisplay>
					          <Version>1.2.2</Version>
					          <DisplayName>Storm</DisplayName>
					          <StartTpe>1</StartTpe>
				        </Software>
				        <Software>
					          <Name>IMPALA</Name>
					          <OnlyDisplay>false</OnlyDisplay>
					          <Version>2.12.2</Version>
					          <DisplayName>Impala</DisplayName>
					          <StartTpe>1</StartTpe>
				        </Software>
				        <Software>
					          <Name>FLINK</Name>
					          <OnlyDisplay>false</OnlyDisplay>
					          <Version>1.7.2</Version>
					          <DisplayName>Flink</DisplayName>
					          <StartTpe>1</StartTpe>
				        </Software>
				        <Software>
					          <Name>RANGER</Name>
					          <OnlyDisplay>false</OnlyDisplay>
					          <Version>1.2.0</Version>
					          <DisplayName>Ranger</DisplayName>
					          <StartTpe>1</StartTpe>
				        </Software>
				        <Software>
					          <Name>KNOX</Name>
					          <OnlyDisplay>false</OnlyDisplay>
					          <Version>1.1.0</Version>
					          <DisplayName>Knox</DisplayName>
					          <StartTpe>1</StartTpe>
				        </Software>
				        <Software>
					          <Name>SUPERSET</Name>
					          <OnlyDisplay>false</OnlyDisplay>
					          <Version>0.28.1</Version>
					          <DisplayName>Superset</DisplayName>
					          <StartTpe>1</StartTpe>
				        </Software>
				        <Software>
					          <Name>BIGBOOT</Name>
					          <OnlyDisplay>false</OnlyDisplay>
					          <Version>1.0.0</Version>
					          <DisplayName>Bigboot</DisplayName>
					          <StartTpe>1</StartTpe>
				        </Software>
				        <Software>
					          <Name>LIVY</Name>
					          <OnlyDisplay>false</OnlyDisplay>
					          <Version>0.6.0</Version>
					          <DisplayName>LIVY</DisplayName>
					          <StartTpe>1</StartTpe>
				        </Software>
				        <Software>
					          <Name>SMARTDATA</Name>
					          <OnlyDisplay>false</OnlyDisplay>
					          <Version>1.0.0</Version>
					          <DisplayName>SmartData</DisplayName>
					          <StartTpe>1</StartTpe>
				        </Software>
				        <Software>
					          <Name>FLUME</Name>
					          <OnlyDisplay>false</OnlyDisplay>
					          <Version>1.8.0</Version>
					          <DisplayName>FLUME</DisplayName>
					          <StartTpe>1</StartTpe>
				        </Software>
				        <Software>
					          <Name>ANALYTICS-ZOO</Name>
					          <OnlyDisplay>false</OnlyDisplay>
					          <Version>0.5.0</Version>
					          <DisplayName>analytics-zoo</DisplayName>
					          <StartTpe>1</StartTpe>
				        </Software>
				        <Software>
					          <Name>APACHEDS</Name>
					          <OnlyDisplay>false</OnlyDisplay>
					          <Version>2.0.0</Version>
					          <DisplayName>ApacheDS</DisplayName>
					          <StartTpe>1</StartTpe>
				        </Software>
			      </Softwares>
			      <EmrVer>EMR-3.21.1</EmrVer>
			      <ClusterType>HADOOP</ClusterType>
		    </SoftwareInfo>
		    <CreateType>MANUAL</CreateType>
		    <VSwitchId>vsw-hp333ghqgdxkk3djb****</VSwitchId>
		    <VpcId>vpc-hp3fawn3tu8ny436o****</VpcId>
		    <TaskNodeTotal>0</TaskNodeTotal>
		    <BootstrapFailed>false</BootstrapFailed>
		    <CoreNodeTotal>0</CoreNodeTotal>
		    <StartTime>1564132024000</StartTime>
		    <MasterNodeTotal>0</MasterNodeTotal>
		    <ChargeType>PostPaid</ChargeType>
		    <Configurations></Configurations>
		    <UserId>107992689699****</UserId>
		    <Name>EMR-API-test</Name>
		    <EasEnable>false</EasEnable>
		    <AutoScalingEnable>false</AutoScalingEnable>
		    <Status>IDLE</Status>
		    <LocalMetaDb>true</LocalMetaDb>
		    <HighAvailabilityEnable>false</HighAvailabilityEnable>
		    <ClusterId>C-02582A4A415E****</ClusterId>
		    <RunningTime>1004</RunningTime>
		    <UserDefinedEmrEcsRole>AliyunEmrEcsDefaultRole</UserDefinedEmrEcsRole>
	  </ClusterInfo>
	  <RequestId>BB81FF02-2FE0-4DB0-A04B-A0876D21EDD7</RequestId>
</DescribeClusterBasicInfoResponse>
```

`JSON` 格式

``` {#json_return_success_demo}
{
	"ClusterInfo":{
		"SecurityGroupId":"sg-hp360xqw5l179aty****",
		"ImageId":"m-hp37dmahkdx7h2b5qunn",
		"MasterNodeInService":0,
		"AutoScalingByLoadAllowed":true,
		"SoftwareInfo":{
			"Softwares":{
				"Software":[
					{
						"Name":"HDFS",
						"OnlyDisplay":false,
						"DisplayName":"HDFS",
						"Version":"2.8.5",
						"StartTpe":1
					},
					{
						"Name":"YARN",
						"OnlyDisplay":false,
						"DisplayName":"YARN",
						"Version":"2.8.5",
						"StartTpe":1
					},
					{
						"Name":"HIVE",
						"OnlyDisplay":false,
						"DisplayName":"Hive",
						"Version":"3.1.1",
						"StartTpe":1
					},
					{
						"Name":"GANGLIA",
						"OnlyDisplay":false,
						"DisplayName":"Ganglia",
						"Version":"3.7.2",
						"StartTpe":1
					},
					{
						"Name":"ZOOKEEPER",
						"OnlyDisplay":false,
						"DisplayName":"Zookeeper",
						"Version":"3.4.13",
						"StartTpe":1
					},
					{
						"Name":"SPARK",
						"OnlyDisplay":false,
						"DisplayName":"Spark",
						"Version":"2.4.3",
						"StartTpe":1
					},
					{
						"Name":"HBASE",
						"OnlyDisplay":false,
						"DisplayName":"HBase",
						"Version":"1.4.9",
						"StartTpe":1
					},
					{
						"Name":"HUE",
						"OnlyDisplay":false,
						"DisplayName":"HUE",
						"Version":"4.4.0",
						"StartTpe":1
					},
					{
						"Name":"ZEPPELIN",
						"OnlyDisplay":false,
						"DisplayName":"Zeppelin",
						"Version":"0.8.1",
						"StartTpe":1
					},
					{
						"Name":"OOZIE",
						"OnlyDisplay":false,
						"DisplayName":"Oozie",
						"Version":"5.1.0",
						"StartTpe":1
					},
					{
						"Name":"TEZ",
						"OnlyDisplay":false,
						"DisplayName":"Tez",
						"Version":"0.9.1",
						"StartTpe":1
					},
					{
						"Name":"PHOENIX",
						"OnlyDisplay":false,
						"DisplayName":"Phoenix",
						"Version":"4.14.1",
						"StartTpe":1
					},
					{
						"Name":"PRESTO",
						"OnlyDisplay":false,
						"DisplayName":"Presto",
						"Version":"0.213",
						"StartTpe":1
					},
					{
						"Name":"SQOOP",
						"OnlyDisplay":false,
						"DisplayName":"Sqoop",
						"Version":"1.4.7",
						"StartTpe":1
					},
					{
						"Name":"PIG",
						"OnlyDisplay":false,
						"DisplayName":"Pig",
						"Version":"0.14.0",
						"StartTpe":1
					},
					{
						"Name":"STORM",
						"OnlyDisplay":false,
						"DisplayName":"Storm",
						"Version":"1.2.2",
						"StartTpe":1
					},
					{
						"Name":"IMPALA",
						"OnlyDisplay":false,
						"DisplayName":"Impala",
						"Version":"2.12.2",
						"StartTpe":1
					},
					{
						"Name":"FLINK",
						"OnlyDisplay":false,
						"DisplayName":"Flink",
						"Version":"1.7.2",
						"StartTpe":1
					},
					{
						"Name":"RANGER",
						"OnlyDisplay":false,
						"DisplayName":"Ranger",
						"Version":"1.2.0",
						"StartTpe":1
					},
					{
						"Name":"KNOX",
						"OnlyDisplay":false,
						"DisplayName":"Knox",
						"Version":"1.1.0",
						"StartTpe":1
					},
					{
						"Name":"SUPERSET",
						"OnlyDisplay":false,
						"DisplayName":"Superset",
						"Version":"0.28.1",
						"StartTpe":1
					},
					{
						"Name":"BIGBOOT",
						"OnlyDisplay":false,
						"DisplayName":"Bigboot",
						"Version":"1.0.0",
						"StartTpe":1
					},
					{
						"Name":"LIVY",
						"OnlyDisplay":false,
						"DisplayName":"LIVY",
						"Version":"0.6.0",
						"StartTpe":1
					},
					{
						"Name":"SMARTDATA",
						"OnlyDisplay":false,
						"DisplayName":"SmartData",
						"Version":"1.0.0",
						"StartTpe":1
					},
					{
						"Name":"FLUME",
						"OnlyDisplay":false,
						"DisplayName":"FLUME",
						"Version":"1.8.0",
						"StartTpe":1
					},
					{
						"Name":"ANALYTICS-ZOO",
						"OnlyDisplay":false,
						"DisplayName":"analytics-zoo",
						"Version":"0.5.0",
						"StartTpe":1
					},
					{
						"Name":"APACHEDS",
						"OnlyDisplay":false,
						"DisplayName":"ApacheDS",
						"Version":"2.0.0",
						"StartTpe":1
					}
				]
			},
			"EmrVer":"EMR-3.21.1",
			"ClusterType":"HADOOP"
		},
		"DepositType":"HALF_MANAGED",
		"CoreNodeInService":0,
		"ZoneId":"cn-huhehaote-a",
		"CreateType":"MANUAL",
		"VSwitchId":"vsw-hp333ghqgdxkk3djb****",
		"AutoScalingSpotWithLimitAllowed":false,
		"VpcId":"vpc-hp3fawn3tu8ny436o****",
		"IoOptimized":true,
		"TaskNodeTotal":0,
		"TaskNodeInService":0,
		"BootstrapFailed":false,
		"CoreNodeTotal":0,
		"StartTime":1564132024000,
		"MasterNodeTotal":0,
		"ChargeType":"PostPaid",
		"MachineType":"ECS",
		"Configurations":"",
		"AutoScalingAllowed":false,
		"UserId":"107992689699****",
		"ShowSoftwareInterface":false,
		"AutoScalingEnable":false,
		"EasEnable":false,
		"Name":"EMR-API-test",
		"Status":"IDLE",
		"ResizeDiskEnable":false,
		"HighAvailabilityEnable":false,
		"LocalMetaDb":true,
		"SecurityGroupName":"sg-hp360xqw5l179aty****",
		"BootstrapActionList":{
			"BootstrapAction":[]
		},
		"ClusterId":"C-02582A4A415E****",
		"CreateResource":"ECM_EMR",
		"RegionId":"cn-huhehaote",
		"RunningTime":1004,
		"GatewayClusterInfoList":{
			"GatewayClusterInfo":[]
		},
		"NetType":"vpc",
		"UserDefinedEmrEcsRole":"AliyunEmrEcsDefaultRole"
	},
	"RequestId":"BB81FF02-2FE0-4DB0-A04B-A0876D21EDD7"
}
```

## 错误码 { .section}

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|403|Params.Illegal|The specified parameters are wrongly formed..|指定参数格式错误|
|404|ClusterId.NotFound|ClusterId \[%s\] does not exist.|Cluster Id不存在，确认集群的Cluster Id|
|403|User.OtherUserResource.NotAllow|It is not allowed to operate other user's resource|不能操作其它用户的资源|
|500|InternalError|The request processing has failed due to some unknown error.|内部错误，请提工单|

访问[错误中心](https://error-center.alibabacloud.com/status/product/Emr)查看更多错误码。

