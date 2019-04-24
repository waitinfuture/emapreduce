# DeleteFlowProjectClusterSetting {#concept_ncw_s2v_fgb .concept}

调用 DeleteFlowProjectClusterSetting 接口删除项目集群设置

## 请求参数 {#section_obj_yxl_2gb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|ClusterId|String|是|C-F32FB31D82954C64|集群 ID|
|ProjectId|String|是|FP-E3F1523F8FC1D2E6|项目 ID|
|RegionId|String|是|cn-hangzhou|区域 ID|

## 返回参数 {#section_vbj_yxl_2gb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|ECC2D0D1-B6D5-468D-B698-30E8805EB574|请求 ID|
|Data|Boolean|true|是否成功|

## 示例 {#section_dcj_yxl_2gb .section}

-   请求示例

    ```
    
    /?Action=DeleteFlowProjectClusterSetting
    &ClusterId=C-F32FB31D82954C64
    &ProjectId=FP-E3F1523F8FC1D2E6
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
    	"message":"Project does not exist [FP-E3F1523F8FC1D2E6]",
    	"requestId":"11BAFBD8-8509-4177-A26D-407505E73713",
    	"successResponse":false
    }
    ```


## 错误码 {#section_fcj_yxl_2gb .section}

[查看本产品错误码](https://error-center.alibabacloud.com/status/product/Emr)

