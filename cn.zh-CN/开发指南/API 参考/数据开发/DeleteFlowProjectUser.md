# DeleteFlowProjectUser {#concept_vrr_4fv_fgb .concept}

调用 DeleteFlowProjectUser 接口删除项目用户

## 请求参数 {#section_obj_yxl_2gb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|UserName|String|是|subuser1|子账号名称|
|ProjectId|String|是|FP-257A173659F59685|项目 ID|
|RegionId|String|是|cn-hangzhou|区域 ID|

## 返回参数 {#section_vbj_yxl_2gb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|243D5A48-96A5-4C0C-8966-93CBF65635ED|请求 ID|
|Data|Boolean|true|是否成功|

## 示例 {#section_dcj_yxl_2gb .section}

-   请求示例

    ```
    
    /?Action=DeleteFlowProjectUser
    &ProjectId=FP-257A173659F59685
    &RegionId=cn-hangzhou
    &UserName=subuser1
    &<公共请求参数>
    ```

-   正常返回示例

    JSON 格式

    ```
    {
    	"Data":"true",
    	"RequestId":"2670BCFB-925D-4C3E-9994-8D12F7A9F538"
    }
    ```

-   异常返回示例

    JSON 格式

    ```
    {
    	"code":"FLOW_API_FAILED",
    	"message":"Invalid type [INVALID_TYPE]",
    	"requestId":"11BAFBD8-8509-4177-A26D-407505E73713",
    	"successResponse":false
    }
    ```


## 错误码 {#section_fcj_yxl_2gb .section}

[查看本产品错误码](https://error-center.alibabacloud.com/status/product/Emr)

