# ListFlowCategory {#doc_api_Emr_ListFlowCategory .reference}

调用 ListFlowCategory 接口展示工作流目录。

## 调试 {#api_explorer .section}

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=ListFlowCategory&type=RPC&version=2016-04-08)

## 请求参数 {#parameters .section}

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ListFlowCategory|系统规定参数。取值：ListFlowCategory。

 |
|ProjectId|String|是|FP-6E8E198E6C79\*\*\*\*|项目ID。

 |
|RegionId|String|是|cn-hangzhou|地域ID。

 |
|PageNumber|Integer|否|1|分页查询的页数。

 |
|PageSize|Integer|否|10|分页查询的每页数量。

 |
|ParentId|String|否|FC-6E8E198E6C79179C|上级目录ID。

 |
|Root|Boolean|否|false|是否为根目录。

 |

## 返回数据 {#resultMapping .section}

|名称|类型|示例值|描述|
|--|--|---|--|
|Categories| | |目录。

 |
|CategoryType|String|FILE|目录节点类型：

 -   FILE：文件
-   FOLDER：目录

 |
|GmtCreate|Long|1563848015000|创建时间。

 |
|GmtModified|Long|1563848015000|修改时间。

 |
|Id|String|FC-DF781BBF3F2E8317|目录ID

 |
|Name|String|测试目录|目录名称。

 |
|ObjectId|String|F-0B30759C146EA1E7|关联实体ID，例如，作业和工作流等。

 |
|ObjectType|String|FLOW|关联实体类型：

 -   FLOW ：工作流。
-   JOB：作业。

 |
|ParentId|String|FC-DF781BBF3F2E8318|上级目录ID。

 |
|ProjectId|String|FP-179332E88F52\*\*\*\*|关联项目ID。

 |
|Type|String|FLOW|目录类型:

 -   FLOW：工作流类型
-   JOB：作业类型

 |
|PageNumber|Integer|1|分页查询的页数。

 |
|PageSize|Integer|10|分页查询的每页数量。

 |
|RequestId|String|5ECD6EA1-838E-4BDF-96C8-AEAA40F04F48|请求ID。

 |
|Total|Integer|100|目录总数。

 |

## 示例 {#demo .section}

请求示例

``` {#request_demo}

http(s)://[Endpoint]/?Action=ListFlowCategory
&ProjectId=FP-6E8E198E6C79****
&RegionId=cn-hangzhou
&<公共请求参数>

```

正常返回示例

`XML` 格式

``` {#xml_return_success_demo}
<ListFlowCategoryResponse>
  <PageSize>10</PageSize>
	  <RequestId>BCF52B64-007F-4883-BAEA-0499106D07C2</RequestId>
	  <PageNumber>1</PageNumber>
	  <Total>1</Total>
	  <Categories>
		    <categoryType>FOLDER</categoryType>
		    <gmtModified>1563848002000</gmtModified>
		    <name>FLOW</name>
		    <id>FC-D566F13A20A00D18</id>
		    <gmtCreate>1563848002000</gmtCreate>
		    <type>FLOW</type>
		    <projectId>FP-179332E88F52****</projectId>
		    <parentId>root_parent</parentId>
	  </Categories>
</ListFlowCategoryResponse>
```

`JSON` 格式

``` {#json_return_success_demo}
{
	"PageNumber":1,
	"PageSize":10,
	"RequestId":"BCF52B64-007F-4883-BAEA-0499106D07C2",
	"Categories":[
		{
			"id":"FC-D566F13A20A00D18",
			"gmtModified":1563848002000,
			"parentId":"root_parent",
			"gmtCreate":1563848002000,
			"name":"FLOW",
			"categoryType":"FOLDER",
			"projectId":"FP-179332E88F52****",
			"type":"FLOW"
		}
	],
	"Total":1
}
```

## 错误码 { .section}

访问[错误中心](https://error-center.alibabacloud.com/status/product/Emr)查看更多错误码。

