# AddClusterService {#doc_api_Emr_AddClusterService .reference}

调用 AddClusterService 接口，为指定的集群添加当前集群的主版本支持的某项服务。

## 调试 {#api_explorer .section}

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=AddClusterService&type=RPC&version=2016-04-08)

## 请求参数 {#parameters .section}

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|AddClusterService| 系统规定参数。取值：AddClusterService。

 |
|ClusterId|String|是|C-F32FB31D8295\*\*\*\*| 待添加服务的集群ID。

 |
|RegionId|String|是|cn-hangzhou| 集群对应的地域ID。

 |
|AccessKeyId|String|否|LTAI8ljWyu7y\*\*\*\*| 阿里云AccessKey ID信息，用于标识访问者身份。

 |
|Comment|String|否|addService| 本次添加服务的备注信息。

 |
|Service.N.ServiceName|String|是|HDFS| 待添加的服务名称，例如，HDFS、YARN。

 |

## 返回数据 {#resultMapping .section}

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|EBB4D49C-4064-4818-B3AE-4C6BE5FC8264| 请求ID。

 |

## 示例 {#demo .section}

请求示例

``` {#request_demo}

http(s)://[Endpoint]/?Action=AddClusterService
&ClusterId=C-F32FB31D82954C64
&RegionId=cn-hangzhou
&Service.1.ServiceName=HDFS
&<公共请求参数>

```

正常返回示例

`XML` 格式

``` {#xml_return_success_demo}
<AddClusterServiceResponse>
	  <requestId>EBB4D49C-4064-4818-B3AE-4C6BE5FC8264</requestId>
</AddClusterServiceResponse>
```

`JSON` 格式

``` {#json_return_success_demo}
{
	"requestId":"EBB4D49C-4064-4818-B3AE-4C6BE5FC8264"
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

访问[错误中心](https://error-center.alibabacloud.com/status/product/Emr)查看更多错误码。

