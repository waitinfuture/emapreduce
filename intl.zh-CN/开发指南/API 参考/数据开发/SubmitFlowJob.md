# SubmitFlowJob {#concept_tgm_rnl_ggb .concept}

提交运行作业，每次只允许存在一个正在运行的实例

## 请求参数 {#section_obj_yxl_2gb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|ClusterId|String|是|C-F32FB31D82954C64|集群 ID|
|JobId|String|是|FJ-1A2FB31D82954C64|作业 ID|
|ProjectId|String|是|FP-3535FE0BE5224A47|项目 ID|
|RegionId|String|是|cn-hangzhou|区域 ID|
|Conf|String|否|\{"key":"value"\}|配置参数信息：\{&quot;key1&quot;:&quot;value1&quot;\},key为 params 的参数值会覆盖实际作业中运行的内容|
|HostName|String|否|emr-header-1.cluster-12345|工作流运行所在的机器 hostName, 支持 master 和 gateway 机器. hostname 格式为 emr-header-1.cluster-12345，可登陆机器用 hostname 命令查看对应的值|

## 返回参数 {#section_vbj_yxl_2gb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|B46F8A2A-B46B-415C-8A9C-B01B99B775A2|请求 ID|
|Id|String|FJI-9DDAAA3ADA5F6FA2|运行的作业实例 ID|

## 示例 {#section_dcj_yxl_2gb .section}

-   请求示例

    ```
    
    /?Action=SubmitFlowJob
    &ClusterId=C-F32FB31D82954C64
    &JobId=FJ-1A2FB31D82954C64
    &ProjectId=FP-3535FE0BE5224A47
    &RegionId=cn-hangzhou
    &Conf={"key":"value"}
    &HostName=emr-header-1.cluster-12345
    &<公共请求参数>
    ```

-   正常返回示例

    JSON 格式

    ```
    {
    	"Id":"FJI-9DDAAA3ADA5F6FA2",
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

