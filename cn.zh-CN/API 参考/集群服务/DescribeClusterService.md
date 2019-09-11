# DescribeClusterService {#doc_api_Emr_DescribeClusterService .reference}

调用 DescribeClusterService 接口，查询集群当前安装服务的详情信息。

## 调试 {#api_explorer .section}

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=DescribeClusterService&type=RPC&version=2016-04-08)

## 请求参数 {#parameters .section}

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DescribeClusterService|系统规定参数。取值：DescribeClusterService。

 |
|ClusterId|String|是|C-F32FB31D8295\*\*\*\*|待查询的集群ID。

 |
|RegionId|String|是|cn-hangzhou|地域ID。

 |
|ServiceName|String|是|HDFS|待查询的服务名称，例如：HDFS、YARN。

 |
|AccessKeyId|String|否|LTAI8ljWyu7y\*\*\*\*|阿里云AccessKey ID信息，用于标识访问者身份。

 |

## 返回数据 {#resultMapping .section}

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|EBB4D49C-4064-4818-B3AE-4C6BE5FC8264|当前请求的唯一ID标识。

 |
|ServiceInfo| | |集群的服务详情信息。

 |
|ClusterServiceSummaryList| | |服务的概览信息，主要是服务各个组件的统计数据信息。

 |
|ClusterServiceSummary| | |服务的概览信息，主要是服务各个组件的统计数据信息。

 |
|AlertInfo|String|""|当前Key对应的警告信息。

 |
|Category|String|MASTER|组件类型，枚举值：Client、MASTER和SLAVE。

 |
|DesiredStoppedValue|Integer|1|预期的已停止组件个数。

 |
|DisplayName|String|NodeManager|展示名称。

 |
|Key|String|NodeManager|Key，一般是服务的组件名称。

 |
|Status|String|OK|状态信息。

 |
|Type|String|COMPONENT|当前Key的类型信息。

 |
|Value|String|20/20 Started|当前Key对应的Value信息。

 |
|NeedRestartComponentNameList| |\["NodeManager","ResourceManager"\]|待重启的组件名称列表。

 |
|Service| | |待重启的组件名称列表。

 |
|NeedRestartHostIdList| |\["HostId1"\]|集群中，当前服务待重启的组件所在的主机ID信息。

 |
|Service| | |集群中，当前服务待重启的组件所在的主机ID信息。

 |
|NeedRestartInfo|String|""|当前服务待重启的组件详情信息。

 |
|NeedRestartNum|Integer|0|当前服务需要重启的组件个数。

 |
|ServiceActionList| | |服务支持的Action列表。

 |
|ServiceAction| | |服务支持的Action列表。

 |
|ActionName|String|CUSTOM\_COMMAND|Action名称。

 |
|Command|String|refreshQueues|ActionName为**CUSTOM\_COMMAND**时，才会有对应的**Command**。不同的服务支持不同的**Command**，当前 YARN 支持**refreshQueues**、**enableCGroups**和**disableCGroups**。

 |
|ComponentName|String|NodeManager|Action对应的组件名称。

 |
|DisplayName|String|RESTART NodeManager|Action的展示名称。

 |
|ServiceName|String|YARN|服务名称。

 |
|ServiceName|String|YARN|服务名称。

 |
|ServiceStatus|String|INSTALLED|服务状态。

 |
|ServiceVersion|String|2.7.2|服务的版本信息。

 |

## 示例 {#demo .section}

请求示例

``` {#request_demo}

http(s)://[Endpoint]/?Action=DescribeClusterService
&ClusterId=C-F32FB31D8295****
&RegionId=cn-hangzhou
&ServiceName=HDFS
&<公共请求参数>

```

正常返回示例

`XML` 格式

