# ListClusterServiceQuickLink

调用ListClusterServiceQuickLink接口，查询集群指定服务或所有服务的快捷链接。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=ListClusterServiceQuickLink&type=RPC&version=2016-04-08)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ListClusterServiceQuickLink|系统规定参数。对于您自行拼凑HTTP或HTTPS URL发起的API请求，该参数为必选参数。取值：ListClusterServiceQuickLink。 |
|ClusterId|String|是|C-A15B381E446C\*\*\*\*|集群ID。您可以调用[ListClusters](~~28147~~)接口查看集群的ID。 |
|RegionId|String|是|cn-hangzhou|地域ID。您可以调用[DescribeRegions](~~25609~~)接口查看最新的阿里云地域列表。 |
|DirectType|Boolean|否|true|保留字段。 |
|ServiceName|String|否|SPARK|服务名称：

 -   SPARK
-   RANGER
-   GANGLIA
-   HDFS
-   YARN |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|QuickLinkList|Array of QuickLink| |快捷链接列表。 |
|QuickLink| | | |
|Port|String|8443|服务端口。 |
|Protocol|String|https|链接协议。 |
|QuickLinkAddress|String|knox.C-A15B381E446C\*\*\*\*.cn-hangzhou.emr.aliyuncs.com:8443/gateway/cluster-topo/sparkhistory/|快捷链接地址。 |
|ServiceDisplayName|String|Spark|服务的展示名称。 |
|ServiceName|String|SPARK|服务名称。 |
|Type|String|KNOX|服务类型：

 -   DEFAULT：包括HUE和ZEPPELIN服务。

 **说明：** 集群已添加HUE和ZEPPELIN服务。

 -   KNOX：包括YARN、HDFS、SPARK、GANGLIA和RANGER服务。 |
|RequestId|String|621094BA-8C21-4199-9F04-CA9AA5B63955|请求ID。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=ListClusterServiceQuickLink
&ClusterId=C-A15B381E446C****
&RegionId=cn-hangzhou
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<RequestId>621094BA-8C21-4199-9F04-CA9AA5B63955</RequestId>
<QuickLinkList>
    <QuickLink>
        <QuickLinkAddress>knox.C-A15B381E446C****.cn-hangzhou.emr.aliyuncs.com:8443/gateway/cluster-topo/sparkhistory/</QuickLinkAddress>
        <Type>KNOX</Type>
        <ServiceName>SPARK</ServiceName>
        <Port>8443</Port>
        <Protocol>https</Protocol>
        <ServiceDisplayName>Spark</ServiceDisplayName>
    </QuickLink>
</QuickLinkList>
```

`JSON` 格式

```
{
	"RequestId": "621094BA-8C21-4199-9F04-CA9AA5B63955",
	"QuickLinkList": {
		"QuickLink": [
			{
				"QuickLinkAddress": "knox.C-A15B381E446C****.cn-hangzhou.emr.aliyuncs.com:8443/gateway/cluster-topo/sparkhistory/",
				"Type": "KNOX",
				"ServiceName": "SPARK",
				"Port": "8443",
				"Protocol": "https",
				"ServiceDisplayName": "Spark"
			}
		]
	}
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|403|Params.Illegal|The specified parameters are wrongly formed..|指定参数格式错误|
|403|User.OtherUserResource.NotAllow|It is not allowed to operate other user's resource|不能操作其它用户的资源|
|403|Invalid.Cluster.Status|Invalid cluster status %s in status list|指定集群状态不合法|
|403|Invalid.Cluster.Type|Invalid cluster type %s in cluster type list|指定集群类型不合法|
|500|InternalError|The request processing has failed due to some unknown error.|内部错误，请提工单|

访问[错误中心](https://error-center.alibabacloud.com/status/product/Emr)查看更多错误码。

