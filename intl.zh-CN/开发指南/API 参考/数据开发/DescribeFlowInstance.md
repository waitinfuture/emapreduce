# DescribeFlowInstance {#concept_gqg_thw_fgb .concept}

获取工作流实例信息参数及示例

## 请求参数 {#section_obj_yxl_2gb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Type|String|是|FLOW|目录类型：FLOW，JOB，ADHOC|
|ProjectId|String|是|FP-ABD24A6163D31274|项目 ID|
|RegionId|String|是|cn-hangzhou|区域 ID|

## 返回参数 {#section_vbj_yxl_2gb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|EDF99BA3-F7AF-49B2-ABA1-36430A31F482|请求 ID|
|Id|String|F-String|工作流实例 ID|
|GmtCreate|Long|1540796248000|创建时间|
|GmtModified|Long|1540796248000|修改时间|
|FlowId|String|F-35683D0E45734E34|工作流 ID|
|FlowName|String|flow-hive|工作流名称|
|ProjectId|String|FP-7A1018ADE9179EE1|项目 ID|
|Status|String|SUCCEEDED|状态：PREP\(准备中\)，RUNNING\(运行中\)，SUCCEEDED\(成功\)，FAILED\(失败\)，KILLED\(已终止\)，SUSPENDED\(暂停中\)|
|ClusterId|String|C-F32FB31D82954C64|集群 ID|
|StartTime|Long|1540796236000|运行开始时间|
|EndTime|Long|1540796248000|运行结束时间|
|Duration|Long|12000|运行持续时间，单位 ms|
|Graph|String|\{"nodes":\[\{"shape":"startControlNode","size":"80\*34","spmAnchorId":"0.0.0.i0.766645eb2cmNtQ","x":500,"y":250,"index":0,"id":"48d474ea","attribute":\{"type":"START"\},"type":"node"\},\{"shape":"hiveSQLJobNode","size":"170\*34","spmAnchorId":"5176.8250060.0.i11.499128d0KWdQvq","x":515,"y":334.5,"index":1,"id":"2f089966","label":"hive-test","attribute":\{"jobId":"FJ-C6C794219DE652B9","type":"JOB","jobType":"HIVE\_SQL"\},"type":"node","config":\{"hostName":"","clusterId":""\}\},\{"shape":"endControlNode","size":"80\*34","spmAnchorId":"5176.8250060.0.i15.499128d0KWdQvq","x":532,"y":453.5,"index":2,"id":"ac092a54","attribute":\{"type":"END"\},"type":"node"\}\],"edges":\[\{"sourceAnchor":0,"targetAnchor":0,"index":3,"source":"48d474ea","id":"77e6117c","target":"2f089966"\},\{"sourceAnchor":1,"targetAnchor":0,"index":4,"source":"2f089966","id":"95ba3716","target":"ac092a54"\}\]\}|图形信息|
|NodeInstance| | |工作流实例运行的节点实例|
|Id|String|FNI-9D14A7CCF2687B84|节点实例 ID|
|GmtCreate|Long|1540796236000|创建时间|
|GmtModified|Long|1540796247000|修改时间|
|Type|String|JOB|节点类型： JOB\(作业\)。CLUSTER\(集群\)。START\(开始\)。END\(结束\)|
|Status|String|OK|状态：PREP\(等待启动\)，SUBMITTING\(提交中\)，RUNNING\(运行中\)，DONE\(已完成\)，OK\(执行成功\)，FAILED\(执行失败\)，KILLED\(已终止\)，KILL\_FAILED\(终止失败\)，START\_RETRY\(开始重试\)|
|JobId|String|FJ-C6C794219DE652B9|对应的作业 ID|
|JobName|String|hive-test|对应的作业名称|
|JobType|String|HIVE\_SQL|对应的作业类型|
|FailAct|String|STOP|失败策略: STOP\(终止工作流\)，CONTINUE\(跳过\)；|
|MaxRetry|String|0|最大重试次数|
|RetryInterval|String|15|重试间隔,单位 s|
|NodeName|String|2f089966|节点名称|
|ClusterId|String|C-F32FB31D82954C64|集群 ID|
|HostName|String|emr-header-1.cluster-12345|节点实例运行所在的机器hostName， 支持 master 和 gateway 机器。 Hostname 格式为 emr-header-1.cluster-12345，可登陆机器用 Hostname 命令查看对应的值|
|ProjectId|String|FP-7A1018ADE9179EE1|项目 ID|
|Pending|Boolean|true|是否结束|
|StartTime|Long|1540796237000|运行开始时间|
|EndTime|Long|1540796248000|运行结束时间|
|Duration|Long|11000|运行持续时间|
|Retries|Integer|0|重试次数|
|ExternalId|String|application\_1540362938289\_1858|外部Id， 例如启动器ApplicationId|
|ExternalStatus|String|SUCCESS|外部状态： SUBMITTED\(已提交\)，RUNNING\(运行中\)，SUCCESS\(执行成功\)，FAIL\(执行失败\)，KILL\_FAIL\(终止失败\)，KILL\_SUCCESS\(终止成功\)|
|ExternalInfo|String|empty|外部信息, 例如运行作业的错误诊断信息|

## 示例 {#section_dcj_yxl_2gb .section}

-   请求示例

    ```
    
    /?Action=DescribeFlowInstance
    &Id=FI-7CAF9709CD328EBE
    &ProjectId=FP-7A1018ADE9179EE1
    &RegionId=cn-hangzhou
    &<公共请求参数>
    ```

