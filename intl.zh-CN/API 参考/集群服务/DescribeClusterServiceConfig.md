# DescribeClusterServiceConfig {#doc_api_Emr_DescribeClusterServiceConfig .reference}

调用 DescribeClusterServiceConfig 接口查询集群指定服务的配置详情信息。

## 调试 {#api_explorer .section}

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=DescribeClusterServiceConfig&type=RPC&version=2016-04-08)

## 请求参数 {#parameters .section}

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|RegionId|String|是|cn-hangzhou|地域ID。

 |
|ClusterId|String|是|C-F32FB31D82954C64|集群ID。

 |
|Action|String|是|DescribeClusterServiceConfig|系统规定参数。取值：DescribeClusterServiceConfig。

 |
|ConfigVersion|String|否|0|配置的版本信息，可通过**DescribeClusterServiceConfigHistory**接口获取。

 |
|GroupId|String|否|0|当前配置组的ID信息。

 |
|HostInstanceId|String|否|ecsId|主机的**instanceId**信息，一般是对应的**ecsId**信息。

 |
|ServiceName|String|否|TEZ|服务名称。

 |
|TagValue|String|否|tez-site|待查询的配置的标签信息，可通过**DescribeClusterServiceConfigTag**接口获取。

 |
|AccessKeyId|String|否|LTAI8ljWyu7y\*\*\*\*|阿里云AccessKey ID信息，用于标识访问者身份。

 |

## 返回数据 {#resultMapping .section}

|名称|类型|示例值|描述|
|--|--|---|--|
|Config| | |配置的详情信息。

 |
|Applied|String|true|当前是否已经生效。

 |
|Author|String|111|修改人。

 |
|Comment|String|""|备注。

 |
|ConfigValueList| | |服务的配置文件列表。

 |
|ConfigValue| | |服务的配置文件列表。

 |
|AllowCustom|Boolean|true|当前配置文件是否允许自定义配置项。

 |
|ConfigItemValueList| | |配置项的K-V对。

 |
|ConfigItemValue| | |配置项的K-V对。

 |
|Description|String|""|配置项的描述信息。

 |
|IsCustom|Boolean|false|是否为自定义配置项。

 |
|ItemName|String|tez.history.logging.service.class|配置项名称。

 |
|Value|String|org.apache.tez.dag.history.logging.ats.ATSHistoryLoggingService|配置项的值。

 |
|ConfigName|String|tez-site|配置文件名。

 |
|ConfigVersion|String|0|配置版本。

 |
|CreateTime|String|1543312717000|创建时间。

 |
|PropertyInfoList| | |配置项的属性信息。

 |
|PropertyInfo| | |配置项的属性信息。

 |
|Component|String|""|组件名。

 |
|Description|String|""|配置项的描述信息。

 |
|DisplayName|String|tez-site|展示名称。

 |
|EffectWay| | |预留字段。

 |
|EffectType|String|""|预留字段。

 |
|InvokeServiceName|String|""|预留字段。

 |
|FileName|String|tez-site|配置文件名。

 |
|Name|String|tez.lib.uris|配置项名称。

 |
|PropertyTypes| |\["MEMORY"\]|配置项的属性类型。

 |
|propertyType| | |配置项的属性类型。

 |
|PropertyValueAttributes| | |配置项的值的属性信息。

 |
|Entries| | |预留字段。

 |
|ValueEntryInfo| | |预留字段。

 |
|Description|String|“”|预留字段。

 |
|Label|String|“”|预留字段。

 |
|Value|String|“”|预留字段。

 |
|Hidden|Boolean|true|是否展示。

 |
|IncrememtStep|String|10|递增类型的Value的递增步长。

 |
|Maximum|String|10000|最大值。

 |
|Mimimum|String|10|最小值。

 |
|ReadOnly|Boolean|true|配置项是否只读（不能修改）。

 |
|Type|String|“”|预留字段，暂时不生效。

 |
|Unit|String|MB|配置项值的单位。

 |
|ServiceName|String|TEZ|服务名称。

 |
