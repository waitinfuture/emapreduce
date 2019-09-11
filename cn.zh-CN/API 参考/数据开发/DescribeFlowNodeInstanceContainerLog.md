# DescribeFlowNodeInstanceContainerLog {#doc_api_Emr_DescribeFlowNodeInstanceContainerLog .reference}

调用 DescribeFlowNodeInstanceContainerLog 接口，查询节点实例容器日志

## 调试 {#api_explorer .section}

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=DescribeFlowNodeInstanceContainerLog&type=RPC&version=2016-04-08)

## 请求参数 {#parameters .section}

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DescribeFlowNodeInstanceContainerLog|系统规定参数。取值：DescribeFlowNodeInstanceContainerLog。

 |
|AppId|String|是|application\_1542620905989\_0010|作业的Application ID。

 |
|ContainerId|String|是|container\_1542620905989\_0010\_01\_000001|Conintainer ID。

 |
|LogName|String|是|stderr|Log 名称：

 -   stderr
-   syslog
-   stdout

 |
|NodeInstanceId|String|是|FNI-0D2534B3AB67\*\*\*\*|节点实例ID。

 |
|ProjectId|String|是|FP-BECB9D35CB12\*\*\*\*|项目ID。

 |
|RegionId|String|是|cn-hangzhou|地域ID。

 |
|Length|Integer|否|200|长度，最大1000。

 |
|Offset|Integer|否|0|偏移。

 |

## 返回数据 {#resultMapping .section}

|名称|类型|示例值|描述|
|--|--|---|--|
|LogEnd|Boolean|true|日志是否结束。

 |
|LogEntrys| | |日志内容。

 |
|LogEntry| | |日志内容。

 |
|Content|String|2018-11-19 17:55:11,792 INFO \[RMCommunicator Allocator\] org.apache.hadoop.yarn.util.RackResolver: Resolved emr-worker-1.cluster-500160492 to /default-rack|日志实际内容。

 |
|RequestId|String|4E216C44-F828-4D59-B398-253DCF86F23C|请求ID。

 |

## 示例 {#demo .section}

请求示例

``` {#request_demo}

http(s)://[Endpoint]/?Action=DescribeFlowNodeInstanceContainerLog
&AppId=application_1542620905989_0010
&ContainerId=container_1542620905989_0010_01_000001
&LogName=stderr
&NodeInstanceId=FNI-0D2534B3AB67****
&ProjectId=FP-BECB9D35CB12****
&RegionId=cn-hangzhou
&<公共请求参数>

```

正常返回示例

`XML` 格式

``` {#xml_return_success_demo}
<DescribeFlowNodeInstanceContainerLogResponse>
	  <data>
		    <RequestId>4E216C44-F828-4D59-B398-253DCF86F23C</RequestId>
		    <LogEnd>false</LogEnd>
		    <LogEntrys>
			      <LogEntry>
				        <Content>2018-11-19 17:55:11,792 INFO [RMCommunicator Allocator] org.apache.hadoop.yarn.util.RackResolver: Resolved emr-worker-1.cluster-500160492 to /default-rack</Content>
			      </LogEntry>
			      <LogEntry>
				        <Content>2018-11-19 17:55:11,793 INFO [RMCommunicator Allocator] org.apache.hadoop.yarn.util.RackResolver: Resolved emr-worker-2.cluster-500160492 to /default-rack</Content>
			      </LogEntry>
		    </LogEntrys>
	  </data>
	  <requestId>4E216C44-F828-4D59-B398-253DCF86F23C</requestId>
</DescribeFlowNodeInstanceContainerLogResponse>
```

`JSON` 格式

``` {#json_return_success_demo}
{
	"requestId":"4E216C44-F828-4D59-B398-253DCF86F23C",
	"data":{
		"LogEntrys":{
			"LogEntry":[
				{
					"Content":"2018-11-19 17:55:11,792 INFO [RMCommunicator Allocator] org.apache.hadoop.yarn.util.RackResolver: Resolved emr-worker-1.cluster-500160492 to /default-rack"
				},
				{
					"Content":"2018-11-19 17:55:11,793 INFO [RMCommunicator Allocator] org.apache.hadoop.yarn.util.RackResolver: Resolved emr-worker-2.cluster-500160492 to /default-rack"
				}
			]
		},
		"RequestId":"4E216C44-F828-4D59-B398-253DCF86F23C",
		"LogEnd":false
	}
}
```

## 错误码 { .section}

访问[错误中心](https://error-center.aliyun.com/status/product/Emr)查看更多错误码。

