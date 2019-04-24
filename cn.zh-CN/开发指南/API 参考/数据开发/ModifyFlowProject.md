# ModifyFlowProject {#concept_q2c_tsq_ggb .concept}

调用 ModifyFlowProject 接口修改数据开发项目

## 请求参数 {#section_obj_yxl_2gb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|ProjectId|String|是|FP-257A173659F59685|项目 ID|
|RegionId|String|是|cn-hangzhou|区域 ID|
|Description|String|否|my\_flow\_description|项目描述|
|Name|String|否|my\_project|项目名称|

## 返回参数 {#section_vbj_yxl_2gb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|11BAFBD8-8509-4177-A26D-407505E73713|请求 ID|
|Data|Boolean|true|结果|

## 示例 {#section_dcj_yxl_2gb .section}

-   请求示例

    ```
    /?ProjectId=FP-257A173659F59685
    &RegionId=cn-hangzhou
    &Description=my flow description
    &Name=my_project
    &<公共请求参数>
    ```

-   正常返回示例

    JSON 格式

    ```
    {
    	"Data":true,
    	"RequestId":"ECC2D0D1-B6D5-468D-B698-30E8805EB574"
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

