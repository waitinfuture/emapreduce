# ListFlowInstance {#concept_rg5_lqj_ggb .concept}

调用 ListFlowInstance 接口查询工作流实例列表

## 请求参数 {#section_obj_yxl_2gb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|RegionId|String|是|cn-hangzhou|区域 ID|
|ProjectId|String|是|FP-3535FE0BE5224A47|项目 ID|
|FlowId|String|否|F-1B4018ADE9179EE1|工作流 ID|
|FlowName|String|否|my\_flow|工作流名称|
|Id|String|否|FI-7CAF9709CD328EBE|工作流实例 ID|
|PageNumber|Integer|否|1|页码|
|PageSize|Integer|否|20|每页数量|
|StatusList.N|RepeatList|否|RUNNING|状态列表|

## 返回参数 {#section_vbj_yxl_2gb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|EDF99BA3-F7AF-49B2-ABA1-36430A31F482|请求 ID|
|PageNumber|Integer|1|页码|
|PageSize|Integer|20|每页数量|
|Total|Integer|42|总数|
|FlowInstances| | |工作流实例列表|
|Id|String|FI-7CAF9709CD328EBE|工作流实例 ID|
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

## 示例 {#section_dcj_yxl_2gb .section}

-   请求示例

    ```
    
    /?Action=ListFlowInstance
    &ProjectId=FP-7A1018ADE9179EE1
    &RegionId=cn-hangzhou
    &FlowId=F-1B4018ADE9179EE1
    &FlowName=my_flow
    &Id=FI-7CAF9709CD328EBE
    &PageNumber=1
    &PageSize=20
    &StatusList.1=RUNNING
    &<公共请求参数>
    ```

-   正常返回示例

    JSON 格式

    ```
    {
    	"FlowInstances":{
    		"FlowInstance":[
    			{
    				"ClusterId":"C-A6C9F4F1E9EC88D9",
    				"Duration":32000,
    				"EndTime":1542963033000,
    				"FlowId":"F-9C8D2BEC4D3803BC",
    				"FlowName":"success+fail",
    				"GmtCreate":1542963000000,
    				"GmtModified":1542963033000,
    				"Id":"FI-03F019530CB8D327",
    				"Owner":****,
    				"ProjectId":"FP-17AB3389E1AD9A34",
    				"StartTime":1542963001000,
    				"Status":"FAILED"
    			},
    			{
    				"ClusterId":"C-A6C9F4F1E9EC88D9",
    				"Duration":15000,
    				"EndTime":1542963015000,
    				"FlowId":"F-FECA99A4AE794199",
    				"FlowName":"success",
    				"GmtCreate":1542963000000,
    				"GmtModified":1542963015000,
    				"Id":"FI-60B9DBFA5AD2432B",
    				"Owner":****,
    				"ProjectId":"FP-17AB3389E1AD9A34",
    				"StartTime":1542963000000,
    				"Status":"SUCCEEDED"
    			}
    		]
    	},
    	"PageNumber":1,
    	"PageSize":2,
    	"RequestId":"780BB15A-9038-4330-BE6C-0A47FE5A1BBF",
    	"Total":50
    }
    ```

-   异常返回示例

    JSON 格式

    ```
    {
    	"code":"FLOW_API_FAILED",
    	"message":"Project does not exist [FP-117AB3389E1AD9A34]",
    	"requestId":"63D6A3AD-8060-4E31-A926-36F2A23888C0",
    	"successResponse":false
    }
    ```


## 错误码 {#section_fcj_yxl_2gb .section}

[查看本产品错误码](https://error-center.alibabacloud.com/status/product/Emr)

