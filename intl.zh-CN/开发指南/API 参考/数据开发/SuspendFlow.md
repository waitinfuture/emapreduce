# SuspendFlow {#concept_rf4_1pl_ggb .concept}

暂停工作流参数及示例

## 请求参数 {#section_obj_yxl_2gb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|FlowInstanceId|String|是|FI-9DDAAA3ADA5F6FA2|FI-9DDAAA3ADA5F6FA2|
|ProjectId|String|是|FP-3535FE0BE5224A47|项目 ID|
|RegionId|String|是|cn-hangzhou|区域 ID|

## 返回参数 {#section_vbj_yxl_2gb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|B46F8A2A-B46B-415C-8A9C-B01B99B775A2|请求 ID|
|Data|Boolean|true|结果|

## 示例 {#section_dcj_yxl_2gb .section}

-   请求示例

    ```
    
    /?Action=SuspendFlow
    &FlowInstanceId=FI-9DDAAA3ADA5F6FA2
    &ProjectId=FP-3535FE0BE5224A47
    &RegionId=cn-hangzhou
    &<公共请求参数>
    ```

-   正常返回示例

    JSON 格式

    ```
    {
    	"Data":true,
    	"RequestId":"B46F8A2A-B46B-415C-8A9C-B01B99B775A2"
    }
    ```

-   异常返回示例

    JSON 格式

    ```
    {
    	"code":"FLOW_API_FAILED",
    	"message":"project not exist",
    	"requestId":"11BAFBD8-8509-4177-A26D-407505E73713",
    	"successResponse":false
    }
    ```


## 错误码 {#section_fcj_yxl_2gb .section}

[查看本产品错误码](https://error-center.alibabacloud.com/status/product/Emr)

