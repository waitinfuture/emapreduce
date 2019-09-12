# ListClusters {#doc_api_Emr_ListClusters .reference}

调用 ListClusters 接口分页查询集群列表。

## 调试 {#api_explorer .section}

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=ListClusters&type=RPC&version=2016-04-08)

## 请求参数 {#parameters .section}

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ListClusters|系统规定参数。取值：ListClusters。

 |
|RegionId|String|是|cn-hangzhou|地域ID。

 |
|AccessKeyId|String|否|LTAI8ljWyu7y\*\*\*\*|阿里云AccessKey ID信息，用于标识访问者身份。

 |
|ClusterTypeList.N|RepeatList|否|\["HADOOP","KAFKA"\]|集群类型列表。

 |
|CreateType|String|否|ON-DEMAND|集群创建类型。可选值：

 -   ON-DEMAND。
-   MANUAL。

 |
|DefaultStatus|Boolean|否|true|是否查询默认状态为初始化中、等待构建、空闲、运行中、释放中、创建失败的集群。

 |
|DepositType|String|否|HALF\_MANAGED|托管类型，标识集群是全托管还是半托管。枚举值：

 -   HALF\_MANAGED。
-   FULLY\_MANAGED。

 |
|IsDesc|Boolean|否|false|是否倒序排列。

 |
|MachineType|String|否|ECS|机器类型。

 |
|PageNumber|Integer|否|1|分页分数，从1开始。

 |
|PageSize|Integer|否|10|分页大小。

 |
|StatusList.N|RepeatList|否|\["CREATING","IDLE"\]|状态列表。

 |

## 返回数据 {#resultMapping .section}

|名称|类型|示例值|描述|
|--|--|---|--|
|Clusters| | |集群列表。

 |
|ClusterInfo| | |集群列表。

 |
|ChargeType|String|PostPaid|付费类型。

 |
|CreateResource|String|ECM\_EMR|ECM\_EMR。

 |
|CreateTime|Long|1542784048000|创建时间。

 |
|DepositType|String|HALF\_MANAGED|托管类型，标识集群是全托管还是半托管。枚举值：HALF\_MANAGED、FULLY\_MANAGED。

 |
|ExpiredTime|Long|1542784048000|包年包月集群超时时间。

 |
|FailReason| | |创建失败原因。

 |
|ErrorCode|String|InvalidImageId.NotFound|错误码。

 |
|ErrorMsg|String|The specified ImageId does not exist.|错误信息。

 |
|RequestId|String|B8DC3A91-3953-4444-92BB-DBC29C47EC1A|请求ID。

 |
|HasUncompletedOrder|Boolean|false|是否有未完成的订单。

 |
|Id|String|C-010A704DA760\*\*\*\*|集群ID。

 |
|MachineType|String|ECS|集群的主机类型，目前默认为ECS。

 |
|MetaStoreType|String|local|统一元数据类型。

 |
|Name|String|cluster\_name|集群名。

 |
|OrderList|String|0|订单列表。

 |
|OrderTaskInfo| | |保留字段，订单任务信息。

 |
|CurrentCount|Integer|0|保留字段。

 |
|OrderIdList|String|0|保留字段。

 |
|TargetCount|Integer|0|保留字段。

 |
|Period|Integer|10|包年包月时间（可选包月数量有：1、2、3、4、5、6、7、8、9、12、24、36。）

 |
|RunningTime|Integer|2345|已运行时间（秒）。

 |
|Status|String|IDEL|集群状态。

 |
|Type|String|HADOOP|集群类型。

 |
|PageNumber|Integer|1|分页页数。

 |
|PageSize|Integer|10|分页大小。

 |
|RequestId|String|BF4FBAC6-B03E-4BFB-B6DB-EB53C34F2E22|请求ID。

 |
|TotalCount|Integer|12|查询总数。

 |

## 示例 {#demo .section}

请求示例

``` {#request_demo}

http(s)://[Endpoint]/?Action=ListClusters
&RegionId=cn-hangzhou
&<公共请求参数>

```

正常返回示例

`XML` 格式

