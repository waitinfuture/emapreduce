# ListFlowNodeSqlResult {#concept_wrk_35k_ggb .concept}

调用 ListFlowNodeSqlResult 接口查询节点实例 sql 结果

## 请求参数 {#section_obj_yxl_2gb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|NodeInstanceId|String|是|FE4BD156E939CF1F|节点实例 ID|
|ProjectId|String|是|FP-7A1018ADE9179EE1|项目 ID|
|RegionId|String|是|cn-hangzhou|区域 ID|
|Length|Integer|否|100|长度|
|Offset|Integer|否|0|偏移|
|SqlIndex|Integer|否|0|第几个 sql 结果|

## 返回参数 {#section_vbj_yxl_2gb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|90F14544-832D-4D81-8F8C-8F8547AE5B9D|请求 ID|
|End|Boolean|true|是否结束|
|RowList| | |行数据|
|RowIndex|Integer|0|行号|
|RowItemList|String|\{ "rowItem": \[ "1", "1", "1" \] \}|行内容|
|HeaderList|String|\{ "Header": \[ "rankings.pageurl", "rankings.pagerank", "rankings.avgduration" \] \}|列数据|

## 示例 {#section_dcj_yxl_2gb .section}

-   请求示例

    ```
    
    /?Action=ListFlowNodeSqlResult
    &NodeInstanceId=FE4BD156E939CF1F
    &ProjectId=FP-7A1018ADE9179EE1
    &RegionId=cn-hangzhou
    &Length=100
    &Offset=0
    &SqlIndex=0
    &<公共请求参数>
    ```

-   正常返回示例

    JSON 格式

    ```
    {
    	"End":true,
    	"HeaderList":{
    		"Header":[
    			"rankings.pageurl",
    			"rankings.pagerank",
    			"rankings.avgduration"
    		]
    	},
    	"RequestId":"90F14544-832D-4D81-8F8C-8F8547AE5B9D",
    	"RowList":{
    		"Row":[
    			{
    				"RowIndex":0,
    				"RowItemList":{
    					"rowItem":[
    						"1",
    						"1",
    						"1"
    					]
    				}
    			},
    			{
    				"RowIndex":1,
    				"RowItemList":{
    					"rowItem":[
    						"1",
    						"1",
    						"1"
    					]
    				}
    			}
    		]
    	}
    }
    ```

-   异常返回示例

    JSON 格式

    ```
    {
    	"code":"FLOW_API_FAILED",
    	"message":"Job instance does not exist [FJI-38BA0B24245155A]",
    	"requestId":"67C4D5F0-E8E1-408C-9BD0-4268A721EC3A",
    	"successResponse":false
    }
    ```


## 错误码 {#section_fcj_yxl_2gb .section}

[查看本产品错误码](https://error-center.alibabacloud.com/status/product/Emr)

