# DescribeFlowInstance {#doc_api_Emr_DescribeFlowInstance .reference}

调用 DescribeFlowInstance 接口，获取工作流实例信息。

## 调试 {#api_explorer .section}

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=DescribeFlowInstance&type=RPC&version=2016-04-08)

## 请求参数 {#parameters .section}

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DescribeFlowInstance|系统规定参数。取值：DescribeFlowInstance。

 |
|Id|String|是|FI-7CAF9709CD32\*\*\*\*|工作流实例ID。

 |
|ProjectId|String|是|FP-7A1018ADE917\*\*\*\*|项目ID。

 |
|RegionId|String|是|cn-hangzhou|地域ID。

 |

## 返回数据 {#resultMapping .section}

|名称|类型|示例值|描述|
|--|--|---|--|
|ClusterId|String|C-F32FB31D8295\*\*\*\*|集群ID。

 |
|CronExpression|String|\* \* \* \* \* ?|Cron表达式。

 |
|DependencyFlowList| | |依赖工作流列表。

 |
|ParentFlow| | |依赖工作流列表。

 |
|BizDate|Long|1540796248000|所依赖的工作流实例的业务时间。

 |
|DependencyFlowId|String|F-96853646C4559D42|依赖工作流ID。

 |
|DependencyInstanceId|String|FI-96853646C4559D42|依赖工作流实例ID。

 |
|FlowId|String|F-96853646C4559D49|工作流ID。

 |
|FlowInstanceId|String|FI-96853646C4559D49|工作流实例ID。

 |
|ProjectId|String|FP-96853646C4559D49|项目ID。

 |
|ScheduleKey|String|20191212|调度时间键值。

 |
|Duration|Long|12000|运行持续时间，单位**ms**。

 |
|EndTime|Long|1540796248000|运行结束时间。

 |
|FlowId|String|F-35683D0E4573\*\*\*\*|工作流ID。

 |
|FlowName|String|flow-hive|工作流名称。

 |
|GmtCreate|Long|1540796248000|创建时间。

 |
|GmtModified|Long|1540796248000|修改时间。

 |
|Graph|String|\{"nodes":\[\{"shape":"startControlNode","size":"80\*34","spmAnchorId":"0.0.0.i0.766645eb2cmNtQ","x":500,"y":250,"index":0,"id":"48d474ea","attribute":\{"type":"START"\},"type":"node"\},\{"shape":"hiveSQLJobNode","size":"170\*34","spmAnchorId":"5176.8250060.0.i11.499128d0KWdQvq","x":515,"y":334.5,"index":1,"id":"2f089966","label":"hive-test","attribute":\{"jobId":"FJ-C6C794219DE652B9","type":"JOB","jobType":"HIVE\_SQL"\},"type":"node","config":\{"hostName":"","clusterId":""\}\},\{"shape":"endControlNode","size":"80\*34","spmAnchorId":"5176.8250060.0.i15.499128d0KWdQvq","x":532,"y":453.5,"index":2,"id":"ac092a54","attribute":\{"type":"END"\},"type":"node"\}\],"edges":\[\{"sourceAnchor":0,"targetAnchor":0,"index":3,"source":"48d474ea","id":"77e6117c","target":"2f089966"\},\{"sourceAnchor":1,"targetAnchor":0,"index":4,"source":"2f089966","id":"95ba3716","target":"ac092a54"\}\]\}|图形信息。

 |
|HasNodeFailed|Boolean|false|已过期。

 |
|Id|String|FI-7CAF9709CD32\*\*\*\*|工作流实例ID。

 |
|NodeInstance| | |工作流实例运行的节点实例。

 |
|NodeInstance| | |工作流实例运行的节点实例。

 |
|ClusterId|String|C-F32FB31D82954C64|集群ID。

 |
|Duration|Long|11000|运行持续时间。

 |
|EndTime|Long|1540796248000|运行结束时间。

 |
|ExternalId|String|application\_1540362938289\_1858|外部ID, 例如启动器**ApplicationId**。

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
|FailAct|String|STOP|失败策略：

 -   STOP（终止工作流）。
-   CONTINUE（跳过）。

 |
|GmtCreate|Long|1540796236000|创建时间。

 |
|GmtModified|Long|1540796247000|修改时间。

 |
