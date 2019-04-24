# DescribeFlowProjectClusterSetting {#concept_tyj_g2x_fgb .concept}

调用 DescribeFlowProjectClusterSetting 接口查询项目集群设置详情

## 请求参数 {#section_obj_yxl_2gb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|ClusterId|String|是|C-DCEE11B49C8F42B4|集群 ID|
|ProjectId|String|是|FP-ED2F3E844FE383AA|项目 ID|
|RegionId|String|是|cn-hangzhou|区域 ID|

## 返回参数 {#section_vbj_yxl_2gb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|F2168FB7-8E60-44EE-A946-DA887297D3D1|请求 ID|
|GmtCreate|Long|1541561123000|创建时间|
|GmtModified|Long|1541561123000|修改时间|
|ProjectId|String|String|项目 ID|
|ClusterId|String|C-DCEE11B49C8F42B4|集群 ID|
|DefaultUser|String|hadoop|默认提交的 Linux 用户|
|DefaultQueue|String|default|默认提交 YARN 队列|
|UserList|String|"User": \["hadoop"\]|Linux 提交用户白名单列表|
|QueueList|String|"Queue": \["default"\]|YARN 队列白名单列表|
|HostList|String|"Host":\["emr-header-1.cluster-500159692"|客户端白名单列表|

## 示例 {#section_dcj_yxl_2gb .section}

-   请求示例

    ```
    
    /?Action=DescribeFlowProjectClusterSetting
    &ClusterId=C-DCEE11B49C8F42B4
    &ProjectId=FP-ED2F3E844FE383AA
    &RegionId=cn-hangzhou
    &<公共请求参数>
    ```

-   正常返回示例

    JSON 格式

    ```
    {
    	"code":"200",
    	"data":{
    		"ClusterId":"C-F32FB31D82954C64",
    		"DefaultQueue":"undefined",
    		"DefaultUser":"hadoop",
    		"GmtCreate":1541561123000,
    		"GmtModified":1542948908000,
    		"HostList":{
    			"Host":["emr-header-1.cluster-500159692"]
    		},
    		"ProjectId":"FP-3535FE0BE5224A47",
    		"QueueList":{
    			"Queue":[]
    		},
    		"RequestId":"80F270E8-27BD-4F24-BB2A-CD3FBCC450DA",
    		"UserList":{
    			"User":[]
    		}
    	},
    	"requestId":"80F270E8-27BD-4F24-BB2A-CD3FBCC450DA",
    	"successResponse":true
    }
    ```

-   异常返回示例

    JSON 格式

    ```
    {
    	"code":"FLOW_API_FAILED",
    	"message":"Project does not exist [FP-3535FE0BE5224A4]",
    	"requestId":"93F6F3CF-B2AE-411A-948F-0F7293058CF5",
    	"successResponse":false
    }
    ```


## 错误码 {#section_fcj_yxl_2gb .section}

[查看本产品错误码](https://error-center.alibabacloud.com/status/product/Emr)

