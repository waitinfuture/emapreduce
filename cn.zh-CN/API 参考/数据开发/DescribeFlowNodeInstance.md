# DescribeFlowNodeInstance {#doc_api_Emr_DescribeFlowNodeInstance .reference}

调用 DescribeFlowNodeInstance 接口，查询节点实例详情（工作流节点实例, 作业运行节点实例）

## 调试 {#api_explorer .section}

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=DescribeFlowNodeInstance&type=RPC&version=2016-04-08)

## 请求参数 {#parameters .section}

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DescribeFlowNodeInstance|系统规定参数。取值：DescribeFlowNodeInstance。

 |
|Id|String|是|FNI-FE4BD156E939\*\*\*\*|节点实例ID。

 |
|ProjectId|String|是|FP-7A1018ADE917\*\*\*\*|项目ID。

 |
|RegionId|String|是|cn-hangzhou|地域ID。

 |

## 返回数据 {#resultMapping .section}

|名称|类型|示例值|描述|
|--|--|---|--|
|Adhoc|Boolean|true|是否临时查询。

 |
|ClusterId|String|C-F32FB31D8295\*\*\*\*|集群ID。

 |
|ClusterName|String|myCluster|集群名称。

 |
|Duration|Long|200|运行持续时间。

 |
|EndTime|Long|1540796248000|运行结束时间。

 |
|EnvConf|String|\{"key":"value"\}|环境变量。

 |
|ExternalChildIds|String|application\_1541559535023\_34028|任务的**application**列表。

 |
|ExternalId|String|application\_1541559535023\_34027|启动器的**application**的ID。

 |
|ExternalInfo|String|empty|外部信息, 例如运行作业的错误诊断信息。

 |
|ExternalStatus|String|SUCCESS|外部状态：

 -   SUBMITTED（已提交）。
-   RUNNING（运行中）。
-   SUCCESS（执行成功）。
-   FAIL（执行失败）。
-   KILL\_FAIL（终止失败）。
-   KILL\_SUCCESS（终止成功）。

 |
|ExternalSubId|String|container\_1541559535023\_34027\_01\_000001|启动器**container**的ID。

 |
|FailAct|String|STOP|失败策略：

 -   STOP（终止工作流）。
-   CONTINUE（跳过）。

 |
|FlowId|String|F-35683D0E4573\*\*\*\*|工作流ID。

 |
|FlowInstanceId|String|FI-7CAF9709CD32\*\*\*\*|工作流实例ID。

 |
|GmtCreate|Long|1540796236000|创建时间。

 |
|GmtModified|Long|1540796247000|修改时间。

 |
|HostName|String|emr-header-1.cluster-12345|节点实例运行所在的机器**hostName**，支持 **Master** 和 **Gateway** 机器。 **hostname**格式为**emr-header-1.cluster-12345**。可登陆机器用**hostname**命令查看对应的值。

 |
|Id|String|FNI-9D14A7CCF268\*\*\*\*|节点实例ID。

 |
|JobId|String|FJ-A23BD131A862\*\*\*\*|作业ID。

 |
|JobName|String|myJob|作业名称。

 |
|JobParams|String|ls -l|作业内容。

 |
|JobType|String|HIVE\_SQL|作业类型。

 |
|MaxRetry|String|0|最大重试次数。

 |
|Mode|String|YARN|模型模式，支持：YARN、LOCAL YARN。将作业包装成一个 launcher提交至YARN中执行LOCAL。作业直接在机器上以进程方式运行。

 |
|MonitorConf|String|\{"inputs":\[\{"type":"KAFKA","clusterId":"C-1234567","topics":"kafka\_topic","consumer.group":"kafka\_consumer\_group"\}\],"outputs":\[\{"type":"KAFKA","clusterId":"C-1234567","topics":"kafka\_topic"\}\]\}|监控配置，只有SPARK\_STREAMING类型作业支持。

 |
|NodeName|String|节点名称|节点名称。

 |
|ParamConf|String|\{"date":"$\{yyyy-MM-dd\}"\}|参数配置。

 |
|Pending|Boolean|true|是否结束。

 |
|ProjectId|String|FP-7A1018ADE917\*\*\*\*|项目ID。

 |
|RequestId|String|1549175a-6d14-4c8a-89f9-5e28300f6d7e|请求ID。

 |
|Retries|Integer|0|重试次数。

 |
|RetryInterval|String|15|重试间隔，单位**s**。

 |
|RunConf|String|\{"priority":1,"userName":"hadoop","memory":2048,"cores":1\}|运行配置

 -   priority： 优先级。