|HostName|String|emr-header-1.cluster-12345|节点实例运行所在的机器hostName。支持**Master**和**Gateway**机器。**Hostname**格式为 **emr-header-1.cluster-12345**。可登陆机器用**Hostname**命令查看对应的值

 |
|Id|String|FNI-9D14A7CCF2687B84|节点实例 ID

 |
|JobId|String|FJ-C6C794219DE652B9|对应的作业 ID

 |
|JobName|String|hive-test|对应的作业名称

 |
|JobType|String|HIVE\_SQL|对应的作业类型

 |
|MaxRetry|String|0|最大重试次数

 |
|NodeName|String|2f089966|节点名称

 |
|Pending|Boolean|true|是否结束

 |
|ProjectId|String|FP-7A1018ADE9179EE1|项目 ID

 |
|Retries|Integer|0|重试次数

 |
|RetryInterval|String|15|重试间隔，单位 s

 |
|StartTime|Long|1540796237000|运行开始时间

 |
|Status|String|OK|状态：PREP\(等待启动\)，SUBMITTING\(提交中\)，RUNNING\(运行中\)，DONE\(已完成\)，OK\(执行成功\)，FAILED\(执行失败\)，KILLED\(已终止\)，KILL\_FAILED\(终止失败\)，START\_RETRY\(开始重试\)

 |
|Type|String|JOB|节点类型： JOB\(作业\)，CLUSTER\(集群\)。START\(开始\)，END\(结束\)

 |
|ProjectId|String|FP-7A1018ADE917\*\*\*\*|项目ID。

 |
|RequestId|String|EDF99BA3-F7AF-49B2-ABA1-36430A31F482|请求ID。

 |
|ScheduleTime|Long|1540796236000|调度时间。

 |
|StartTime|Long|1540796236000|运行开始时间。

 |
|Status|String|SUCCEEDED|状态：

 -   PREP（准备中）。
-   RUNNING（运行中）。
-   SUCCEEDED（成功）。
-   FAILED（失败）。
-   KILLED（已终止）。
-   SUSPENDED（暂停中）。

 |

## 示例 {#demo .section}

请求示例

``` {#request_demo}

http(s)://[Endpoint]/?Action=DescribeFlowInstance
&Id=FI-7CAF9709CD32****
&ProjectId=FP-7A1018ADE917****
&RegionId=cn-hangzhou
&<公共请求参数>

```

正常返回示例

`XML` 格式

