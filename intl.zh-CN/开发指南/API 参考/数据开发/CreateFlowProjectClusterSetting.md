# CreateFlowProjectClusterSetting {#concept_qfp_dlp_fgb .concept}

创建项目集群配置参数及示例

## 请求参数 {#section_obj_yxl_2gb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|ClusterId|String|是|C-DCEE11B49C8F42B4|集群 ID|
|ProjectId|String|是|FP-ED2F3E844FE383AA|项目 ID|
|RegionId|String|是|cn-hangzhou|区域 ID|
|DefaultQueue|String|否|default|默认提交队列，默认值为 default|
|DefaultUser|String|否|hadoop|默认Linux提交用户, 默认值为hadoop|
|HostList.N|RepeatList|否|emr-header-1.cluster-12345|提交机器白名单列表, 只支持 gateway 和 master 机器|
|QueueList.N|RepeatList|否|queue1|队列白名单|
|UserList.N|RepeatList|否|hadoop|Linux 提交用户白名单|

## 返回参数 {#section_vbj_yxl_2gb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|0AD9FF20-F585-4E0A-870D-B8A8F884CCE6|请求 ID|
|Data|Boolean|true|是否成功|

## 示例 {#section_dcj_yxl_2gb .section}

-   请求示例

    ```
    /?Action=CreateFlowProjectClusterSetting
    &ClusterId=C-DCEE11B49C8F42B4
    &ProjectId=FP-ED2F3E844FE383AA
    &RegionId=cn-hangzhou
    &DefaultQueue=default
    &DefaultUser=hadoop
    &HostList.1=emr-header-1.cluster-12345
    &QueueList.1=queue1
    &UserList.1=hadoop
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
    	"message":"Cluster host does not exit [[emr-header-1.cluster-12345]",
    	"requestId":"2FEF3292-084E-45D8-8D8A-C9B8867B3F37",
    	"successResponse":false
    }
    ```


## 错误码 {#section_fcj_yxl_2gb .section}

[查看本产品错误码](https://error-center.alibabacloud.com/status/product/Emr)

