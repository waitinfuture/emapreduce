# DescribeFlowProject {#doc_api_Emr_DescribeFlowProject .reference}

调用 DescribeFlowProject 接口，查询项目详情

## 调试 {#api_explorer .section}

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=DescribeFlowProject&type=RPC&version=2016-04-08)

## 请求参数 {#parameters .section}

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DescribeFlowProject|系统规定参数。取值：DescribeFlowProject。

 |
|ProjectId|String|是|FP-5D55DA9DEDF2\*\*\*\*|项目ID。

 |
|RegionId|String|是|cn-hangzhou|地域ID。

 |

## 返回数据 {#resultMapping .section}

|名称|类型|示例值|描述|
|--|--|---|--|
|Description|String|project description demo|项目描述。

 |
|GmtCreate|Long|1542934807000|创建时间。

 |
|GmtModified|Long|1542934807000|修改时间。

 |
|Id|String|FP-5D55DA9DEDF2\*\*\*\*|项目ID。

 |
|Name|String|project\_name\_demo|项目名称。

 |
|RequestId|String|ACD3A7A5-CD6E-48DA-823B-ACE5B01DA43D|请求ID。

 |
|UserId|String|123456789|主账号ID。

 |

## 示例 {#demo .section}

请求示例

``` {#request_demo}

http(s)://[Endpoint]/?Action=DescribeFlowProject
&ProjectId=FP-5D55DA9DEDF2****
&RegionId=cn-hangzhou
&<公共请求参数>

```

正常返回示例

`XML` 格式

``` {#xml_return_success_demo}
<DescribeFlowProjectResponse>
	  <data>
		    <GmtCreate>1542934807000</GmtCreate>
		    <Description>integration_test_project</Description>
		    <RequestId>ACD3A7A5-CD6E-48DA-823B-ACE5B01DA43D</RequestId>
		    <UserId>123456789</UserId>
		    <GmtModified>1542934807000</GmtModified>
		    <Id>FP-5D55DA9DEDF2****</Id>
		    <Name>integration_test_project</Name>
	  </data>
	  <requestId>ACD3A7A5-CD6E-48DA-823B-ACE5B01DA43D</requestId>
</DescribeFlowProjectResponse>
```

`JSON` 格式

``` {#json_return_success_demo}
{
	"requestId":"ACD3A7A5-CD6E-48DA-823B-ACE5B01DA43D",
	"data":{
		"Name":"integration_test_project",
		"Description":"integration_test_project",
		"RequestId":"ACD3A7A5-CD6E-48DA-823B-ACE5B01DA43D",
		"Id":"FP-5D55DA9DEDF2****",
		"UserId":"123456789",
		"GmtCreate":1542934807000,
		"GmtModified":1542934807000
	}
}
```

## 错误码 { .section}

访问[错误中心](https://error-center.alibabacloud.com/status/product/Emr)查看更多错误码。