``` {#xml_return_success_demo}
<DescribeFlowInstanceResponse>
	  <data>
		    <Status>SUCCEEDED</Status>
		    <RequestId>EDF99BA3-F7AF-49B2-ABA1-36430A31F482</RequestId>
		    <EndTime>1540796248000</EndTime>
		    <ClusterId>C-F32FB31D8295****</ClusterId>
		    <GmtModified>1540796247000</GmtModified>
		    <StartTime>1540796236000</StartTime>
		    <ProjectId>FP-7A1018ADE917****</ProjectId>
		    <Duration>12000</Duration>
		    <FlowId>F-35683D0E4573****</FlowId>
		    <GmtCreate>1540796235000</GmtCreate>
		    <Graph>
			      <nodes>
				        <shape>startControlNode</shape>
				        <size>80*34</size>
				        <spmAnchorId>0.0.0.i0.766645eb2c****</spmAnchorId>
				        <x>500</x>
				        <y>250</y>
				        <index>0</index>
				        <id>48d4****</id>
				        <attribute>
					          <type>START</type>
				        </attribute>
				        <type>node</type>
			      </nodes>
			      <nodes>
				        <shape>hiveSQLJobNode</shape>
				        <size>170*34</size>
				        <spmAnchorId>5176.8250060.0.i11.499128d0KW****</spmAnchorId>
				        <x>515</x>
				        <y>334.5</y>
				        <index>1</index>
				        <id>2f089966</id>
				        <label>hive-test</label>
				        <attribute>
					          <jobId>FJ-C6C794219DE6****</jobId>
					          <type>JOB</type>
					          <jobType>HIVE_SQL</jobType>
				        </attribute>
				        <type>node</type>
				        <config>
					          <hostName></hostName>
					          <clusterId></clusterId>
				        </config>
			      </nodes>
			      <nodes>
				        <shape>endControlNode</shape>
				        <size>80*34</size>
				        <spmAnchorId>5176.8250060.0.i15.499128d0KW****</spmAnchorId>
				        <x>532</x>
				        <y>453.5</y>
				        <index>2</index>
				        <id>ac09****</id>
				        <attribute>
					          <type>END</type>
				        </attribute>
				        <type>node</type>
			      </nodes>
			      <edges>
				        <sourceAnchor>0</sourceAnchor>
				        <targetAnchor>0</targetAnchor>
				        <index>3</index>
				        <source>48d474ea</source>
				        <id>77e6****</id>
				        <target>2f089966</target>
			      </edges>
			      <edges>
				        <sourceAnchor>1</sourceAnchor>
				        <targetAnchor>0</targetAnchor>
				        <index>4</index>
				        <source>2f089966</source>
				        <id>95ba****</id>
				        <target>ac092a54</target>
			      </edges>
		    </Graph>
		    <FlowName>flow-hive</FlowName>
		    <Id>FI-7CAF9709CD328EBE</Id>
		    <NodeInstance>
			      <NodeInstance>
				        <FailAct>STOP</FailAct>
				        <Status>OK</Status>
				        <EndTime>1540796236000</EndTime>
				        <ClusterId>C-F32FB31D8295****</ClusterId>
				        <NodeName>:start:</NodeName>
				        <GmtModified>1540796236000</GmtModified>
				        <StartTime>1540796236000</StartTime>
				        <ProjectId>FP-7A1018ADE917****</ProjectId>
				        <Duration>0</Duration>
				        <MaxRetry>0</MaxRetry>
				        <Type>START</Type>
				        <GmtCreate>1540796236000</GmtCreate>
				        <Retries>0</Retries>
				        <Id>FNI-1286C1ED46C4****</Id>
				        <HostName></HostName>
				        <Pending>false</Pending>
			      </NodeInstance>
			      <NodeInstance>
				        <FailAct>STOP</FailAct>
				        <Status>OK</Status>
				        <EndTime>1540796248000</EndTime>
				        <ClusterId>C-F32FB31D8295****</ClusterId>
				        <NodeName>2f089966</NodeName>
				        <ExternalId>application_1540362938289_1858</ExternalId>
				        <JobName>hive-test</JobName>
				        <GmtModified>1540796247000</GmtModified>
				        <StartTime>1540796237000</StartTime>
				        <ProjectId>FP-7A1018ADE917****</ProjectId>
				        <Duration>11000</Duration>
				        <MaxRetry>0</MaxRetry>
				        <ExternalStatus>SUCCESS</ExternalStatus>
				        <Type>JOB</Type>
				        <GmtCreate>1540796236000</GmtCreate>
				        <JobType>HIVE_SQL</JobType>
				        <ExternalInfo></ExternalInfo>
				        <Retries>0</Retries>
				        <RetryInterval>15</RetryInterval>
				        <Id>FNI-9D14A7CCF268****</Id>
				        <HostName>emr-header-1.cluster-500159692</HostName>
				        <JobId>FJ-C6C794219DE6****</JobId>
				        <Pending>false</Pending>
			      </NodeInstance>
			      <NodeInstance>
				        <FailAct>STOP</FailAct>
				        <Status>OK</Status>
				        <EndTime>1540796248000</EndTime>
				        <ClusterId>C-F32FB31D8295****</ClusterId>
				        <NodeName>ac092a54</NodeName>
				        <GmtModified>1540796247000</GmtModified>
				        <StartTime>1540796248000</StartTime>
				        <ProjectId>FP-7A1018ADE917****</ProjectId>
				        <Duration>0</Duration>
				        <MaxRetry>0</MaxRetry>
				        <Type>END</Type>
				        <GmtCreate>1540796247000</GmtCreate>
				        <Retries>0</Retries>
				        <Id>FNI-14C14E312AF9****</Id>
				        <HostName></HostName>
				        <Pending>false</Pending>
			      </NodeInstance>
		    </NodeInstance>
	  </data>
	  <requestId>EDF99BA3-F7AF-49B2-ABA1-36430A31F482</requestId>
</DescribeFlowInstanceResponse>
```

`JSON` 格式

