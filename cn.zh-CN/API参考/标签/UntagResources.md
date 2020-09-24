# UntagResources

调用UntagResources，为指定的EMR集群列统一解绑标签。解绑后，如果该标签没有绑定其他任何资源，会被自动删除。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=UntagResources&type=RPC&version=2016-04-08)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|否|UntagResources|系统规定参数。取值：UntagResources。 |
|RegionId|String|是|cn-hangzhou|资源所属的地域ID。 |
|ResourceId.N|RepeatList|是|C-3652B95F65967\*\*\*|EMR集群ID，N的取值范围为1~50。 |
|ResourceType|String|是|cluster|资源类型。EMR集群为cluster。 |
|TagKey.N|RepeatList|否|DevNianmin|EMR集群的标签键。N的取值范围：1~20。 |
|All|Boolean|否|false|是否解绑资源上全部的标签。当请求中未设置TagKey.N时，该参数才有效。取值范围：

 -   **true**
-   **false**

 默认值：**false**。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|C46FF5A8-C5F0-4024-8262-B16B639225A0|请求ID。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=UntagResources
&RegionId=cn-hangzhou
&ResourceId.1=C-3652B95F65967***
&ResourceType=cluster
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<RequestId>C46FF5A8-C5F0-4024-8262-B16B639225A0</RequestId>
```

`JSON` 格式

```
{"RequestId":"C46FF5A8-C5F0-4024-8262-B16B639225A0"}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|500|InternalError|The request processing has failed due to some unknown error.|内部错误，请提工单|

访问[错误中心](https://error-center.aliyun.com/status/product/Emr)查看更多错误码。

访问[错误中心](https://error-center.alibabacloud.com/status/product/Emr)查看更多错误码。