|Value|String|$\{fs.defaultFS\}/apps/tez-0.9.1-1.0.2/,$\{fs.defaultFS\}/apps/tez-0.9.1-1.0.2/lib/|配置项的value模板或者示例。

 |
|ServiceName|String|TEZ|服务名称。

 |
|RequestId|String|094585B2-13AF-4780-96B3-C8E925418B5D|请求ID。

 |

## 示例 {#demo .section}

请求示例

``` {#request_demo}

http(s)://[Endpoint]/?Action=DescribeClusterServiceConfig
&RegionId=cn-hangzhou
&ClusterId=C-F32FB31D82954C64
&<公共请求参数>

```

正常返回示例

`XML` 格式

``` {#xml_return_success_demo}
<DescribeClusterServiceConfigResponse>
	  <RequestId>094585B2-13AF-4780-96B3-C8E925418B5D</RequestId>
	  <Config>
		    <Comment></Comment>
		    <ServiceName>TEZ</ServiceName>
		    <PropertyInfoList>
			      <PropertyInfo>
				        <PropertyTypes></PropertyTypes>
				        <Description></Description>
				        <PropertyValueAttributes>
					          <ReadOnly>false</ReadOnly>
					          <Type></Type>
					          <Maximum>2147483647</Maximum>
					          <Mimimum></Mimimum>
					          <Hidden>false</Hidden>
					          <IncrememtStep></IncrememtStep>
					          <Entries></Entries>
					          <Unit></Unit>
				        </PropertyValueAttributes>
				        <ServiceName>TEZ</ServiceName>
				        <EffectWay></EffectWay>
				        <FileName>tez-site</FileName>
				        <Value>${fs.defaultFS}/apps/tez-0.9.1-1.0.2/,${fs.defaultFS}/apps/tez-0.9.1-1.0.2/lib/</Value>
				        <DisplayName></DisplayName>
				        <Name>tez.lib.uris</Name>
			      </PropertyInfo>
			      <PropertyInfo>
				        <PropertyTypes></PropertyTypes>
				        <Description></Description>
				        <PropertyValueAttributes>
					          <ReadOnly>false</ReadOnly>
					          <Type></Type>
					          <Maximum>2147483647</Maximum>
					          <Mimimum></Mimimum>
					          <Hidden>false</Hidden>
					          <IncrememtStep></IncrememtStep>
					          <Entries></Entries>
					          <Unit></Unit>
				        </PropertyValueAttributes>
				        <ServiceName>TEZ</ServiceName>
				        <EffectWay></EffectWay>
				        <FileName>tez-site</FileName>
				        <Value>/tmp/tez/staging</Value>
				        <DisplayName></DisplayName>
				        <Name>tez.staging-dir</Name>
			      </PropertyInfo>
			      <PropertyInfo>
				        <PropertyTypes>
					          <propertyType>MEMORY</propertyType>
				        </PropertyTypes>
				        <Description></Description>
				        <PropertyValueAttributes>
					          <ReadOnly>false</ReadOnly>
					          <Type></Type>
					          <Maximum>2147483647</Maximum>
					          <Mimimum></Mimimum>
					          <Hidden>false</Hidden>
					          <IncrememtStep></IncrememtStep>
					          <Entries></Entries>
					          <Unit></Unit>
				        </PropertyValueAttributes>
				        <ServiceName>TEZ</ServiceName>
				        <EffectWay></EffectWay>
				        <FileName>tez-site</FileName>
				        <Value>#yarn_app_mapreduce_am_resource_mb#</Value>
				        <DisplayName></DisplayName>
				        <Name>tez.am.resource.memory.mb</Name>
			      </PropertyInfo>
			      <PropertyInfo>
				        <PropertyTypes>
					          <propertyType>MEMORY</propertyType>
				        </PropertyTypes>
				        <Description></Description>
				        <PropertyValueAttributes>
					          <ReadOnly>false</ReadOnly>
					          <Type></Type>
					          <Maximum>2147483647</Maximum>
					          <Mimimum></Mimimum>
					          <Hidden>false</Hidden>
					          <IncrememtStep></IncrememtStep>
					          <Entries></Entries>
					          <Unit></Unit>
				        </PropertyValueAttributes>
				        <ServiceName>TEZ</ServiceName>
				        <EffectWay></EffectWay>
				        <FileName>tez-site</FileName>
				        <Value>0.8</Value>
				        <DisplayName></DisplayName>
				        <Name>tez.container.max.java.heap.fraction</Name>
			      </PropertyInfo>
			      <PropertyInfo>
				        <PropertyTypes></PropertyTypes>
				        <Description></Description>
				        <PropertyValueAttributes>
					          <ReadOnly>false</ReadOnly>
					          <Type></Type>
					          <Maximum>2147483647</Maximum>
					          <Mimimum></Mimimum>
					          <Hidden>false</Hidden>
					          <IncrememtStep></IncrememtStep>
					          <Entries></Entries>
					          <Unit></Unit>
				        </PropertyValueAttributes>
				        <ServiceName>TEZ</ServiceName>
				        <EffectWay></EffectWay>
				        <FileName>tez-site</FileName>
				        <Value>org.apache.tez.dag.history.logging.ats.ATSHistoryLoggingService</Value>
				        <DisplayName></DisplayName>
				        <Name>tez.history.logging.service.class</Name>
			      </PropertyInfo>
			      <PropertyInfo>
				        <PropertyTypes></PropertyTypes>
				        <Description>Publish configuration information to Timeline server.</Description>
				        <PropertyValueAttributes>
					          <ReadOnly>false</ReadOnly>
					          <Type></Type>
					          <Maximum>2147483647</Maximum>
					          <Mimimum></Mimimum>
					          <Hidden>false</Hidden>
					          <IncrememtStep></IncrememtStep>
					          <Entries></Entries>
					          <Unit></Unit>
				        </PropertyValueAttributes>
				        <ServiceName>TEZ</ServiceName>
				        <EffectWay></EffectWay>
				        <FileName>tez-site</FileName>
				        <Value>true</Value>
				        <DisplayName></DisplayName>
				        <Name>tez.runtime.convert.user-payload.to.history-text</Name>
			      </PropertyInfo>
			      <PropertyInfo>
				        <PropertyTypes></PropertyTypes>
				        <Description></Description>
				        <PropertyValueAttributes>
					          <ReadOnly>false</ReadOnly>
					          <Type></Type>
					          <Maximum>2147483647</Maximum>
					          <Mimimum></Mimimum>
					          <Hidden>false</Hidden>
					          <IncrememtStep></IncrememtStep>
					          <Entries></Entries>
					          <Unit></Unit>
				        </PropertyValueAttributes>
				        <ServiceName>TEZ</ServiceName>
				        <EffectWay></EffectWay>
				        <FileName>tez-site</FileName>
				        <Value>http://emr-header-1:8090/tez-ui2/</Value>
				        <DisplayName></DisplayName>
				        <Name>tez.tez-ui.history-url.base</Name>
			      </PropertyInfo>
			      <PropertyInfo>
				        <PropertyTypes></PropertyTypes>
				        <Description></Description>
				        <PropertyValueAttributes>
					          <ReadOnly>false</ReadOnly>
					          <Type></Type>
					          <Maximum>2147483647</Maximum>
					          <Mimimum></Mimimum>
					          <Hidden>false</Hidden>
					          <IncrememtStep></IncrememtStep>
					          <Entries></Entries>
					          <Unit></Unit>
				        </PropertyValueAttributes>
				        <ServiceName>TEZ</ServiceName>
				        <EffectWay></EffectWay>
				        <FileName>tez-site</FileName>
				        <Value>-Xmx512m</Value>
				        <DisplayName>tez.am.java.opts</DisplayName>
				        <Name>tez.am.java.opts</Name>
			      </PropertyInfo>
			      <PropertyInfo>
				        <PropertyTypes></PropertyTypes>
				        <Description></Description>
				        <PropertyValueAttributes>
					          <ReadOnly>false</ReadOnly>
					          <Type></Type>
					          <Maximum>2147483647</Maximum>
					          <Mimimum></Mimimum>
					          <Hidden>false</Hidden>
					          <IncrememtStep></IncrememtStep>
					          <Entries></Entries>
					          <Unit></Unit>
				        </PropertyValueAttributes>
				        <ServiceName>TEZ</ServiceName>
				        <EffectWay></EffectWay>
				        <FileName>user_params</FileName>
				        <Value>512</Value>
				        <DisplayName>tomcat_heapsize</DisplayName>
				        <Name>tomcat_heapsize</Name>
			      </PropertyInfo>
			      <PropertyInfo>
				        <PropertyTypes></PropertyTypes>
				        <Description></Description>
				        <PropertyValueAttributes>
					          <ReadOnly>false</ReadOnly>
					          <Type></Type>
					          <Maximum>2147483647</Maximum>
					          <Mimimum></Mimimum>
					          <Hidden>false</Hidden>
					          <IncrememtStep></IncrememtStep>
					          <Entries></Entries>
					          <Unit></Unit>
				        </PropertyValueAttributes>
				        <ServiceName>TEZ</ServiceName>
				        <EffectWay></EffectWay>
				        <FileName>user_params</FileName>
				        <Value>emr-header-1</Value>
				        <DisplayName>do_init_host</DisplayName>
				        <Name>do_init_host</Name>
			      </PropertyInfo>
		    </PropertyInfoList>
		    <ConfigVersion>0</ConfigVersion>
		    <CreateTime>1543312717000</CreateTime>
		    <Author>1250460021754461</Author>
		    <Applied>true</Applied>
		    <ConfigValueList>
			      <ConfigValue>
				        <ConfigName>tez-site</ConfigName>
				        <AllowCustom>true</AllowCustom>
				        <ConfigItemValueList>
					          <ConfigItemValue>
						            <Value>org.apache.tez.dag.history.logging.ats.ATSHistoryLoggingService</Value>
						            <ItemName>tez.history.logging.service.class</ItemName>
					          </ConfigItemValue>
					          <ConfigItemValue>
						            <Value>true</Value>
						            <ItemName>tez.runtime.convert.user-payload.to.history-text</ItemName>
					          </ConfigItemValue>
					          <ConfigItemValue>
						            <Value>640</Value>
						            <ItemName>tez.am.resource.memory.mb</ItemName>
					          </ConfigItemValue>
					          <ConfigItemValue>
						            <Value>http://emr-header-1:8090/tez-ui2/</Value>
						            <ItemName>tez.tez-ui.history-url.base</ItemName>
					          </ConfigItemValue>
					          <ConfigItemValue>
						            <Value>${fs.defaultFS}/apps/tez-0.9.1-1.0.2/,${fs.defaultFS}/apps/tez-0.9.1-1.0.2/lib/</Value>
						            <ItemName>tez.lib.uris</ItemName>
					          </ConfigItemValue>
					          <ConfigItemValue>
						            <Value>0.8</Value>
						            <ItemName>tez.container.max.java.heap.fraction</ItemName>
					          </ConfigItemValue>
					          <ConfigItemValue>
						            <Value>-Xmx512m</Value>
						            <ItemName>tez.am.java.opts</ItemName>
					          </ConfigItemValue>
					          <ConfigItemValue>
						            <Value>/tmp/tez/staging</Value>
						            <ItemName>tez.staging-dir</ItemName>
					          </ConfigItemValue>
				        </ConfigItemValueList>
			      </ConfigValue>
		    </ConfigValueList>
	  </Config>
</DescribeClusterServiceConfigResponse>
```

