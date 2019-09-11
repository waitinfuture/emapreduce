# ListFlowNodeInstance {#doc_api_Emr_ListFlowNodeInstance .reference}

调用 ListFlowNodeInstance 接口，查询工作流节点实例列表

查询工作流节点实例列表

## 调试 {#api_explorer .section}

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=ListFlowNodeInstance&type=RPC&version=2016-04-08)

## 请求参数 {#parameters .section}

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ListFlowNodeInstance|系统规定参数。取值：ListFlowNodeInstance。

 |
|RegionId|String|是|cn-hangzhou|地域ID。

 |
|OrderBy|String|否|start\_time|排序字段。

 |
|OrderType|String|否|desc|排序类型。

 |
|PageNumber|Integer|否|1|分页查询的页数。

 |
|PageSize|Integer|否|20|分页查询的每页数量。

 |
|ProjectId|String|否|FP-257A173659F5\*\*\*\*|项目ID。

 |
|StartTime|Long|否|1540796248000|开始时间。

 |
|StatusList.N|RepeatList|否|FAILED|状态列表。

 |

## 返回数据 {#resultMapping .section}

|名称|类型|示例值|描述|
|--|--|---|--|
|FlowNodeInstances| | |节点实例列表。

 |
|FlowNodeInstance| | |节点实例列表。

 |
|ClusterId|String|C-A6C9F4F1E9E\*\*\*\*|集群ID。

 |
|Duration|Long|200|运行持续时间。

 |
|EndTime|Long|1540796248000|运行结束时间。

 |
|ExternalChildIds|String|application\_1541559535023\_34028|任务的Application列表。

 |
|ExternalId|String|application\_1541559535023\_34027|启动器的Application的ID。

 |
|ExternalInfo|String|empty|外部信息，例如运行作业的错误诊断信息。

 |
|ExternalStatus|String|SUCCESS|外部状态：

 -   SUBMITTED（已提交）。
-   RUNNING（运行中）。
-   SUCCESS（执行成功）。
-   FAIL（执行失败）。
-   KILL\_FAIL（终止失败）。
-   KILL\_SUCCESS（终止成功）。

 |
|ExternalSubId|String|container\_1541559535023\_34027\_01\_000001|启动器Container的ID。

 |
|FailAct|String|STOP|失败策略：

 -   STOP（终止工作流）。
-   CONTINUE（跳过）。

 |
|FlowId|String|F-35683D0E45734E34|工作流ID。

 |
|FlowInstanceId|String|FI-7CAF9709CD328EBE|工作流实例ID。

 |
|GmtCreate|Long|1540796236000|创建时间。

 |
|GmtModified|Long|1540796247000|修改时间。

 |
|HostName|String|emr-header-1.cluster-12345|节点实例运行所在的机器HostName，支持Master和Gateway机器。Hostname格式为**emr-header-1.cluster-12345**，可登录机器用**hostname**命令查看对应的值。

 |
|Id|String|FNI-9D14A7CCF2687B84|节点实例ID。

 |
|JobId|String|FJ-A23BD131A862F184|作业ID。

 |
|JobName|String|myJob|作业名称。

 |
|JobParams|String|ls -l|作业内容。

 |
|JobType|String|HIVE\_SQL|作业类型。

 |
|MaxRetry|String|0|最大重试次数。

 |
|NodeName|String|node|节点名称。

 |
|Pending|Boolean|false|是否已经结束。

 |
|ProjectId|String|FP-7A1018ADE917\*\*\*\*|项目ID。

 |
|Retries|Integer|0|重试次数。

 |
|RetryInterval|String|200|重试间隔。

 |
|StartTime|Long|1540796237000|开始运行时间。

 |
|Status|String|OK|状态:

 -   PREP（等待启动）。
-   SUBMITTING（提交中）。
-   RUNNING（运行中）。
-   DONE（已完成）。
-   OK（执行成功）。
-   FAILED（执行失败）。
-   KILLED（已终止）。
-   KILL\_FAILED（终止失败）。
-   START\_RETRY（开始重试）。

 |
|Type|String|JOB|节点类型。

 |
|PageNumber|Integer|1|页码。

 |
|PageSize|Integer|20|每页数量。

 |
|RequestId|String|83B256D4-4E95-454B-AD08-799DF31D5556|请求ID。

 |
|Total|Integer|12|总数。

 |

## 示例 {#demo .section}

请求示例

``` {#request_demo}

http(s)://[Endpoint]/?Action=ListFlowNodeInstance
&RegionId=cn-hangzhou
&<公共请求参数>

```

正常返回示例

`XML` 格式

``` {#xml_return_success_demo}
<ListFlowNodeInstanceResponse>
  <PageSize>10</PageSize>
	  <RequestId>BCF52B64-007F-4883-BAEA-0499106D07C2</RequestId>
	  <PageNumber>1</PageNumber>
	  <Total>1</Total>
	  <FlowNodeInstances>
		    <FlowNodeInstance>
			      <FailAct>STOP</FailAct>
			      <Status>OK</Status>
			      <EndTime>1542957514000</EndTime>
			      <ClusterId>C-A6C9F4F1E9EC****</ClusterId>
			      <ExternalId>application_1542955685866_0003</ExternalId>
			      <pending>false</pending>
			      <JobName>success</JobName>
			      <GmtModified>1542957514000</GmtModified>
			      <StartTime>1542957499000</StartTime>
			      <ProjectId>FP-17AB3389E1AD9A34</ProjectId>
			      <MaxRetry>0</MaxRetry>
			      <ParamConf>{"cyctime":"2018-11-23 15:18:19"}</ParamConf>
			      <ExternalStatus>SUCCESS</ExternalStatus>
			      <GmtCreate>1542957499000</GmtCreate>
			      <JobType>SHELL</JobType>
			      <ExternalInfo></ExternalInfo>
			      <Retries>0</Retries>
			      <RetryInterval>0</RetryInterval>
			      <Id>FJI-F4FC53D7207E4BEF</Id>
			      <HostName>emr-header-2.cluster-500160670</HostName>
			      <JobId>FJ-31BD66C7BC502815</JobId>
		    </FlowNodeInstance>
	  </FlowNodeInstances>
	</ListFlowNodeInstanceResponse>
```

`JSON` 格式

``` {#json_return_success_demo}
{
	"PageNumber":1,
	"FlowNodeInstances":{
		"FlowNodeInstance":[
			{
				"FailAct":"STOP",
				"Retries":0,
				"ExternalId":"application_1542955685866_0003",
				"JobName":"success",
				"GmtCreate":1542957499000,
				"GmtModified":1542957514000,
				"HostName":"emr-header-2.cluster-500160670",
				"JobType":"SHELL",
				"Status":"OK",
				"MaxRetry":0,
				"RetryInterval":0,
				"ClusterId":"C-A6C9F4F1E9EC****",
				"JobId":"FJ-31BD66C7BC502815",
				"pending":false,
				"ParamConf":"{\"cyctime\":\"2018-11-23 15:18:19\"}",
				"Id":"FJI-F4FC53D7207E4BEF",
				"EndTime":1542957514000,
				"ExternalInfo":"",
				"StartTime":1542957499000,
				"ExternalStatus":"SUCCESS",
				"ProjectId":"FP-17AB3389E1AD9A34"
			}
		]
	},
	"PageSize":10,
	"RequestId":"BCF52B64-007F-4883-BAEA-0499106D07C2",
	"Total":1
}
```

## 错误码 { .section}

访问[错误中心](https://error-center.aliyun.com/status/product/Emr)查看更多错误码。

