# CloneFlow {#concept_nk3_pxn_fgb .concept}

克隆工作流参数及示例

## 请求参数 {#section_obj_yxl_2gb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Id|String|是|F-8C1EB0C6452A82A1|克隆的目标工作流 ID|
|ProjectId|String|是|FP-53C4D36FC7310AC0|克隆的目标工作流所属的项目 ID|
|RegionId|String|是|cn-hangzhou|区域 ID|

## 返回参数 {#section_vbj_yxl_2gb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|12E7685E-C7A2-4BED-878B-07AF5BAD475F|请求 ID|
|Id|String|F-CDFE2BF3A7963F06|新产生的工作流 ID|

## 示例 {#section_dcj_yxl_2gb .section}

-   请求示例

    ```
    /?Action=CloneFlow
    &Id=F-8C1EB0C6452A82A1
    &ProjectId=FP-53C4D36FC7310AC0
    &RegionId=cn-hangzhou
    &<公共请求参数>
    ```

-   正常返回示例

    JSON 格式

    ```
    {
    	"Id":"F-CDFE2BF3A7963F06",
    	"RequestId":"12E7685E-C7A2-4BED-878B-07AF5BAD475F"
    }
    ```

-   异常返回示例

    JSON 格式

    ```
    {
    	"code":"FLOW_API_FAILED",
    	"message":"Flow name already exists: [0_copy]",
    	"requestId":"7DF74E3B-5BEA-4365-8E67-4FD95095924F",
    	"successResponse":false
    }
    ```


## 错误码 {#section_fcj_yxl_2gb .section}

[查看本产品错误码](https://error-center.alibabacloud.com/status/product/Emr)

