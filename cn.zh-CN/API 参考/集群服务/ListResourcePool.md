# ListResourcePool {#doc_api_Emr_ListResourcePool .reference}

调用 ListResourcePool 接口查询资源池列表。

## 调试 {#api_explorer .section}

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=ListResourcePool&type=RPC&version=2016-04-08)

## 请求参数 {#parameters .section}

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ListResourcePool|系统规定参数。取值：ListResourcePool。

 |
|ClusterId|String|是|C-EBD62A703A43\*\*\*\*|集群 ID

 |
|RegionId|String|是|cn-hangzhou|区域 ID

 |
|AccessKeyId|String|否|LTAI8ljWyu7y\*\*\*\*|AccessKeyId

 |
|PageNumber|Integer|否|100|分页查询的页码

 |
|PageSize|Integer|否|50|分页查询的每页记录数

 |
|PoolType|String|否|CAPACITY\_SCHEDULER|资源池类型，合法值：CAPACITY\_SCHEDULER,FAIR\_SCHEDULER

 |

## 返回数据 {#resultMapping .section}

|名称|类型|示例值|描述|
|--|--|---|--|
|PageNumber|Integer|1|分页序号

 |
|PageSize|Integer|20|分页大小

 |
|PoolInfoList| | |资源池列表

 |
|EcmResourcePool| | |资源池

 |
|Active|Boolean|true|激活状态

 |
|Id|Long|116|资源池 ID

 |
|Name|String|DEFAULT|资源池名称

 |
|Note|String|test|资源池描述

 |
|PoolType|String|CAPACITY\_SCHEDULER|资源池类型

 |
|UserId|String|1528342356764\*\*\*\*|用户 ID

 |
|YarnSiteConfig|String|null|YarnSite 配置

 |
|EcmResourcePoolConfigList| | |资源池配置列表

 |
|Category|String|QUEUE\_RESOURCE\_LIMIT|参数类别

 |
|ConfigKey|String|minimum-user-limit-percent|参数 Key

 |
|ConfigType|String|RESOURCE\_QUEUE\_CONFIG|参数类型

 |
|ConfigValue|String|0|参数值

 |
|Id|Long|2926|参数ID

 |
|Note|String|test|参数描述

 |
|Status|String|NORMAL|参数状态

 |
|QueueList| | |资源池队列列表

 |
|EcmResourcePoolConfigList| | |资源池配置

 |
|Category|String|QUEUE\_RESOURCE\_LIMIT|参数类别

 |
|ConfigKey|String|minimum-user-limit-percent|参数 Key

 |
|ConfigType|String|RESOURCE\_QUEUE\_CONFIG|参数类型

 |
|ConfigValue|String|0|参数值

 |
|Id|Long|2926|参数 ID

 |
|Note|String|test|参数描述

 |
|Status|String|NORMAL|参数状态

 |
|EcmResourceQueue| | |资源队列

 |
|Id|Long|2928|队列 ID

 |
|Leaf|Boolean|false|是否叶子节点

 |
|Name|String|DEFAULT2|队列名称

 |
|ParentQueueId|Long|116|父队列 ID

 |
|QualifiedName|String|default|QualifiedName

 |
|QueueType|String|null|队列类型

 |
|ResourcePoolId|Long|116|资源池 ID

 |
|Status|String|NORMAL|队列状态

 |
|UserId|String|1528342356764\*\*\*\*|用户 ID

 |
|RequestId|String|A544317F-4A60-4532-AC96-191B9D80420A|请求 ID

 |
|Total|Integer|10|总记录数

 |

## 示例 {#demo .section}

请求示例

``` {#request_demo}

http(s)://[Endpoint]/?Action=ListResourcePool
&ClusterId=C-EBD62A703A43****
&RegionId=cn-hangzhou
&<公共请求参数>

```

正常返回示例

`XML` 格式

