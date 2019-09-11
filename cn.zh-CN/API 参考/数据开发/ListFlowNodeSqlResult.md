# ListFlowNodeSqlResult {#doc_api_Emr_ListFlowNodeSqlResult .reference}

调用 ListFlowNodeSqlResult 接口，查询节点实例 sql 结果

## 调试 {#api_explorer .section}

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=ListFlowNodeSqlResult&type=RPC&version=2016-04-08)

## 请求参数 {#parameters .section}

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ListFlowNodeSqlResult|系统规定参数。取值：ListFlowNodeSqlResult。

 |
|NodeInstanceId|String|是|FE4BD156E939CF1F|节点实例ID。

 |
|ProjectId|String|是|FP-7A1018ADE9179EE1|项目ID。

 |
|RegionId|String|是|cn-hangzhou|地域ID。

 |
|Length|Integer|否|100|长度。

 |
|Offset|Integer|否|0|偏移。

 |
|SqlIndex|Integer|否|0|第几个SQL结果。

 |

## 返回数据 {#resultMapping .section}

|名称|类型|示例值|描述|
|--|--|---|--|
|End|Boolean|true|是否结束。

 |
|HeaderList| |\{ "Header": \[ "rankings.pageurl", "rankings.pagerank", "rankings.avgduration" \] \}|列数据。

 |
|Header| | |列数据。

 |
|RequestId|String|90F14544-832D-4D81-8F8C-8F8547AE5B9D|请求ID。

 |
|RowList| | |行数据。

 |
|Row| | |行数据。

 |
|RowIndex|Integer|0|行号。

 |
|RowItemList| |\{ "rowItem": \[ "1", "1", "1" \] \}|行内容。

 |
|rowItem| | |行内容。

 |

## 示例 {#demo .section}

请求示例

``` {#request_demo}

http(s)://[Endpoint]/?Action=ListFlowNodeSqlResult
&NodeInstanceId=FE4BD156E939CF1F
&ProjectId=FP-7A1018ADE9179EE1
&RegionId=cn-hangzhou
&<公共请求参数>

```

正常返回示例

`XML` 格式

``` {#xml_return_success_demo}
<ListFlowNodeSqlResultResponse>
  <RowList>
		    <Row>
			      <RowItemList>
				        <rowItem>1</rowItem>
				        <rowItem>1</rowItem>
				        <rowItem>1</rowItem>
			      </RowItemList>
			      <RowIndex>0</RowIndex>
		    </Row>
		    <Row>
			      <RowItemList>
				        <rowItem>1</rowItem>
				        <rowItem>1</rowItem>
				        <rowItem>1</rowItem>
			      </RowItemList>
			      <RowIndex>1</RowIndex>
		    </Row>
	  </RowList>
	  <RequestId>90F14544-832D-4D81-8F8C-8F8547AE5B9D</RequestId>
	  <HeaderList>
		    <Header>rankings.pageurl</Header>
		    <Header>rankings.pagerank</Header>
		    <Header>rankings.avgduration</Header>
	  </HeaderList>
	  <End>true</End>
	

</ListFlowNodeSqlResultResponse>
```

`JSON` 格式

``` {#json_return_success_demo}
{
	"End":true,
	"HeaderList":{
		"Header":[
			"rankings.pageurl",
			"rankings.pagerank",
			"rankings.avgduration"
		]
	},
	"RequestId":"90F14544-832D-4D81-8F8C-8F8547AE5B9D",
	"RowList":{
		"Row":[
			{
				"RowItemList":{
					"rowItem":[
						"1",
						"1",
						"1"
					]
				},
				"RowIndex":0
			},
			{
				"RowItemList":{
					"rowItem":[
						"1",
						"1",
						"1"
					]
				},
				"RowIndex":1
			}
		]
	}
}
```

## 错误码 { .section}

访问[错误中心](https://error-center.aliyun.com/status/product/Emr)查看更多错误码。

