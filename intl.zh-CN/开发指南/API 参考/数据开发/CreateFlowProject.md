# CreateFlowProject {#concept_dyp_sjp_fgb .concept}

创建数据开发项目参数及示例

## 请求参数 {#section_obj_yxl_2gb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Description|String|是|这是一个项目描述|项目描述|
|Name|String|是|my\_project|项目名称|
|RegionId|String|是|cn-hangzhou|区域|

## 返回参数 {#section_vbj_yxl_2gb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|2670BCFB-925D-4C3E-9994-8D12F7A9F538|请求 ID|
|Id|String|FP-257A173659F59685|项目 ID|

## 示例 {#section_dcj_yxl_2gb .section}

-   请求示例

    ```
    /?Action=CreateFlowProject
    &Description=这是一个项目描述
    &Name=my_project
    &RegionId=cn-hangzhou
    &<公共请求参数>
    ```

-   正常返回示例

    JSON 格式

    ```
    {
    	"Id":"FP-257A173659F59685",
    	"RequestId":"2670BCFB-925D-4C3E-9994-8D12F7A9F538"
    }
    ```

-   异常返回示例

    JSON 格式

    ```
    {
    	"code":"FLOW_API_FAILED",
    	"message":"Invalid project name [Project name longer than 256]",
    	"requestId":"0AD9FF20-F585-4E0A-870D-B8A8F884CCE6",
    	"successResponse":false
    }
    ```


## 错误码 {#section_fcj_yxl_2gb .section}

[查看本产品错误码](https://error-center.alibabacloud.com/status/product/Emr)