``` {#xml_return_success_demo}
<ListResourcePool>
	  <code>200</code>
	  <data>
		    <PoolInfoList>
			      <PoolInfo>
				        <EcmResourcePoolConfigList></EcmResourcePoolConfigList>
				        <EcmResourcePool>
					          <Name>DEFAULT</Name>
					          <YarnSiteConfig></YarnSiteConfig>
					          <PoolType>CAPACITY_SCHEDULER</PoolType>
					          <Active>true</Active>
					          <Id>116</Id>
					          <Note></Note>
					          <UserId>152834231764****</UserId>
				        </EcmResourcePool>
				        <QueueList>
					          <Queue>
						            <EcmResourcePoolConfigList>
							              <EcmResourcePoolConfig>
								                <Status>NORMAL</Status>
								                <Category>QUEUE_RESOURCE_LIMIT</Category>
								                <ConfigKey>capacity</ConfigKey>
								                <ConfigValue>100</ConfigValue>
								                <Id>2925</Id>
								                <Note></Note>
								                <ConfigType>RESOURCE_QUEUE_CONFIG</ConfigType>
							              </EcmResourcePoolConfig>
							              <EcmResourcePoolConfig>
								                <Status>NORMAL</Status>
								                <Category>QUEUE_RESOURCE_LIMIT</Category>
								                <ConfigKey>minimum-user-limit-percent</ConfigKey>
								                <ConfigValue>0</ConfigValue>
								                <Id>2926</Id>
								                <Note></Note>
								                <ConfigType>RESOURCE_QUEUE_CONFIG</ConfigType>
							              </EcmResourcePoolConfig>
							              <EcmResourcePoolConfig>
								                <Status>NORMAL</Status>
								                <Category>QUEUE_RESOURCE_LIMIT</Category>
								                <ConfigKey>maximum-capacity</ConfigKey>
								                <ConfigValue>0</ConfigValue>
								                <Id>2927</Id>
								                <Note></Note>
								                <ConfigType>RESOURCE_QUEUE_CONFIG</ConfigType>
							              </EcmResourcePoolConfig>
							              <EcmResourcePoolConfig>
								                <Status>NORMAL</Status>
								                <Category>QUEUE_RESOURCE_LIMIT</Category>
								                <ConfigKey>user-limit-factor</ConfigKey>
								                <ConfigValue></ConfigValue>
								                <Id>2928</Id>
								                <Note></Note>
								                <ConfigType>RESOURCE_QUEUE_CONFIG</ConfigType>
							              </EcmResourcePoolConfig>
							              <EcmResourcePoolConfig>
								                <Status>NORMAL</Status>
								                <Category>QUEUE_RESOURCE_LIMIT</Category>
								                <ConfigKey>maximum-allocation-mb</ConfigKey>
								                <ConfigValue></ConfigValue>
								                <Id>2929</Id>
								                <Note></Note>
								                <ConfigType>RESOURCE_QUEUE_CONFIG</ConfigType>
							              </EcmResourcePoolConfig>
							              <EcmResourcePoolConfig>
								                <Status>NORMAL</Status>
								                <Category>QUEUE_RESOURCE_LIMIT</Category>
								                <ConfigKey>maximum-allocation-vcores</ConfigKey>
								                <ConfigValue></ConfigValue>
								                <Id>2930</Id>
								                <Note></Note>
								                <ConfigType>RESOURCE_QUEUE_CONFIG</ConfigType>
							              </EcmResourcePoolConfig>
							              <EcmResourcePoolConfig>
								                <Status>NORMAL</Status>
								                <Category>QUEUE_RESOURCE_LIMIT</Category>
								                <ConfigKey>maximum-applications</ConfigKey>
								                <ConfigValue></ConfigValue>
								                <Id>2931</Id>
								                <Note></Note>
								                <ConfigType>RESOURCE_QUEUE_CONFIG</ConfigType>
							              </EcmResourcePoolConfig>
							              <EcmResourcePoolConfig>
								                <Status>NORMAL</Status>
								                <Category>QUEUE_RESOURCE_LIMIT</Category>
								                <ConfigKey>maximum-am-resource-percent</ConfigKey>
								                <ConfigValue></ConfigValue>
								                <Id>2932</Id>
								                <Note></Note>
								                <ConfigType>RESOURCE_QUEUE_CONFIG</ConfigType>
							              </EcmResourcePoolConfig>
							              <EcmResourcePoolConfig>
								                <Status>NORMAL</Status>
								                <Category>QUEUE_RESOURCE_LIMIT</Category>
								                <ConfigKey>state</ConfigKey>
								                <ConfigValue>RUNNING</ConfigValue>
								                <Id>2933</Id>
								                <Note></Note>
								                <ConfigType>RESOURCE_QUEUE_CONFIG</ConfigType>
							              </EcmResourcePoolConfig>
							              <EcmResourcePoolConfig>
								                <Status>NORMAL</Status>
								                <Category>QUEUE_SUBMISSION_ACCESS_CONTROL</Category>
								                <ConfigKey>acl_submit_applications</ConfigKey>
								                <ConfigValue></ConfigValue>
								                <Id>2934</Id>
								                <Note></Note>
								                <ConfigType>RESOURCE_QUEUE_CONFIG</ConfigType>
							              </EcmResourcePoolConfig>
							              <EcmResourcePoolConfig>
								                <Status>NORMAL</Status>
								                <Category>QUEUE_ADMINISTRATION_ACCESS_CONTROL</Category>
								                <ConfigKey>acl_administer_queue</ConfigKey>
								                <ConfigValue></ConfigValue>
								                <Id>2935</Id>
								                <Note></Note>
								                <ConfigType>RESOURCE_QUEUE_CONFIG</ConfigType>
							              </EcmResourcePoolConfig>
						            </EcmResourcePoolConfigList>
						            <EcmResourceQueue>
							              <Name>default</Name>
							              <Status>NORMAL</Status>
							              <QualifiedName>default</QualifiedName>
							              <Id>247</Id>
							              <QueueType></QueueType>
							              <ResourcePoolId>116</ResourcePoolId>
							              <UserId>152834231764****</UserId>
							              <Leaf>false</Leaf>
							              <ParentQueueId>0</ParentQueueId>
						            </EcmResourceQueue>
					          </Queue>
				        </QueueList>
			      </PoolInfo>
			      <PoolInfo>
				        <EcmResourcePoolConfigList></EcmResourcePoolConfigList>
				        <EcmResourcePool>
					          <Name>pool1</Name>
					          <YarnSiteConfig></YarnSiteConfig>
					          <PoolType>CAPACITY_SCHEDULER</PoolType>
					          <Active>true</Active>
					          <Id>117</Id>
					          <Note></Note>
					          <UserId>152834231764****</UserId>
				        </EcmResourcePool>
				        <QueueList></QueueList>
			      </PoolInfo>
		    </PoolInfoList>
		    <RequestId>213DD361-EF38-4A31-A909-96857AEE42DE</RequestId>
	  </data>
	  <requestId>213DD361-EF38-4A31-A909-96857AEE42DE</requestId>
	  <successResponse>true</successResponse>
</ListResourcePool>
```