`JSON` 格式

``` {#json_return_success_demo}
{
	"Config":{
		"Applied":true,
		"PropertyInfoList":{
			"PropertyInfo":[
				{
					"Name":"tez.lib.uris",
					"Value":"${fs.defaultFS}/apps/tez-0.9.1-1.0.2/,${fs.defaultFS}/apps/tez-0.9.1-1.0.2/lib/",
					"Description":"",
					"ServiceName":"TEZ",
					"FileName":"tez-site",
					"PropertyTypes":{
						"propertyType":[]
					},
					"EffectWay":{},
					"PropertyValueAttributes":{
						"ReadOnly":false,
						"Maximum":"2147483647",
						"Type":"",
						"Mimimum":"",
						"Hidden":false,
						"IncrememtStep":"",
						"Unit":"",
						"Entries":{
							"ValueEntryInfo":[]
						}
					},
					"DisplayName":""
				},
				{
					"Name":"tez.staging-dir",
					"Value":"/tmp/tez/staging",
					"Description":"",
					"ServiceName":"TEZ",
					"FileName":"tez-site",
					"PropertyTypes":{
						"propertyType":[]
					},
					"EffectWay":{},
					"PropertyValueAttributes":{
						"ReadOnly":false,
						"Maximum":"2147483647",
						"Type":"",
						"Mimimum":"",
						"Hidden":false,
						"IncrememtStep":"",
						"Unit":"",
						"Entries":{
							"ValueEntryInfo":[]
						}
					},
					"DisplayName":""
				},
				{
					"Name":"tez.am.resource.memory.mb",
					"Value":"#yarn_app_mapreduce_am_resource_mb#",
					"Description":"",
					"ServiceName":"TEZ",
					"FileName":"tez-site",
					"PropertyTypes":{
						"propertyType":[
							"MEMORY"
						]
					},
					"EffectWay":{},
					"PropertyValueAttributes":{
						"ReadOnly":false,
						"Maximum":"2147483647",
						"Type":"",
						"Mimimum":"",
						"Hidden":false,
						"IncrememtStep":"",
						"Unit":"",
						"Entries":{
							"ValueEntryInfo":[]
						}
					},
					"DisplayName":""
				},
				{
					"Name":"tez.container.max.java.heap.fraction",
					"Value":"0.8",
					"Description":"",
					"ServiceName":"TEZ",
					"FileName":"tez-site",
					"PropertyTypes":{
						"propertyType":[
							"MEMORY"
						]
					},
					"EffectWay":{},
					"PropertyValueAttributes":{
						"ReadOnly":false,
						"Maximum":"2147483647",
						"Type":"",
						"Mimimum":"",
						"Hidden":false,
						"IncrememtStep":"",
						"Unit":"",
						"Entries":{
							"ValueEntryInfo":[]
						}
					},
					"DisplayName":""
				},
				{
					"Name":"tez.history.logging.service.class",
					"Value":"org.apache.tez.dag.history.logging.ats.ATSHistoryLoggingService",
					"Description":"",
					"ServiceName":"TEZ",
					"FileName":"tez-site",
					"PropertyTypes":{
						"propertyType":[]
					},
					"EffectWay":{},
					"PropertyValueAttributes":{
						"ReadOnly":false,
						"Maximum":"2147483647",
						"Type":"",
						"Mimimum":"",
						"Hidden":false,
						"IncrememtStep":"",
						"Unit":"",
						"Entries":{
							"ValueEntryInfo":[]
						}
					},
					"DisplayName":""
				},
				{
					"Name":"tez.runtime.convert.user-payload.to.history-text",
					"Value":"true",
					"Description":"Publish configuration information to Timeline server.",
					"ServiceName":"TEZ",
					"FileName":"tez-site",
					"PropertyTypes":{
						"propertyType":[]
					},
					"EffectWay":{},
					"PropertyValueAttributes":{
						"ReadOnly":false,
						"Maximum":"2147483647",
						"Type":"",
						"Mimimum":"",
						"Hidden":false,
						"IncrememtStep":"",
						"Unit":"",
						"Entries":{
							"ValueEntryInfo":[]
						}
					},
					"DisplayName":""
				},
				{
					"Name":"tez.tez-ui.history-url.base",
					"Value":"http://emr-header-1:8090/tez-ui2/",
					"Description":"",
					"ServiceName":"TEZ",
					"FileName":"tez-site",
					"PropertyTypes":{
						"propertyType":[]
					},
					"EffectWay":{},
					"PropertyValueAttributes":{
						"ReadOnly":false,
						"Maximum":"2147483647",
						"Type":"",
						"Mimimum":"",
						"Hidden":false,
						"IncrememtStep":"",
						"Unit":"",
						"Entries":{
							"ValueEntryInfo":[]
						}
					},
					"DisplayName":""
				},
				{
					"Name":"tez.am.java.opts",
					"Value":"-Xmx512m",
					"Description":"",
					"ServiceName":"TEZ",
					"FileName":"tez-site",
					"PropertyTypes":{
						"propertyType":[]
					},
					"EffectWay":{},
					"PropertyValueAttributes":{
						"ReadOnly":false,
						"Maximum":"2147483647",
						"Type":"",
						"Mimimum":"",
						"Hidden":false,
						"IncrememtStep":"",
						"Unit":"",
						"Entries":{
							"ValueEntryInfo":[]
						}
					},
					"DisplayName":"tez.am.java.opts"
				},
				{
					"Name":"tomcat_heapsize",
					"Value":"512",
					"Description":"",
					"ServiceName":"TEZ",
					"FileName":"user_params",
					"PropertyTypes":{
						"propertyType":[]
					},
					"EffectWay":{},
					"PropertyValueAttributes":{
						"ReadOnly":false,
						"Maximum":"2147483647",
						"Type":"",
						"Mimimum":"",
						"Hidden":false,
						"IncrememtStep":"",
						"Unit":"",
						"Entries":{
							"ValueEntryInfo":[]
						}
					},
					"DisplayName":"tomcat_heapsize"
				},
				{
					"Name":"do_init_host",
					"Value":"emr-header-1",
					"Description":"",
					"ServiceName":"TEZ",
					"FileName":"user_params",
					"PropertyTypes":{
						"propertyType":[]
					},
					"EffectWay":{},
					"PropertyValueAttributes":{
						"ReadOnly":false,
						"Maximum":"2147483647",
						"Type":"",
						"Mimimum":"",
						"Hidden":false,
						"IncrememtStep":"",
						"Unit":"",
						"Entries":{
							"ValueEntryInfo":[]
						}
					},
					"DisplayName":"do_init_host"
				}
			]
		},
		"ConfigValueList":{
			"ConfigValue":[
				{
					"ConfigItemValueList":{
						"ConfigItemValue":[
							{
								"Value":"org.apache.tez.dag.history.logging.ats.ATSHistoryLoggingService",
								"ItemName":"tez.history.logging.service.class"
							},
							{
								"Value":"true",
								"ItemName":"tez.runtime.convert.user-payload.to.history-text"
							},
							{
								"Value":"640",
								"ItemName":"tez.am.resource.memory.mb"
							},
							{
								"Value":"http://emr-header-1:8090/tez-ui2/",
								"ItemName":"tez.tez-ui.history-url.base"
							},
							{
								"Value":"${fs.defaultFS}/apps/tez-0.9.1-1.0.2/,${fs.defaultFS}/apps/tez-0.9.1-1.0.2/lib/",
								"ItemName":"tez.lib.uris"
							},
							{
								"Value":"0.8",
								"ItemName":"tez.container.max.java.heap.fraction"
							},
							{
								"Value":"-Xmx512m",
								"ItemName":"tez.am.java.opts"
							},
							{
								"Value":"/tmp/tez/staging",
								"ItemName":"tez.staging-dir"
							}
						]
					},
					"ConfigName":"tez-site",
					"AllowCustom":true
				}
			]
		},
		"ServiceName":"TEZ",
		"ConfigVersion":"0",
		"CreateTime":"1543312717000",
		"Author":"1250460021754461",
		"Comment":""
	},
	"RequestId":"094585B2-13AF-4780-96B3-C8E925418B5D"
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

访问[错误中心](https://error-center.aliyun.com/status/product/Emr)查看更多错误码。

