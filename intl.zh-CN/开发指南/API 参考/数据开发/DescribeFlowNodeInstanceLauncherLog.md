# DescribeFlowNodeInstanceLauncherLog {#concept_lzc_lzw_fgb .concept}

调用 DescribeFlowNodeInstanceLauncherLog 接口查询节点实例启动器日志

## 请求参数 {#section_obj_yxl_2gb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|NodeInstanceId|String|是|FNI-0D2534B3AB675B63|节点实例 ID|
|ProjectId|String|是|FP-BECB9D35CB121F32|项目 ID|
|RegionId|String|是|cn-hangzhou|区域 ID|
|EndTime|Long|否|1540796248000|查询结束时间|
|Length|Integer|否|100|已过期|
|Lines|Integer|否|100|长度，最大限制为 1000|
|Offset|Integer|否|200|已过期|
|Reverse|Boolean|否|true|是否倒序|
|Start|Integer|否|200|起始偏移|
|StartTime|Long|否|1540796236000|查询开始时间|

## 返回参数 {#section_vbj_yxl_2gb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|4E216C44-F828-4D59-B398-253DCF86F23C|请求 ID|
|LogEnd|Boolean|false|日志是否结束|
|LogEntrys| | |日志内容|
|Content|String|2018-11-19 17:55:11,792 INFO \[RMCommunicator Allocator\] org.apache.hadoop.yarn.util.RackResolver: Resolved emr-worker-1.cluster-500160492 to /default-rack|日志实际内容|

## 示例 {#section_dcj_yxl_2gb .section}

-   请求示例

    ```
    
    /?Action=DescribeFlowNodeInstanceLauncherLog
    &NodeInstanceId=FNI-0D2534B3AB675B63
    &ProjectId=FP-BECB9D35CB121F32
    &RegionId=cn-hangzhou
    &EndTime=1540796248000
    &Lines=100
    &Reverse=true
    &Start=200
    &StartTime=1540796236000
    &<公共请求参数>
    ```

-   正常返回示例

    JSON 格式

    ```
    {
    	"code":"200",
    	"data":{
    		"LogEnd":false,
    		"LogEntrys":{
    			"LogEntry":[
    				{
    					"Content":"Fri Nov 16 09:56:00 CST 2018 [LocalJobLauncherAM] INFO Starting emr flow launcher am ..."
    				},
    				{
    					"Content":"== System Properties ============"
    				},
    				{
    					"Content":"  java.runtime.name=OpenJDK Runtime Environment"
    				}
    			]
    		},
    		"RequestId":"F4BBBCEF-20B9-4AD2-99EF-E1BA8D72B2C3"
    	},
    	"requestId":"F4BBBCEF-20B9-4AD2-99EF-E1BA8D72B2C3",
    	"successResponse":true
    }
    ```

-   异常返回示例

    JSON 格式

    ```
    {
    	"code":"FLOW_API_FAILED",
    	"message":"flow instance does not exist [FNI-2DC5BBC3D47FE45]",
    	"requestId":"2D695EEA-DB91-498B-B974-464BCED3293B",
    	"successResponse":false
    }
    ```


## 错误码 {#section_fcj_yxl_2gb .section}

[查看本产品错误码](https://error-center.alibabacloud.com/status/product/Emr)

