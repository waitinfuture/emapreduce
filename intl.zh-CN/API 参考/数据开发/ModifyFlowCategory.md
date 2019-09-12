# ModifyFlowCategory {#doc_api_Emr_ModifyFlowCategory .reference}

调用ModifyFlowCategory接口，重命名目录。

## 调试 {#api_explorer .section}

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=ModifyFlowCategory&type=RPC&version=2016-04-08)

## 请求参数 {#parameters .section}

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ModifyFlowCategory|系统规定参数。取值：ModifyFlowCategory。

 |
|Id|String|是|FC-ABCDEFGHI\*\*\*\*|目录ID。

 |
|ProjectId|String|是|FP-ABCDEFGHI\*\*\*\*|项目ID。

 |
|RegionId|String|是|cn-hangzhou|地域ID。

 |
|Name|String|否|Test|目录名称。

 |
|ParentId|String|否|FC-ABCDEFGHI\*\*\*\*|上级目录ID。

 |

## 返回数据 {#resultMapping .section}

|名称|类型|示例值|描述|
|--|--|---|--|
|Data|Boolean|true|修改结果，**true**表示修改成功，**false**表示修改失败。

 |
|RequestId|String|CEA9AFD2-B340-41F4-A661-8916CBF07C32|请求ID。

 |

## 示例 {#demo .section}

请求示例

``` {#request_demo}

http(s)://[Endpoint]/?Action=ModifyFlowCategory
&Id=FC-ABCDEFGHI****
&ProjectId=FP-ABCDEFGHI****
&RegionId=cn-hangzhou
&<公共请求参数>

```

正常返回示例

`XML` 格式

``` {#xml_return_success_demo}
<ModifyFlowCategoryResponse>
	  <data>
		    <RequestId>CEA9AFD2-B340-41F4-A661-8916CBF07C32</RequestId>
		    <Data>true</Data>
	  </data>
	  <requestId>CEA9AFD2-B340-41F4-A661-8916CBF07C32</requestId>
</ModifyFlowCategoryResponse>
```

`JSON` 格式

``` {#json_return_success_demo}
{
	"requestId":"CEA9AFD2-B340-41F4-A661-8916CBF07C32",
	"data":{
		"Data":true,
		"RequestId":"CEA9AFD2-B340-41F4-A661-8916CBF07C32"
	}
}
```

## 错误码 { .section}

访问[错误中心](https://error-center.alibabacloud.com/status/product/Emr)查看更多错误码。

