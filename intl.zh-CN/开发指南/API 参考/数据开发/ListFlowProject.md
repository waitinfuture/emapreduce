# ListFlowProject {#concept_nhs_rvk_ggb .concept}

调用 ListFlowProject 接口查询项目列表

## 请求参数 {#section_obj_yxl_2gb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|RegionId|String|是|cn-hangzhou|区域 ID|
|Name|String|否|my\_project|名称|
|PageNumber|Integer|否|1|页码|
|PageSize|Integer|否|20|每页数量|
|ProjectId|String|否|FP-7A1018ADE9179EE1|项目 ID|

## 返回参数 {#section_vbj_yxl_2gb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|83B256D4-4E95-454B-AD08-799DF31D5556|请求 ID|
|PageNumber|Integer|1|页码|
|PageSize|Integer|20|每页数量|
|Total|Integer|1|总数|
|Projects| | |项目列表|
|Id|String|7A1018ADE9179EE1|项目 ID|
|GmtCreate|Long|1540796236000|创建时间|
|GmtModified|Long|1540796236000|修改时间|
|UserId|String|\*\*\*\*|主账号 ID|
|Name|String|my\_project|项目名称|
|Description|String|my project description|项目描述|

## 示例 {#section_dcj_yxl_2gb .section}

-   请求示例

    ```
    
    /?Action=ListFlowProject
    &RegionId=cn-hangzhou
    &Name=my_project
    &PageNumber=1
    &PageSize=20
    &ProjectId=FP-7A1018ADE9179EE1
    &<公共请求参数>
    ```

-   正常返回示例

    JSON 格式

    ```
    {
    	"PageNumber":1,
    	"PageSize":1,
    	"Projects":{
    		"Project":[{
    			"Description":"gh-告警增强11",
    			"GmtCreate":1542957424000,
    			"GmtModified":1542964674000,
    			"Id":"FP-17AB3389E1AD9A34",
    			"Name":"gh-告警增强",
    			"UserId":****
    		}]
    	},
    	"RequestId":"3177EF07-34F5-48D0-9BFD-3C33E70EB260",
    	"Total":34
    }
    ```

-   异常返回示例

    JSON 格式

    ```
    {
    	"code":"FLOW_API_FAILED",
    	"message":"Invalid Param[pageSize should less than 250]",
    	"requestId":"5E866FC1-7BBD-4965-AC6B-4689A121FC88",
    	"successResponse":false
    }
    ```


## 错误码 {#section_fcj_yxl_2gb .section}

[查看本产品错误码](https://error-center.alibabacloud.com/status/product/Emr)

