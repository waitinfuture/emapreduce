# CreateFlowCategory {#concept_vly_rd4_fgb .concept}

创建目录文件夹参数及示例

## 请求参数 {#section_obj_yxl_2gb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Name|String|是|myFolder|目录名称|
|ProjectId|String|是|F-48EB68215AEAE28D|项目 ID|
|RegionId|String|是|cn-hangzhou|区域 ID|
|Type|String|是|FLOW|类型： FLOW\(工作流\)， JOB\(作业\)，ADHOC\(临时查询\)|
|ParentId|String|否|FC-AF08490649B8FD5E|父目录ID，为空时则默认为 root 目录|

## 返回参数 {#section_vbj_yxl_2gb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|2670BCFB-925D-4C3E-9994-8D12F7A9F538|请求 ID|
|Id|String|FC-AF08490649B8FD5E|新增的目录文件夹 ID|

## 示例 {#section_dcj_yxl_2gb .section}

-   请求示例

    ```
    /?Action=CreateFlowCategory
    &Name=myFolder
    &ProjectId=F-48EB68215AEAE28D
    &RegionId=cn-hangzhou
    &Type=FLOW
    &ParentId=FC-AF08490649B8FD5E
    &<公共请求参数>
    ```

-   正常返回示例

    JSON 格式

    ```
    {
    	"Id":"FC-AF08490649B8FD5E",
    	"RequestId":"2670BCFB-925D-4C3E-9994-8D12F7A9F538"
    }
    ```

-   异常返回示例

    JSON 格式

    ```
    {
    	"code":"FLOW_API_FAILED",
    	"message":"Project does not exist [FP-C05787BC0EC88258]",
    	"requestId":"11BAFBD8-8509-4177-A26D-407505E73713",
    	"successResponse":false
    }
    ```


## 错误码 {#section_fcj_yxl_2gb .section}

[查看本产品错误码](https://error-center.alibabacloud.com/status/product/Emr)