``` {#xml_return_success_demo}
<ListClustersResponse>
	  <clusters>
		    <chargeType>PostPaid</chargeType>
		    <createResource>ECM_EMR</createResource>
		    <createTime>1542784048000</createTime>
		    <failReason></failReason>
		    <hasUncompletedOrder>false</hasUncompletedOrder>
		    <id>C-010A704DA760****</id>
		    <name>jy_test_d1_test</name>
		    <orderTaskInfo></orderTaskInfo>
		    <runningTime>3138</runningTime>
		    <status>RELEASED</status>
		    <type>HADOOP</type>
	  </clusters>
	  <clusters>
		    <chargeType>PostPaid</chargeType>
		    <createResource>ECM_EMR</createResource>
		    <createTime>1538107586000</createTime>
		    <failReason></failReason>
		    <hasUncompletedOrder>false</hasUncompletedOrder>
		    <id>C-B9712209060C****</id>
		    <name>intelligence-313-yp</name>
		    <orderTaskInfo></orderTaskInfo>
		    <runningTime>2069400</runningTime>
		    <status>RELEASED</status>
		    <type>HADOOP</type>
	  </clusters>
	  <clusters>
		    <chargeType>PostPaid</chargeType>
		    <createResource>ECM_EMR</createResource>
		    <createTime>1536546078000</createTime>
		    <failReason></failReason>
		    <hasUncompletedOrder>false</hasUncompletedOrder>
		    <id>C-4CD9EBBD6B23****</id>
		    <name>mg-storm</name>
		    <orderTaskInfo></orderTaskInfo>
		    <runningTime>1382155</runningTime>
		    <status>RELEASED</status>
		    <type>HADOOP</type>
	  </clusters>
	  <clusters>
		    <chargeType>PostPaid</chargeType>
		    <createResource>ECM_EMR</createResource>
		    <createTime>1535363759000</createTime>
		    <failReason></failReason>
		    <hasUncompletedOrder>false</hasUncompletedOrder>
		    <id>C-75D6EE95D722****</id>
		    <name>df-3101-upgrade-test</name>
		    <orderTaskInfo></orderTaskInfo>
		    <runningTime>676284</runningTime>
		    <status>RELEASED</status>
		    <type>HADOOP</type>
	  </clusters>
	  <clusters>
		    <chargeType>PostPaid</chargeType>
		    <createResource>ECM_EMR</createResource>
		    <createTime>1534492361000</createTime>
		    <failReason>
			      <errorCode>InvalidImageId.NotFound</errorCode>
			      <errorMsg>The specified ImageId does not exist.</errorMsg>
			      <requestId>B8DC3A91-3953-4444-92BB-DBC29C47EC1A</requestId>
		    </failReason>
		    <hasUncompletedOrder>false</hasUncompletedOrder>
		    <id>C-5EF0F3C257B****</id>
		    <name>测试集群1</name>
		    <orderTaskInfo></orderTaskInfo>
		    <runningTime>0</runningTime>
		    <status>CREATE_FAILED</status>
		    <type>HADOOP</type>
	  </clusters>
	  <pageNumber>1</pageNumber>
	  <pageSize>5</pageSize>
	  <requestId>5443DB14-4641-4DFF-9226-4888EC5A2EA9</requestId>
	  <totalCount>11</totalCount>
</ListClustersResponse>
```

`JSON` 格式

``` {#json_return_success_demo}
{
	"clusters":[
		{
			"id":"C-010A704DA760****",
			"createTime":1542784048000,
			"hasUncompletedOrder":false,
			"createResource":"ECM_EMR",
			"status":"RELEASED",
			"orderTaskInfo":{},
			"name":"jy_test_d1_test",
			"failReason":{},
			"type":"HADOOP",
			"chargeType":"PostPaid",
			"runningTime":3138
		},
		{
			"id":"C-B9712209060C****",
			"createTime":1538107586000,
			"hasUncompletedOrder":false,
			"createResource":"ECM_EMR",
			"status":"RELEASED",
			"orderTaskInfo":{},
			"name":"intelligence-313-yp",
			"failReason":{},
			"type":"HADOOP",
			"chargeType":"PostPaid",
			"runningTime":2069400
		},
		{
			"id":"C-4CD9EBBD6B23****",
			"createTime":1536546078000,
			"hasUncompletedOrder":false,
			"createResource":"ECM_EMR",
			"status":"RELEASED",
			"orderTaskInfo":{},
			"name":"mg-storm",
			"failReason":{},
			"type":"HADOOP",
			"chargeType":"PostPaid",
			"runningTime":1382155
		},
		{
			"id":"C-75D6EE95D722****",
			"createTime":1535363759000,
			"hasUncompletedOrder":false,
			"createResource":"ECM_EMR",
			"status":"RELEASED",
			"orderTaskInfo":{},
			"name":"df-3101-upgrade-test",
			"failReason":{},
			"type":"HADOOP",
			"chargeType":"PostPaid",
			"runningTime":676284
		},
		{
			"id":"C-5EF0F3C257B1****",
			"createTime":1534492361000,
			"hasUncompletedOrder":false,
			"createResource":"ECM_EMR",
			"status":"CREATE_FAILED",
			"orderTaskInfo":{},
			"name":"测试集群1",
			"failReason":{
				"requestId":"B8DC3A91-3953-4444-92BB-DBC29C47EC1A",
				"errorCode":"InvalidImageId.NotFound",
				"errorMsg":"The specified ImageId does not exist."
			},
			"type":"HADOOP",
			"chargeType":"PostPaid",
			"runningTime":0
		}
	],
	"totalCount":11,
	"requestId":"5443DB14-4641-4DFF-9226-4888EC5A2EA9",
	"pageSize":5,
	"pageNumber":1
}
```

## 错误码 { .section}

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|403|Params.Illegal|The specified parameters are wrongly formed..|指定参数格式错误|
|403|User.OtherUserResource.NotAllow|It is not allowed to operate other user's resource|不能操作其它用户的资源|
|403|Invalid.Cluster.Status|Invalid cluster status %s in status list|指定集群状态不合法|
|403|Invalid.Cluster.Type|Invalid cluster type %s in cluster type list|指定集群类型不合法|
|403|InternalError|The request processing has failed due to some unknown error.|内部错误，请提工单|

访问[错误中心](https://error-center.aliyun.com/status/product/Emr)查看更多错误码。

