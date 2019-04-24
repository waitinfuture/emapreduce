# DescribeFlowNodeInstanceContainerLog {#concept_tmr_2vw_fgb .concept}

调用 DescribeFlowNodeInstanceContainerLog 接口查询节点实例容器日志

## 请求参数 {#section_obj_yxl_2gb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|AppId|String|是|application\_1542620905989\_0010|作业的 Application ID|
|ContainerId|String|是|container\_1542620905989\_0010\_01\_000001|Conintainer ID|
|LogName|String|是|stderr|Log 名称：stderr，syslog，stdout|
|NodeInstanceId|String|是|FNI-0D2534B3AB675B63|节点实例 ID|
|ProjectId|String|是|FP-BECB9D35CB121F32|项目 ID|
|RegionId|String|是|cn-hangzhou|区域 ID|
|Length|Integer|否|200|长度，最大1000|
|Offset|Integer|否|0|偏移|

## 返回参数 {#section_vbj_yxl_2gb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|4E216C44-F828-4D59-B398-253DCF86F23C|请求 ID|
|LogEnd|Boolean|true|日志是否结束|
|LogEntrys| | |日志内容|
|Content|String|2018-11-19 17:55:11,792 INFO \[RMCommunicator Allocator\] org.apache.hadoop.yarn.util.RackResolver: Resolved emr-worker-1.cluster-500160492 to /default-rack|日志实际内容|

## 示例 {#section_dcj_yxl_2gb .section}

-   请求示例

    ```
    
    /?Action=DescribeFlowNodeInstanceContainerLog
    &AppId=application_1542620905989_0010
    &ContainerId=container_1542620905989_0010_01_000001
    &LogName=stderr
    &NodeInstanceId=FNI-0D2534B3AB675B63
    &ProjectId=FP-BECB9D35CB121F32
    &RegionId=cn-hangzhou
    &Length=200
    &Offset=0
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
    					"Content":"2018-11-19 17:55:11,792 INFO [RMCommunicator Allocator] org.apache.hadoop.yarn.util.RackResolver: Resolved emr-worker-1.cluster-500160492 to /default-rack"
    				},
    				{
    					"Content":"2018-11-19 17:55:11,793 INFO [RMCommunicator Allocator] org.apache.hadoop.yarn.util.RackResolver: Resolved emr-worker-2.cluster-500160492 to /default-rack"
    				}
    			]
    		},
    		"RequestId":"4E216C44-F828-4D59-B398-253DCF86F23C"
    	},
    	"requestId":"4E216C44-F828-4D59-B398-253DCF86F23C",
    	"successResponse":true
    }
    ```

-   异常返回示例

    JSON 格式

    ```
    {
    	"code":"FLOW_API_FAILED",
    	"message":"flow node instance does not exist [FNI-0D2534B3AB675B6]",
    	"requestId":"D737D945-7F58-4E9D-8E19-E720556C46D5",
    	"successResponse":false
    }
    ```


## 错误码 {#section_fcj_yxl_2gb .section}

[查看本产品错误码](https://error-center.alibabacloud.com/status/product/Emr)

