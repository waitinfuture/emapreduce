# DeleteFlow {#concept_hts_ctp_fgb .concept}

删除工作流参数及示例

## 请求参数 {#section_obj_yxl_2gb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Id|String|是|F-7A39731FE7196358|工作流 ID|
|ProjectId|String|是|FP-257A173659F59685|项目 ID|
|RegionId|String|是|cn-hangzhou|区域 ID|

## 返回参数 {#section_vbj_yxl_2gb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|11BAFBD8-8509-4177-A26D-407505E73713|请求 ID|
|Data|Boolean|true|是否成功|

## 示例 {#section_dcj_yxl_2gb .section}

-   请求示例

    ```
    
    /?Action=DeleteFlow
    &Id=F-7A39731FE7196358
    &ProjectId=FP-257A173659F59685
    &RegionId=cn-hangzhou
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
    	"message":"Flow does not exist [F-12345]",
    	"requestId":"11BAFBD8-8509-4177-A26D-407505E73713",
    	"successResponse":false
    }
    ```


## 错误码 {#section_fcj_yxl_2gb .section}

[查看本产品错误码](https://error-center.alibabacloud.com/status/product/Emr)

