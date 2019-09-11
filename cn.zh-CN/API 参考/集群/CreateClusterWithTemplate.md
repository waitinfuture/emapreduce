# CreateClusterWithTemplate {#doc_api_Emr_CreateClusterWithTemplate .reference}

调用 CreateClusterWithTemplate 接口，通过集群模版创建集群。

## 调试 {#api_explorer .section}

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=CreateClusterWithTemplate&type=RPC&version=2016-04-08)

## 请求参数 {#parameters .section}

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|CreateClusterWithTemplate|系统规定参数。取值：CreateClusterWithTemplate。

 |
|TemplateBizId|String|是|CT-35498C56B3F1\*\*\*\*|模版ID。

 |
|AccessKeyId|String|否|LTAI8ljWyu7y\*\*\*\*|阿里云AccessKey ID信息，用于标识访问者身份。

 |
|ClusterName|String|否|hadoop\_cluster\_name\_1|自定义集群名称。

 |
|UniqueTag|String|否|60a632f0-5909-430d-b65c-1b0f248e4947|用户自定义的业务标识，此字段的值对集群模版创建无影响。

 |

## 返回数据 {#resultMapping .section}

|名称|类型|示例值|描述|
|--|--|---|--|
|ClusterId|String|C-D7958B72E59B\*\*\*\*|集群ID。

 |
|CoreOrderId|String|0|Core节点订单ID。

 |
|EmrOrderId|String|0|EMR订单ID。

 |
|MasterOrderId|String|0|Master节点订单ID。

 |
|RequestId|String|BF4FBAC6-B03E-4BFB-B6DB-EB53C34F2E22|请求ID。

 |

## 示例 {#demo .section}

请求示例

``` {#request_demo}

http(s)://[Endpoint]/?Action=CreateClusterWithTemplate
&TemplateBizId=CT-35498C56B3F1****
&<公共请求参数>

```

正常返回示例

`XML` 格式

``` {#xml_return_success_demo}
<CreateClusterWithTemplateResponse>
	  <ClusterId>C-956FA9059886****</ClusterId>
	  <RequestId>BF4FBAC6-B03E-4BFB-B6DB-EB53C34F2E22</RequestId>
</CreateClusterWithTemplateResponse>
```

`JSON` 格式

``` {#json_return_success_demo}
{
	"ClusterId":"C-956FA9059886****",
	"RequestId":"BF4FBAC6-B03E-4BFB-B6DB-EB53C34F2E22"
}
```

## 错误码 { .section}

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|403|Forbbiden|User not authorized to operate on the specified resource.|没有权限操作指定资源，联系主账号授权|
|500|InternalError|The request processing has failed due to some unknown error.|内部错误，请提工单|

访问[错误中心](https://error-center.aliyun.com/status/product/Emr)查看更多错误码。

