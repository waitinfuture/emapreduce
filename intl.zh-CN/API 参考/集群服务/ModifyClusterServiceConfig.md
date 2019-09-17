# ModifyClusterServiceConfig {#doc_api_Emr_ModifyClusterServiceConfig .reference}

调用 ModifyClusterServiceConfig 接口，修改集群指定服务的配置信息。

## 调试 {#api_explorer .section}

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=ModifyClusterServiceConfig&type=RPC&version=2016-04-08)

## 请求参数 {#parameters .section}

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ModifyClusterServiceConfig|系统规定参数。取值：ModifyClusterServiceConfig。

 |
|ClusterId|String|是|C-xxx|集群ID。

 |
|ConfigParams|String|是|\{"tez-site":\{"tez.am.resource.memory.mb":"640"\}\}|本次修改的具体内容，JSON格式的字符串。

 |
|RegionId|String|是|cn-hangzhou|地域ID。

 |
|ServiceName|String|是|TEZ|服务名称。

 |
|Comment|String|否|modify tez config|本次修改的备注信息。

 |
|ConfigType|String|否|""|配置项类型。

 |
|CustomConfigParams|String|否|\{"tez-site":\{"key1":\{"Value":"value1"\}\}\}|自定义配置项的修改内容。

 |
|GatewayClusterIdList.N|RepeatList|否|C-XXX|同步修改的关联gateway集群ID

 |
|GroupId|String|否|G-xxx|主机组ID。

 |
|HostInstanceId|String|否|i-xxx|主机实例ID，一般是**ecsID**。

 |
|RefreshHostConfig|Boolean|否|false|修改完成后是否立刻执行configure

 |
|AccessKeyId|String|否|LTAI8ljWyu7y\*\*\*\*|阿里云AccessKey ID信息，用于标识访问者身份。

 |

## 返回数据 {#resultMapping .section}

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|D9A09DDE-6BE7-473B-9E4B-B3CB762FD4B3|请求ID。

 |

## 示例 {#demo .section}

请求示例

``` {#request_demo}

http(s)://[Endpoint]/?Action=ModifyClusterServiceConfig
&ClusterId=C-xxx
&RegionId=cn-hangzhou
&ServiceName=TEZ
&<公共请求参数>

```

正常返回示例

`XML` 格式

``` {#xml_return_success_demo}
<ModifyClusterServiceConfigResponse>
	  <RequestId>D9A09DDE-6BE7-473B-9E4B-B3CB762FD4B3</RequestId>
</ModifyClusterServiceConfigResponse>
```

`JSON` 格式

``` {#json_return_success_demo}
{
	"RequestId":"D9A09DDE-6BE7-473B-9E4B-B3CB762FD4B3"
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
|404|ClusterId.NotFound|The ClusterId provided does not exist in our records.|Cluster Id不存在，确认集群的Cluster Id|
|500|InternalError|The request processing has failed due to some unknown error.|内部错误，请提工单|

访问[错误中心](https://error-center.alibabacloud.com/status/product/Emr)查看更多错误码。