``` {#xml_return_success_demo}
<DescribeClusterServiceResponse>
	  <data>
		    <RequestId>7A23195A-BC03-4D82-BED5-90ED0D36F002</RequestId>
		    <ServiceInfo>
			      <NeedRestartHostIdList></NeedRestartHostIdList>
			      <ServiceActionList>
				        <ServiceAction>
					          <ActionName>CONFIGURE</ActionName>
					          <ServiceName>YARN</ServiceName>
					          <DisplayName>CONFIGURE All Components</DisplayName>
					          <ComponentName>ALL COMPONENTS</ComponentName>
				        </ServiceAction>
				        <ServiceAction>
					          <ActionName>START</ActionName>
					          <ServiceName>YARN</ServiceName>
					          <DisplayName>START All Components</DisplayName>
					          <ComponentName>ALL COMPONENTS</ComponentName>
				        </ServiceAction>
				        <ServiceAction>
					          <ActionName>STOP</ActionName>
					          <ServiceName>YARN</ServiceName>
					          <DisplayName>STOP All Components</DisplayName>
					          <ComponentName>ALL COMPONENTS</ComponentName>
				        </ServiceAction>
				        <ServiceAction>
					          <ActionName>RESTART</ActionName>
					          <ServiceName>YARN</ServiceName>
					          <DisplayName>RESTART All Components</DisplayName>
					          <ComponentName>ALL COMPONENTS</ComponentName>
				        </ServiceAction>
				        <ServiceAction>
					          <ActionName>RESTART</ActionName>
					          <ServiceName>YARN</ServiceName>
					          <DisplayName>RESTART NodeManager</DisplayName>
					          <ComponentName>NodeManager</ComponentName>
				        </ServiceAction>
				        <ServiceAction>
					          <ActionName>RESTART</ActionName>
					          <ServiceName>YARN</ServiceName>
					          <DisplayName>RESTART WebAppProxyServer</DisplayName>
					          <ComponentName>WebAppProxyServer</ComponentName>
				        </ServiceAction>
				        <ServiceAction>
					          <ActionName>RESTART</ActionName>
					          <ServiceName>YARN</ServiceName>
					          <DisplayName>RESTART JobHistory</DisplayName>
					          <ComponentName>JobHistory</ComponentName>
				        </ServiceAction>
				        <ServiceAction>
					          <ActionName>RESTART</ActionName>
					          <ServiceName>YARN</ServiceName>
					          <DisplayName>RESTART ResourceManager</DisplayName>
					          <ComponentName>ResourceManager</ComponentName>
				        </ServiceAction>
				        <ServiceAction>
					          <ActionName>RESTART</ActionName>
					          <ServiceName>YARN</ServiceName>
					          <DisplayName>RESTART App Timeline Server</DisplayName>
					          <ComponentName>TimeLineServer</ComponentName>
				        </ServiceAction>
				        <ServiceAction>
					          <Command>refreshQueues</Command>
					          <ActionName>CUSTOM_COMMAND</ActionName>
					          <ServiceName>YARN</ServiceName>
					          <DisplayName>Refresh Queues</DisplayName>
					          <ComponentName>ResourceManager</ComponentName>
				        </ServiceAction>
				        <ServiceAction>
					          <Command>enableCGroups</Command>
					          <ActionName>CUSTOM_COMMAND</ActionName>
					          <ServiceName>YARN</ServiceName>
					          <DisplayName>Enable CGroups</DisplayName>
					          <ComponentName>NodeManager</ComponentName>
				        </ServiceAction>
				        <ServiceAction>
					          <Command>disableCGroups</Command>
					          <ActionName>CUSTOM_COMMAND</ActionName>
					          <ServiceName>YARN</ServiceName>
					          <DisplayName>Disable CGroups</DisplayName>
					          <ComponentName>NodeManager</ComponentName>
				        </ServiceAction>
			      </ServiceActionList>
			      <ServiceName>YARN</ServiceName>
			      <NeedRestartInfo></NeedRestartInfo>
			      <NeedRestartNum>0</NeedRestartNum>
			      <ClusterServiceSummaryList>
				        <ClusterServiceSummary>
					          <Status>OK</Status>
					          <Value>20/20 Started</Value>
					          <Key>NodeManager</Key>
					          <Type>COMPONENT</Type>
					          <DisplayName>NodeManager</DisplayName>
					          <AlertInfo></AlertInfo>
				        </ClusterServiceSummary>
				        <ClusterServiceSummary>
					          <Status>OK</Status>
					          <Value>1/1 Started</Value>
					          <Key>JobHistory</Key>
					          <Type>COMPONENT</Type>
					          <DisplayName>JobHistory</DisplayName>
					          <AlertInfo></AlertInfo>
				        </ClusterServiceSummary>
				        <ClusterServiceSummary>
					          <Status>OK</Status>
					          <Value>22/22 Installed</Value>
					          <Key>YarnInit</Key>
					          <Type>COMPONENT</Type>
					          <DisplayName>Yarn Client</DisplayName>
					          <AlertInfo></AlertInfo>
				        </ClusterServiceSummary>
				        <ClusterServiceSummary>
					          <Status>OK</Status>
					          <Value>1/1 Started</Value>
					          <Key>TimeLineServer</Key>
					          <Type>COMPONENT</Type>
					          <DisplayName>App Timeline Server</DisplayName>
					          <AlertInfo></AlertInfo>
				        </ClusterServiceSummary>
				        <ClusterServiceSummary>
					          <Status>OK</Status>
					          <Value>1/1 Started</Value>
					          <Key>WebAppProxyServer</Key>
					          <Type>COMPONENT</Type>
					          <DisplayName>WebAppProxyServer</DisplayName>
					          <AlertInfo></AlertInfo>
				        </ClusterServiceSummary>
				        <ClusterServiceSummary>
					          <Status>OK</Status>
					          <Value>2/2 Started</Value>
					          <Key>ResourceManager</Key>
					          <Type>COMPONENT</Type>
					          <DisplayName>ResourceManager</DisplayName>
					          <AlertInfo></AlertInfo>
				        </ClusterServiceSummary>
			      </ClusterServiceSummaryList>
			      <ServiceVersion>2.7.2-1.3.1</ServiceVersion>
			      <NeedRestartComponentNameList></NeedRestartComponentNameList>
			      <ServiceStatus>INSTALLING</ServiceStatus>
		    </ServiceInfo>
	  </data>
	  <requestId>7A23195A-BC03-4D82-BED5-90ED0D36F002</requestId>
</DescribeClusterServiceResponse>
```

