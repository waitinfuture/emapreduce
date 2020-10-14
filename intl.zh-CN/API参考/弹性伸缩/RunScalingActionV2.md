# RunScalingActionV2

调用RunScalingActionV2接口，执行伸缩组示例控制动作。例如，启动、停止、重启和刷新伸缩组实例。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=RunScalingActionV2&type=RPC&version=2016-04-08)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|RunScalingActionV2|系统规定参数。对于您自行拼凑HTTP或HTTPS URL发起的API请求，该参数为必选参数。取值：RunScalingActionV2。 |
|RegionId|String|是|cn-hangzhou|地域ID。您可以调用[DescribeRegions](~~25609~~)接口查看最新的阿里云地域列表。 |
|ScalingActionType|String|是|START\_SCALING\_GROUP|弹性伸缩引擎动作。取值如下：

 -   START\_SCALING\_GROUP：启动伸缩组
-   STOP\_SCALING\_GROUP：停止伸缩组
-   RESTART\_SCALING\_GROUP：重启伸缩组
-   UPDATE\_SCALING\_GROUP\_INSTANCE：刷新伸缩组配置，应用最新配置 |
|ScalingGroupBizId|String|是|SGB-234242ABC\*\*\*\*|伸缩组ID。你可以调用[ListScalingGroupV2](~~184367~~)查看伸缩组ID。 |
|ResourceGroupId|String|否|SGB-CA3A16B87BF1\*\*\*\*|资源组ID。你可以调用[ListResourceGroups](~~158855~~)查看资源组ID。 |
|ActionParam|String|否|\{\}|控制动作参数。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Data|String|true|返回结果。 取值如下：

 -   true：成功
-   false：失败 |
|RequestId|String|123DF0CD-C5F5-4A8D-984A-C5A4331EB84F|请求ID。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=RunScalingActionV2
&RegionId=cn-hangzhou
&ScalingActionType=START_SCALING_GROUP
&ScalingGroupBizId=SGB-234242ABC****
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<RequestId>123DF0CD-C5F5-4A8D-984A-C5A4331EB84F</RequestId>
<Data>true</Data>
```

`JSON` 格式

```
{
    "RequestId":"123DF0CD-C5F5-4A8D-984A-C5A4331EB84F",
    "Data":"true"
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|403|Params.Illegal|The specified parameters are wrongly formed..|指定参数格式错误|
|500|InternalError|The request processing has failed due to some unknown error.|内部错误，请提工单|

访问[错误中心](https://error-center.alibabacloud.com/status/product/Emr)查看更多错误码。