`JSON` 格式

``` {#json_return_success_demo}
{
	"successResponse":true,
	"requestId":"213DD361-EF38-4A31-A909-96857AEE42DE",
	"data":{
		"PoolInfoList":{
			"PoolInfo":[
				{
					"EcmResourcePoolConfigList":{
						"EcmResourcePoolConfig":[]
					},
					"EcmResourcePool":{
						"YarnSiteConfig":"",
						"Name":"DEFAULT",
						"Active":true,
						"PoolType":"CAPACITY_SCHEDULER",
						"Note":"",
						"Id":116,
						"UserId":"152834231764****"
					},
					"QueueList":{
						"Queue":[
							{
								"EcmResourcePoolConfigList":{
									"EcmResourcePoolConfig":[
										{
											"Status":"NORMAL",
											"Category":"QUEUE_RESOURCE_LIMIT",
											"ConfigKey":"capacity",
											"ConfigValue":"100",
											"Note":"",
											"Id":2925,
											"ConfigType":"RESOURCE_QUEUE_CONFIG"
										},
										{
											"Status":"NORMAL",
											"Category":"QUEUE_RESOURCE_LIMIT",
											"ConfigKey":"minimum-user-limit-percent",
											"ConfigValue":"0",
											"Note":"",
											"Id":2926,
											"ConfigType":"RESOURCE_QUEUE_CONFIG"
										},
										{
											"Status":"NORMAL",
											"Category":"QUEUE_RESOURCE_LIMIT",
											"ConfigKey":"maximum-capacity",
											"ConfigValue":"0",
											"Note":"",
											"Id":2927,
											"ConfigType":"RESOURCE_QUEUE_CONFIG"
										},
										{
											"Status":"NORMAL",
											"Category":"QUEUE_RESOURCE_LIMIT",
											"ConfigKey":"user-limit-factor",
											"ConfigValue":"",
											"Note":"",
											"Id":2928,
											"ConfigType":"RESOURCE_QUEUE_CONFIG"
										},
										{
											"Status":"NORMAL",
											"Category":"QUEUE_RESOURCE_LIMIT",
											"ConfigKey":"maximum-allocation-mb",
											"ConfigValue":"",
											"Note":"",
											"Id":2929,
											"ConfigType":"RESOURCE_QUEUE_CONFIG"
										},
										{
											"Status":"NORMAL",
											"Category":"QUEUE_RESOURCE_LIMIT",
											"ConfigKey":"maximum-allocation-vcores",
											"ConfigValue":"",
											"Note":"",
											"Id":2930,
											"ConfigType":"RESOURCE_QUEUE_CONFIG"
										},
										{
											"Status":"NORMAL",
											"Category":"QUEUE_RESOURCE_LIMIT",
											"ConfigKey":"maximum-applications",
											"ConfigValue":"",
											"Note":"",
											"Id":2931,
											"ConfigType":"RESOURCE_QUEUE_CONFIG"
										},
										{
											"Status":"NORMAL",
											"Category":"QUEUE_RESOURCE_LIMIT",
											"ConfigKey":"maximum-am-resource-percent",
											"ConfigValue":"",
											"Note":"",
											"Id":2932,
											"ConfigType":"RESOURCE_QUEUE_CONFIG"
										},
										{
											"Status":"NORMAL",
											"Category":"QUEUE_RESOURCE_LIMIT",
											"ConfigKey":"state",
											"ConfigValue":"RUNNING",
											"Note":"",
											"Id":2933,
											"ConfigType":"RESOURCE_QUEUE_CONFIG"
										},
										{
											"Status":"NORMAL",
											"Category":"QUEUE_SUBMISSION_ACCESS_CONTROL",
											"ConfigKey":"acl_submit_applications",
											"ConfigValue":"",
											"Note":"",
											"Id":2934,
											"ConfigType":"RESOURCE_QUEUE_CONFIG"
										},
										{
											"Status":"NORMAL",
											"Category":"QUEUE_ADMINISTRATION_ACCESS_CONTROL",
											"ConfigKey":"acl_administer_queue",
											"ConfigValue":"",
											"Note":"",
											"Id":2935,
											"ConfigType":"RESOURCE_QUEUE_CONFIG"
										}
									]
								},
								"EcmResourceQueue":{
									"Name":"default",
									"Status":"NORMAL",
									"QualifiedName":"default",
									"ResourcePoolId":116,
									"QueueType":"",
									"Id":247,
									"UserId":"152834231764****",
									"Leaf":false,
									"ParentQueueId":0
								}
							}
						]
					}
				},
				{
					"EcmResourcePoolConfigList":{
						"EcmResourcePoolConfig":[]
					},
					"EcmResourcePool":{
						"YarnSiteConfig":"",
						"Name":"pool1",
						"Active":true,
						"PoolType":"CAPACITY_SCHEDULER",
						"Note":"",
						"Id":117,
						"UserId":"152834231764****"
					},
					"QueueList":{
						"Queue":[]
					}
				}
			]
		},
		"RequestId":"213DD361-EF38-4A31-A909-96857AEE42DE"
	},
	"code":"200"
}
```

## 错误码 { .section}

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|403|Params.Illegal|The specified parameters are wrongly formed..|指定参数格式错误|
|403|User.OtherUserResource.NotAllow|It is not allowed to operate other user's resource|不能操作其它用户的资源|
|403|Invalid.Cluster.Status|Invalid cluster status %s in status list|指定集群状态不合法|
|403|Invalid.Cluster.Type|Invalid cluster type %s in cluster type list|指定集群类型不合法|
|500|InternalError|The request processing has failed due to some unknown error.|内部错误，请提工单|

访问[错误中心](https://error-center.alibabacloud.com/status/product/Emr)查看更多错误码。