`JSON` 格式

``` {#json_return_success_demo}
{
	"requestId":"7A23195A-BC03-4D82-BED5-90ED0D36F002",
	"data":{
		"RequestId":"7A23195A-BC03-4D82-BED5-90ED0D36F002",
		"ServiceInfo":{
			"NeedRestartHostIdList":{
				"Service":[]
			},
			"ServiceActionList":{
				"ServiceAction":[
					{
						"ActionName":"CONFIGURE",
						"ServiceName":"YARN",
						"DisplayName":"CONFIGURE All Components",
						"ComponentName":"ALL COMPONENTS"
					},
					{
						"ActionName":"START",
						"ServiceName":"YARN",
						"DisplayName":"START All Components",
						"ComponentName":"ALL COMPONENTS"
					},
					{
						"ActionName":"STOP",
						"ServiceName":"YARN",
						"DisplayName":"STOP All Components",
						"ComponentName":"ALL COMPONENTS"
					},
					{
						"ActionName":"RESTART",
						"ServiceName":"YARN",
						"DisplayName":"RESTART All Components",
						"ComponentName":"ALL COMPONENTS"
					},
					{
						"ActionName":"RESTART",
						"ServiceName":"YARN",
						"DisplayName":"RESTART NodeManager",
						"ComponentName":"NodeManager"
					},
					{
						"ActionName":"RESTART",
						"ServiceName":"YARN",
						"DisplayName":"RESTART WebAppProxyServer",
						"ComponentName":"WebAppProxyServer"
					},
					{
						"ActionName":"RESTART",
						"ServiceName":"YARN",
						"DisplayName":"RESTART JobHistory",
						"ComponentName":"JobHistory"
					},
					{
						"ActionName":"RESTART",
						"ServiceName":"YARN",
						"DisplayName":"RESTART ResourceManager",
						"ComponentName":"ResourceManager"
					},
					{
						"ActionName":"RESTART",
						"ServiceName":"YARN",
						"DisplayName":"RESTART App Timeline Server",
						"ComponentName":"TimeLineServer"
					},
					{
						"Command":"refreshQueues",
						"ActionName":"CUSTOM_COMMAND",
						"ServiceName":"YARN",
						"DisplayName":"Refresh Queues",
						"ComponentName":"ResourceManager"
					},
					{
						"Command":"enableCGroups",
						"ActionName":"CUSTOM_COMMAND",
						"ServiceName":"YARN",
						"DisplayName":"Enable CGroups",
						"ComponentName":"NodeManager"
					},
					{
						"Command":"disableCGroups",
						"ActionName":"CUSTOM_COMMAND",
						"ServiceName":"YARN",
						"DisplayName":"Disable CGroups",
						"ComponentName":"NodeManager"
					}
				]
			},
			"ServiceName":"YARN",
			"NeedRestartInfo":"",
			"ClusterServiceSummaryList":{
				"ClusterServiceSummary":[
					{
						"Status":"OK",
						"Value":"20/20 Started",
						"Key":"NodeManager",
						"Type":"COMPONENT",
						"DisplayName":"NodeManager",
						"AlertInfo":""
					},
					{
						"Status":"OK",
						"Value":"1/1 Started",
						"Key":"JobHistory",
						"Type":"COMPONENT",
						"DisplayName":"JobHistory",
						"AlertInfo":""
					},
					{
						"Status":"OK",
						"Value":"22/22 Installed",
						"Key":"YarnInit",
						"Type":"COMPONENT",
						"DisplayName":"Yarn Client",
						"AlertInfo":""
					},
					{
						"Status":"OK",
						"Value":"1/1 Started",
						"Key":"TimeLineServer",
						"Type":"COMPONENT",
						"DisplayName":"App Timeline Server",
						"AlertInfo":""
					},
					{
						"Status":"OK",
						"Value":"1/1 Started",
						"Key":"WebAppProxyServer",
						"Type":"COMPONENT",
						"DisplayName":"WebAppProxyServer",
						"AlertInfo":""
					},
					{
						"Status":"OK",
						"Value":"2/2 Started",
						"Key":"ResourceManager",
						"Type":"COMPONENT",
						"DisplayName":"ResourceManager",
						"AlertInfo":""
					}
				]
			},
			"NeedRestartNum":0,
			"ServiceVersion":"2.7.2-1.3.1",
			"NeedRestartComponentNameList":{
				"Service":[]
			},
			"ServiceStatus":"INSTALLING"
		}
	}
}
```

## 错误码 { .section}

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InvalidServiceName|The cluster doesn't have this service.|集群中不存在该服务|
|403|Params.Illegal|The specified parameters are wrongly formed..|指定参数格式错误|
|404|ClusterId.NotFound|ClusterId \[%s\] does not exist.|Cluster Id不存在，确认集群的Cluster Id|
|403|User.OtherUserResource.NotAllow|It is not allowed to operate other user's resource|不能操作其它用户的资源|
|500|InternalError|The request processing has failed due to some unknown error.|内部错误，请提工单|

访问[错误中心](https://error-center.alibabacloud.com/status/product/Emr)查看更多错误码。

