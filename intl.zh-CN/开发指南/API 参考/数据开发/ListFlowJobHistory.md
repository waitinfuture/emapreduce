# ListFlowJobHistory {#concept_cwy_cwj_ggb .concept}

调用 ListFlowJobHistory 接口查询作业的运行实例列表

## 请求参数 {#section_obj_yxl_2gb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Id|String|是|FJ-BCCAE48B90CCB37B|作业 ID|
|ProjectId|String|是|FP-257A173659F59685|项目 ID|
|RegionId|String|是|cn-hangzhou|区域 ID|
|PageNumber|Integer|否|1|页码|
|PageSize|Integer|否|20|每页查询数量|

## 返回参数 {#section_vbj_yxl_2gb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|F5540D8F-06E8-4E3C-B47A-D75CED72A795|请求 ID|
|PageNumber|Integer|1|页码|
|PageSize|Integer|20|每页数量|
|Total|Integer|12|总数|
|NodeInstances| | |作业实例列表|
|Id|String|FNI-9D14A7CCF2687B84|作业实例 ID|
|GmtCreate|Long|1540796248000|创建时间|
|GmtModified|Long|1540796248000|修改时间|
|Type|String|JOB|节点类型: JOB\(作业\)，CLUSTER\(集群\)，START\(开始\)，END\(结束\)|
|Status|String|OK|状态：PREP\(等待启动\)，SUBMITTING\(提交中\)，RUNNING\(运行中\)，DONE\(已完成\)，OK\(执行成功\)，FAILED\(执行失败\)，KILLED\(已终止\)，KILL\_FAILED\(终止失败\)，START\_RETRY\(开始重试\)|
|JobId|String|FJ-A23BD131A862F184|作业 ID|
|JobName|String|myJob|作业名称|
|JobType|String|HIVE\_SQL|作业类型|
|JobParams|String|ls -l|作业内容|
|FailAct|String|STOP|失败策略：STOP\(终止工作流\)，CONTINUE\(跳过\)|
|MaxRetry|Integer|0|最大重试次数|
|RetryInterval|Long|200|重试次数|
|ClusterId|String|C-A6C9F4F1E9EC88D9|集群 ID|
|HostName|String|emr-header-1.cluster-12345|节点实例运行所在的机器 hostName，支持 master 和 gateway 机器。 Hostname格式为 emr-header-1.cluster-12345，可登陆机器用 hostname 命令查看对应的值。|
|ProjectId|String|FP-3535FE0BE5224A47|项目 ID|
|StartTime|Long|1540796237000|运行开始时间|
|EndTime|Long|1540796248000|运行结束时间|
|pending|Boolean|false|是否结束|
|Retries|Integer|0|重试次数|
|ExternalId|String|application\_1541559535023\_34027|启动器的 application 的 ID|
|ExternalStatus|String|SUCCESS|外部状态：SUBMITTED\(已提交\)，RUNNING\(运行中\)，SUCCESS\(执行成功\)，FAIL\(执行失败\)，KILL\_FAIL\(终止失败\)，KILL\_SUCCESS\(终止成功\)|
|ExternalInfo|String|empty|外部信息，例如运行作业的错误诊断信息|
|ParamConf|String|\{"date":"$\{yyyy-MM-dd\}"\}|参数配置|
|EnvConf|String|\{"key":"value"\}|环境变量|
|RunConf|String|\{"priority":1,"userName":"hadoop","memory":2048,"cores":1\}|运行配置 priority： 优先级 ；userName：提交作业的 Linux 用户； memory：内存 单位为 MB； cores： 核数|

## 示例 {#section_dcj_yxl_2gb .section}

-   请求示例

    ```
    
    /?Action=ListFlowJobHistory
    Id=FJ-BCCAE48B90CCB37B
    &ProjectId=FP-257A173659F59685
    &RegionId=cn-hangzhou
    &PageNumber=1
    &PageSize=20
    &<公共请求参数>
    ```

-   正常返回示例

    JSON 格式

    ```
    {
    	"NodeInstances":{
    		"NodeInstance":[{
    			"ClusterId":"C-A6C9F4F1E9EC88D9",
    			"EndTime":1542957514000,
    			"ExternalId":"application_1542955685866_0003",
    			"ExternalInfo":"",
    			"ExternalStatus":"SUCCESS",
    			"FailAct":"STOP",
    			"GmtCreate":1542957499000,
    			"GmtModified":1542957514000,
    			"HostName":"emr-header-2.cluster-500160670",
    			"Id":"FJI-F4FC53D7207E4BEF",
    			"JobId":"FJ-31BD66C7BC502815",
    			"JobName":"success",
    			"JobType":"SHELL",
    			"MaxRetry":0,
    			"ParamConf":"{\"cyctime\":\"2018-11-23 15:18:19\"}",
    			"ProjectId":"FP-17AB3389E1AD9A34",
    			"Retries":0,
    			"RetryInterval":0,
    			"StartTime":1542957499000,
    			"Status":"OK",
    			"pending":false
    		}]
    	},
    	"PageNumber":1,
    	"PageSize":10,
    	"RequestId":"BCF52B64-007F-4883-BAEA-0499106D07C2",
    	"Total":1
    }
    ```

-   异常返回示例

    JSON 格式

    ```
    {
    	"code":"FLOW_API_FAILED",
    	"message":"Project does not exist [FP-3535FE0BE5224A4]",
    	"requestId":"83B256D4-4E95-454B-AD08-799DF31D5556",
    	"successResponse":false
    }
    ```


## 错误码 {#section_fcj_yxl_2gb .section}

[查看本产品错误码](https://error-center.alibabacloud.com/status/product/Emr)

