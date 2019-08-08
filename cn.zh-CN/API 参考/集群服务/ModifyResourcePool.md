# ModifyResourcePool {#doc_api_Emr_ModifyResourcePool .reference}

调用 ModifyResourcePool 接口更新资源池。

更新资源池

## 调试 {#api_explorer .section}

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=ModifyResourcePool&type=RPC&version=2016-04-08)

## 请求参数 {#parameters .section}

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ModifyResourcePool|系统规定参数。取值：ModifyResourcePool。

 |
|ClusterId|String|是|C-0E995C0EE7E5ECB3|集群 ID

 |
|Id|String|是|116|资源池 ID

 |
|RegionId|String|是|cn-hangzhou|区域 ID

 |
|AccessKeyId|String|否|LTAI8ljWyu7y\*\*\*\*|AccessKeyId

 |
|Active|Boolean|否|true|是否激活

 |
|Config.N.Category|String|否|DEFAULT\_SETTINGS|配置类别,合法值DEFAULT\_SETTINGS,ACCESS\_CONTROL\_SETTINGS,QUEUE\_RESOURCE\_LIMIT,QUEUE\_SCHEDULING\_POLICY,QUEUE\_PREEMPTION,QUEUE\_SUBMISSION\_ACCESS\_CONTROL,QUEUE\_ADMINISTRATION\_ACCESS\_CONTROL

 |
|Config.N.ConfigKey|String|否|capacity|配置参数 KEY

 |
|Config.N.ConfigValue|String|否|100|配置参数值

 |
|Config.N.Id|String|否|10|配置参数 ID

 |
|Config.N.Note|String|否|容量权重|配置参数描述

 |
|Name|String|否|custompool|资源池名称

 |
|Yarnsiteconfig|String|否|Yarnsiteconfig|YARN 配置列表

 |

## 返回数据 {#resultMapping .section}

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|A544317F-4A60-4532-AC96-191B9D80420A|请求ID

 |

## 示例 {#demo .section}

请求示例

``` {#request_demo}

http(s)://[Endpoint]/?Action=ModifyResourcePool
&ClusterId=C-0E995C0EE7E5ECB3
&Id=116
&RegionId=cn-hangzhou
&<公共请求参数>

```

正常返回示例

`XML` 格式

``` {#xml_return_success_demo}
<ModifyResourcePool>
	  <code>200</code>
	  <requestId>A544317F-4A60-4532-AC96-191B9D80420A</requestId>
	  <successResponse>true</successResponse>
</ModifyResourcePool>
```

`JSON` 格式

``` {#json_return_success_demo}
{
	"successResponse":true,
	"requestId":"A544317F-4A60-4532-AC96-191B9D80420A",
	"code":"200"
}
```

## 错误码 { .section}

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|403|Params.Illegal|The specified parameters are wrongly formed..|指定参数格式错误|
|403|User.OtherUserResource.NotAllow|It is not allowed to operate other user's resource|不能操作其它用户的资源|
|403|User.Account.Abnormal|The User Account maybe is out of service!|用户帐号已经停止服务|
|403|JobId.Not.Exist|Job \[%s\] does not exist or is deleted!|指定作业ID不存在请填写正确的参数|
|403|Job.RegionId.Not.Match|Specified job does not exist in this region\[%s\]!|指定作业不在此Region中，请填写正确的参数|
|500|InternalError|The request processing has failed due to some unknown error.|内部错误，请提工单|

访问[错误中心](https://error-center.aliyun.com/status/product/Emr)查看更多错误码。

