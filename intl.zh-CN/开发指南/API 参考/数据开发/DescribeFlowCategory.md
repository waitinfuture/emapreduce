# DescribeFlowCategory {#concept_lbl_z2w_fgb .concept}

查询目录详细信息参数及示例

## 请求参数 {#section_obj_yxl_2gb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Id|String|是|FC-075AB9477DAE9AAC|目录 ID|
|ProjectId|String|是|FP-ABD24A6163D31274|项目 ID|
|RegionId|String|是|cn-hangzhou|区域 ID|

## 返回参数 {#section_vbj_yxl_2gb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|BCEDC663-43F1-4202-9C49-9D3D66EA93D7|请求 ID|
|Id|String|FC-075AB9477DAE9AAC|目录 ID|
|GmtCreate|Long|1542783867503|创建时间|
|GmtModified|Long|1542783867503|修改时间|
|Name|String|my\_category|目录名称|
|ParentId|String|FC-3A749C1F5D12BAAC|父目录 ID|
|Type|String|FLOW|类型: FLOW，JOB，ADHOC|
|CategoryType|String|FILE|目录属性：FOLDER\(文件夹\)，FILE\(文件\)|
|ObjectType|String|FLOW|目录对象的具体类型: FLOW，MR，SPARK，HIVE\_SQL，HIVE，PIG，SQOOP，SPARK\_SQL，SPARK\_STREAMING，SHELL|
|ObjectId|String|String|关联的目标对象 ID， 例如工作流 ID 或者作业 ID|
|ProjectId|String|FP-E3F1523F8FC1D2E6|项目 ID|

## 示例 {#section_dcj_yxl_2gb .section}

-   请求示例

    ```
    
    /?Action=DescribeFlowCategory
    &Id=FC-075AB9477DAE9AAC
    &ProjectId=FP-ABD24A6163D31274
    &RegionId=区域ID
    &<公共请求参数>
    ```

-   正常返回示例

    JSON 格式

    ```
    {
    	"code":"200",
    	"data":{
    		"CategoryType":"FILE",
    		"GmtCreate":1541858202000,
    		"GmtModified":1541858767000,
    		"Id":"FC-3A749C1F5D12BAAC",
    		"Name":"myFlow",
    		"ObjectId":"F-33FA7CD5E1957945",
    		"ObjectType":"FLOW",
    		"ParentId":"FC-198D4E3ACA4F75B3",
    		"ProjectId":"FP-E3F1523F8FC1D2E6",
    		"RequestId":"BBEB8FD9-3AA8-4D53-831C-037D9C53348E",
    		"Type":"FLOW"
    	},
    	"requestId":"BBEB8FD9-3AA8-4D53-831C-037D9C53348E",
    	"successResponse":true
    }
    ```

-   异常返回示例

    JSON 格式

    ```
    {
    	"code":"FLOW_API_FAILED",
    	"message":"Project does not exist [FP-E3F1523F8FC1D2E]",
    	"requestId":"5C5E4A6F-5140-4627-AB81-F3E0D06C5C36",
    	"successResponse":false
    }
    ```


## 错误码 {#section_fcj_yxl_2gb .section}

[查看本产品错误码](https://error-center.alibabacloud.com/status/product/Emr)

