# DescribeFlowCategory {#doc_api_Emr_DescribeFlowCategory .reference}

调用 DescribeFlowCategory 接口查询目录详细信息

## 调试 {#api_explorer .section}

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=DescribeFlowCategory&type=RPC&version=2016-04-08)

## 请求参数 {#parameters .section}

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DescribeFlowCategory|系统规定参数。取值：DescribeFlowCategory。

 |
|Id|String|是|FC-075AB9477DAE9AAC|目录ID。

 |
|ProjectId|String|是|FP-ABD24A6163D31274|项目ID。

 |
|RegionId|String|是|区域ID|地域ID。

 |

## 返回数据 {#resultMapping .section}

|名称|类型|示例值|描述|
|--|--|---|--|
|CategoryType|String|FILE|目录属性：

 -   FOLDER（文件夹）。
-   FILE（文件）。

 |
|GmtCreate|Long|1542783867503|创建时间。

 |
|GmtModified|Long|1542783867503|修改时间。

 |
|Id|String|FC-075AB9477DAE9AAC|目录ID。

 |
|Name|String|my\_category|目录名称。

 |
|ObjectId|String|F-EB3021F73E512A7D|关联的目标对象ID， 例如，工作流ID或者作业ID。

 |
|ObjectType|String|FLOW|目录对象的具体类型： FLOW、MR、SPARK、HIVE\_SQL、HIVE、PIG、SQOOP、SPARK\_SQL、SPARK\_STREAMING、SHELL。

 |
|ParentId|String|FC-3A749C1F5D12BAAC|父目录ID。

 |
|ProjectId|String|FP-E3F1523F8FC1D2E6|项目ID。

 |
|RequestId|String|BCEDC663-43F1-4202-9C49-9D3D66EA93D7|请求ID。

 |
|Type|String|FLOW|类型：

 -   FLOW。
-   JOB。
-   ADHOC。

 |

## 示例 {#demo .section}

请求示例

``` {#request_demo}

http(s)://[Endpoint]/?Action=DescribeFlowCategory
&Id=FC-075AB9477DAE9AAC
&ProjectId=FP-ABD24A6163D31274
&RegionId=区域ID
&<公共请求参数>

```

正常返回示例

`XML` 格式

``` {#xml_return_success_demo}
<DescribeFlowCategoryResponse>
  <code>200</code>
	  <data>
		    <ParentId>FC-198D4E3ACA4F75B3</ParentId>
		    <ObjectType>FLOW</ObjectType>
		    <Type>FLOW</Type>
		    <GmtCreate>1541858202000</GmtCreate>
		    <RequestId>BBEB8FD9-3AA8-4D53-831C-037D9C53348E</RequestId>
		    <ObjectId>F-33FA7CD5E1957945</ObjectId>
		    <ProjectId>FP-E3F1523F8FC1D2E6</ProjectId>
		    <GmtModified>1541858767000</GmtModified>
		    <CategoryType>FILE</CategoryType>
		    <Id>FC-3A749C1F5D12****</Id>
		    <Name>myFlow</Name>
	  </data>
	  <requestId>BBEB8FD9-3AA8-4D53-831C-037D9C53348E</requestId>
	  <successResponse>true</successResponse>
	

</DescribeFlowCategoryResponse>
```

`JSON` 格式

``` {#json_return_success_demo}
{
	"successResponse":true,
	"requestId":"BBEB8FD9-3AA8-4D53-831C-037D9C53348E",
	"data":{
		"Name":"myFlow",
		"CategoryType":"FILE",
		"ParentId":"FC-198D4E3ACA4F75B3",
		"ObjectType":"FLOW",
		"Type":"FLOW",
		"RequestId":"BBEB8FD9-3AA8-4D53-831C-037D9C53348E",
		"Id":"FC-3A749C1F5D12****",
		"GmtCreate":1541858202000,
		"GmtModified":1541858767000,
		"ProjectId":"FP-E3F1523F8FC1D2E6",
		"ObjectId":"F-33FA7CD5E1957945"
	},
	"code":"200"
}
```

## 错误码 { .section}

访问[错误中心](https://error-center.aliyun.com/status/product/Emr)查看更多错误码。

