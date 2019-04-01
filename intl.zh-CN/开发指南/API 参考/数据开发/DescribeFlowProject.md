# DescribeFlowProject {#concept_l23_vbx_fgb .concept}

查询项目详情参数及示例

## 请求参数 {#section_obj_yxl_2gb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|ProjectId|String|是|FP-5D55DA9DEDF22ED3|项目 ID|
|RegionId|String|是|cn-hangzhou|区域 ID|

## 返回参数 {#section_vbj_yxl_2gb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|ACD3A7A5-CD6E-48DA-823B-ACE5B01DA43D|请求 ID|
|Id|String|FP-5D55DA9DEDF22ED3|项目 ID|
|GmtCreate|Long|1542934807000|创建时间|
|GmtModified|Long|1542934807000|修改时间|
|UserId|String|123456789|主账号 ID|
|Name|String|project\_name\_demo|项目名称|
|Description|String|project description demo|项目描述|

## 示例 {#section_dcj_yxl_2gb .section}

-   请求示例

    ```
    
    /?Action=DescribeFlowProject
    &ProjectId=FP-5D55DA9DEDF22ED3
    &RegionId=cn-hangzhou
    &<公共请求参数>
    ```

-   正常返回示例

    JSON 格式

    ```
    {
    	"code":"200",
    	"data":{
    		"Description":"integration_test_project",
    		"GmtCreate":1542934807000,
    		"GmtModified":1542934807000,
    		"Id":"FP-5D55DA9DEDF22ED3",
    		"Name":"integration_test_project",
    		"RequestId":"ACD3A7A5-CD6E-48DA-823B-ACE5B01DA43D",
    		"UserId":"123456789"
    	},
    	"requestId":"ACD3A7A5-CD6E-48DA-823B-ACE5B01DA43D",
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