-   userName： 用于提交作业 linux 用户。
-   memory：内存单位为MB。
-   cores： 核数。

 |
|StartTime|Long|1540796237000|运行开始时间。

 |
|Status|String|OK|状态：

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
|Type|String|JOB|节点类型：

 -   JOB（作业）。
-   CLUSTER（集群）。
-   START（开始）。
-   END（结束）。

 |

## 示例 {#demo .section}

请求示例

``` {#request_demo}

http(s)://[Endpoint]/?Action=DescribeFlowNodeInstance
&Id=FNI-FE4BD156E939****
&ProjectId=FP-7A1018ADE917****
&RegionId=cn-hangzhou
&<公共请求参数>

```

正常返回示例

`XML` 格式

``` {#xml_return_success_demo}
<DescribeFlowNodeInstanceResponse>
	  <data>
		    <FailAct>STOP</FailAct>
		    <EndTime>1542884476000</EndTime>
		    <NodeName>812589f5</NodeName>
		    <GmtModified>1542884475000</GmtModified>
		    <JobName>success</JobName>
		    <ExternalStatus>SUCCESS</ExternalStatus>
		    <FlowId>F-60C9B1257A80****</FlowId>
		    <ParamConf>
			      <cyctime>2018-11-22 19:00:00</cyctime>
		    </ParamConf>
		    <ExternalInfo></ExternalInfo>
		    <Retries>0</Retries>
		    <ClusterName>mingbo-v199v1-勿删</ClusterName>
		    <JobId>FJ-EE3AF471B2E6****</JobId>
		    <HostName>emr-header-1.cluster-500159692</HostName>
		    <JobParams>
			sleep 10;
		exit 0;</JobParams>
		    <Status>OK</Status>
		    <RequestId>F5540D8F-06E8-4E3C-B47A-D75CED72A795</RequestId>
		    <ClusterId>C-F32FB31D8295****</ClusterId>
		    <ExternalId>application_1541559535023_34027</ExternalId>
		    <StartTime>1542884402000</StartTime>
		    <ProjectId>FP-3535FE0BE5224A47</ProjectId>
		    <FlowInstanceId>FI-C112BB938D2C****</FlowInstanceId>
		    <Duration>74000</Duration>
		    <ExternalSubId>container_1541559535023_34027_01_000001</ExternalSubId>
		    <MaxRetry>0</MaxRetry>
		    <Type>JOB</Type>
		    <GmtCreate>1542884401000</GmtCreate>
		    <JobType>SHELL</JobType>
		    <RetryInterval>15</RetryInterval>
		    <Id>FNI-E548777B9DCD****</Id>
		    <Pending>false</Pending>
	  </data>
	  <requestId>F5540D8F-06E8-4E3C-B47A-D75CED72A795</requestId>
</DescribeFlowNodeInstanceResponse>
```

`JSON` 格式

``` {#json_return_success_demo}
{
	"requestId":"F5540D8F-06E8-4E3C-B47A-D75CED72A795",
	"data":{
		"FailAct":"STOP",
		"Type":"JOB",
		"GmtModified":1542884475000,
		"FlowInstanceId":"FI-C112BB938D2C****",
		"RetryInterval":"15",
		"ClusterName":"mingbo-v199v1-勿删",
		"ExternalSubId":"container_1541559535023_34027_01_000001",
		"JobId":"FJ-EE3AF471B2E6****",
		"Duration":74000,
		"ExternalInfo":"",
		"StartTime":1542884402000,
		"ExternalStatus":"SUCCESS",
		"NodeName":"812589f5",
		"Retries":0,
		"ExternalId":"application_1541559535023_34027",
		"FlowId":"F-60C9B1257A80****",
		"JobName":"success",
		"GmtCreate":1542884401000,
		"HostName":"emr-header-1.cluster-500159692",
		"JobParams":"sleep 10;\nexit 0;",
		"JobType":"SHELL",
		"Status":"OK",
		"MaxRetry":"0",
		"ClusterId":"C-F32FB31D8295****",
		"RequestId":"F5540D8F-06E8-4E3C-B47A-D75CED72A795",
		"ParamConf":{
			"cyctime":"2018-11-22 19:00:00"
		},
		"Id":"FNI-E548777B9DCD****",
		"EndTime":1542884476000,
		"Pending":false,
		"ProjectId":"FP-3535FE0BE5224A47"
	}
}
```

## 错误码 { .section}

访问[错误中心](https://error-center.aliyun.com/status/product/Emr)查看更多错误码。