-   正常返回示例

    JSON 格式

    ```
    {
    	"code":"200",
    	"data":{
    		"ClusterId":"C-F32FB31D82954C64",
    		"Duration":12000,
    		"EndTime":1540796248000,
    		"FlowId":"F-35683D0E45734E34",
    		"FlowName":"flow-hive",
    		"GmtCreate":1540796235000,
    		"GmtModified":1540796247000,
    		"Graph":{
    			"edges":[
    				{
    					"id":"77e6117c",
    					"index":3,
    					"source":"48d474ea",
    					"sourceAnchor":0,
    					"target":"2f089966",
    					"targetAnchor":0
    				},
    				{
    					"id":"95ba3716",
    					"index":4,
    					"source":"2f089966",
    					"sourceAnchor":1,
    					"target":"ac092a54",
    					"targetAnchor":0
    				}
    			],
    			"nodes":[
    				{
    					"attribute":{
    						"type":"START"
    					},
    					"id":"48d474ea",
    					"index":0,
    					"shape":"startControlNode",
    					"size":"80*34",
    					"spmAnchorId":"0.0.0.i0.766645eb2cmNtQ",
    					"type":"node",
    					"x":500,
    					"y":250
    				},
    				{
    					"attribute":{
    						"jobId":"FJ-C6C794219DE652B9",
    						"jobType":"HIVE_SQL",
    						"type":"JOB"
    					},
    					"config":{
    						"clusterId":"",
    						"hostName":""
    					},
    					"id":"2f089966",
    					"index":1,
    					"label":"hive-test",
    					"shape":"hiveSQLJobNode",
    					"size":"170*34",
    					"spmAnchorId":"5176.8250060.0.i11.499128d0KWdQvq",
    					"type":"node",
    					"x":515,
    					"y":334.5
    				},
    				{
    					"attribute":{
    						"type":"END"
    					},
    					"id":"ac092a54",
    					"index":2,
    					"shape":"endControlNode",
    					"size":"80*34",
    					"spmAnchorId":"5176.8250060.0.i15.499128d0KWdQvq",
    					"type":"node",
    					"x":532,
    					"y":453.5
    				}
    			]
    		},
    		"Id":"FI-7CAF9709CD328EBE",
    		"NodeInstance":{
    			"NodeInstance":[
    				{
    					"ClusterId":"C-F32FB31D82954C64",
    					"Duration":0,
    					"EndTime":1540796236000,
    					"FailAct":"STOP",
    					"GmtCreate":1540796236000,
    					"GmtModified":1540796236000,
    					"HostName":"",
    					"Id":"FNI-1286C1ED46C4577A",
    					"MaxRetry":"0",
    					"NodeName":":start:",
    					"Pending":false,
    					"ProjectId":"FP-7A1018ADE9179EE1",
    					"Retries":0,
    					"StartTime":1540796236000,
    					"Status":"OK",
    					"Type":"START"
    				},
    				{
    					"ClusterId":"C-F32FB31D82954C64",
    					"Duration":11000,
    					"EndTime":1540796248000,
    					"ExternalId":"application_1540362938289_1858",
    					"ExternalInfo":"",
    					"ExternalStatus":"SUCCESS",
    					"FailAct":"STOP",
    					"GmtCreate":1540796236000,
    					"GmtModified":1540796247000,
    					"HostName":"emr-header-1.cluster-500159692",
    					"Id":"FNI-9D14A7CCF2687B84",
    					"JobId":"FJ-C6C794219DE652B9",
    					"JobName":"hive-test",
    					"JobType":"HIVE_SQL",
    					"MaxRetry":"0",
    					"NodeName":"2f089966",
    					"Pending":false,
    					"ProjectId":"FP-7A1018ADE9179EE1",
    					"Retries":0,
    					"RetryInterval":"15",
    					"StartTime":1540796237000,
    					"Status":"OK",
    					"Type":"JOB"
    				},
    				{
    					"ClusterId":"C-F32FB31D82954C64",
    					"Duration":0,
    					"EndTime":1540796248000,
    					"FailAct":"STOP",
    					"GmtCreate":1540796247000,
    					"GmtModified":1540796247000,
    					"HostName":"",
    					"Id":"FNI-14C14E312AF94FB8",
    					"MaxRetry":"0",
    					"NodeName":"ac092a54",
    					"Pending":false,
    					"ProjectId":"FP-7A1018ADE9179EE1",
    					"Retries":0,
    					"StartTime":1540796248000,
    					"Status":"OK",
    					"Type":"END"
    				}
    			]
    		},
    		"ProjectId":"FP-7A1018ADE9179EE1",
    		"RequestId":"EDF99BA3-F7AF-49B2-ABA1-36430A31F482",
    		"StartTime":1540796236000,
    		"Status":"SUCCEEDED"
    	},
    	"requestId":"EDF99BA3-F7AF-49B2-ABA1-36430A31F482",
    	"successResponse":true
    }
    ```

-   异常返回示例

    JSON 格式

    ```
    {
    	"code":"FLOW_API_FAILED",
    	"message":"flow instance does not exist [FI-7CAF9709CD328EB]",
    	"requestId":"9AEDC439-1F63-491D-B8C6-9737C372BF3A",
    	"successResponse":false
    }
    ```


## 错误码 {#section_fcj_yxl_2gb .section}

[查看本产品错误码](https://error-center.alibabacloud.com/status/product/Emr)