``` {#json_return_success_demo}
{
	"requestId":"EDF99BA3-F7AF-49B2-ABA1-36430A31F482",
	"data":{
		"NodeInstance":{
			"NodeInstance":[
				{
					"FailAct":"STOP",
					"Retries":0,
					"Type":"START",
					"GmtCreate":1540796236000,
					"GmtModified":1540796236000,
					"HostName":"",
					"Status":"OK",
					"MaxRetry":"0",
					"ClusterId":"C-F32FB31D8295****",
					"Duration":0,
					"Id":"FNI-1286C1ED46C4****",
					"EndTime":1540796236000,
					"StartTime":1540796236000,
					"Pending":false,
					"ProjectId":"FP-7A1018ADE917****",
					"NodeName":":start:"
				},
				{
					"FailAct":"STOP",
					"Retries":0,
					"Type":"JOB",
					"ExternalId":"application_1540362938289_1858",
					"JobName":"hive-test",
					"GmtCreate":1540796236000,
					"GmtModified":1540796247000,
					"HostName":"emr-header-1.cluster-500159692",
					"JobType":"HIVE_SQL",
					"Status":"OK",
					"MaxRetry":"0",
					"RetryInterval":"15",
					"ClusterId":"C-F32FB31D8295****",
					"JobId":"FJ-C6C794219DE6****",
					"Duration":11000,
					"Id":"FNI-9D14A7CCF268****",
					"EndTime":1540796248000,
					"ExternalInfo":"",
					"StartTime":1540796237000,
					"ExternalStatus":"SUCCESS",
					"Pending":false,
					"ProjectId":"FP-7A1018ADE917****",
					"NodeName":"2f089966"
				},
				{
					"FailAct":"STOP",
					"Retries":0,
					"Type":"END",
					"GmtCreate":1540796247000,
					"GmtModified":1540796247000,
					"HostName":"",
					"Status":"OK",
					"MaxRetry":"0",
					"ClusterId":"C-F32FB31D8295****",
					"Duration":0,
					"Id":"FNI-14C14E312AF94FB8",
					"EndTime":1540796248000,
					"StartTime":1540796248000,
					"Pending":false,
					"ProjectId":"FP-7A1018ADE917****",
					"NodeName":"ac092a54"
				}
			]
		},
		"FlowId":"F-35683D0E4573****",
		"GmtCreate":1540796235000,
		"GmtModified":1540796247000,
		"Status":"SUCCEEDED",
		"ClusterId":"C-F32FB31D8295****",
		"FlowName":"flow-hive",
		"RequestId":"EDF99BA3-F7AF-49B2-ABA1-36430A31F482",
		"Duration":12000,
		"Graph":{
			"edges":[
				{
					"id":"77e6****",
					"index":3,
					"source":"48d474ea",
					"sourceAnchor":0,
					"target":"2f089966",
					"targetAnchor":0
				},
				{
					"id":"95ba****",
					"index":4,
					"source":"2f089966",
					"sourceAnchor":1,
					"target":"ac092a54",
					"targetAnchor":0
				}
			],
			"nodes":[
				{
					"id":"48d4****",
					"index":0,
					"spmAnchorId":"0.0.0.i0.766645eb2c****",
					"attribute":{
						"type":"START"
					},
					"shape":"startControlNode",
					"type":"node",
					"y":250,
					"x":500,
					"size":"80*34"
				},
				{
					"id":"2f089966",
					"index":1,
					"spmAnchorId":"5176.8250060.0.i11.499128d0KW****",
					"config":{
						"hostName":"",
						"clusterId":""
					},
					"attribute":{
						"jobType":"HIVE_SQL",
						"jobId":"FJ-C6C794219DE6****",
						"type":"JOB"
					},
					"label":"hive-test",
					"shape":"hiveSQLJobNode",
					"type":"node",
					"y":334.5,
					"x":515,
					"size":"170*34"
				},
				{
					"id":"ac09****",
					"index":2,
					"spmAnchorId":"5176.8250060.0.i15.499128d0KW****",
					"attribute":{
						"type":"END"
					},
					"shape":"endControlNode",
					"type":"node",
					"y":453.5,
					"x":532,
					"size":"80*34"
				}
			]
		},
		"EndTime":1540796248000,
		"Id":"FI-7CAF9709CD328EBE",
		"StartTime":1540796236000,
		"ProjectId":"FP-7A1018ADE917****"
	}
}
```

## 错误码 { .section}

访问[错误中心](https://error-center.alibabacloud.com/status/product/Emr)查看更多错误码。

