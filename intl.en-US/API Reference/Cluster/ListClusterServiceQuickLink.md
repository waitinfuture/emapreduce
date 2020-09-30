# ListClusterServiceQuickLink

You can call this operation to query the shortcut links of a specified service or all services in a cluster.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=Emr&api=ListClusterServiceQuickLink&type=RPC&version=2016-04-08)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|ListClusterServiceQuickLink|The operation that you want to perform. For API requests using the HTTP or HTTPS URL, this parameter is required. Set the value to ListClusterServiceQuickLink. |
|ClusterId|String|Yes|C-A15B381E446C\*\*\*\*|The ID of the cluster. You can call [ListClusters](~~28147~~) to view the cluster ID. |
|RegionId|String|Yes|cn-hangzhou|The region ID of the resource. You can call [DescribeRegions](~~25609~~) to view the latest list of Alibaba Cloud regions. |
|DirectType|Boolean|No|true|A reserved parameter. |
|ServiceName|String|No|SPARK|Service name:

 -   SPARK
-   RANGER
-   GANGLIA
-   HDFS
-   YARN |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|QuickLinkList|Array of QuickLink| |The list of links. |
|QuickLink| | | |
|Port|String|8443|The service port. |
|Protocol|String|https|The connection protocol. |
|QuickLinkAddress|String|knox.C-A15B381E446C\*\*\*\*.cn-hangzhou.emr.aliyuncs.com:8443/gateway/cluster-topo/sparkhistory/|The URL of the service. |
|ServiceDisplayName|String|Spark|The display name of the service. |
|ServiceName|String|SPARK|The name of the service. |
|Type|String|KNOX|Service type:

 -   DEFAULT: includes HUE and ZEPPELIN.

 **Note:** You have added HUE and ZEPPELIN to the cluster.

 -   KNOX: includes YARN, HDFS, SPARK, GANGLIA, and RANGER. |
|RequestId|String|621094BA-8C21-4199-9F04-CA9AA5B63955|The request ID. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=ListClusterServiceQuickLink
&ClusterId=C-A15B381E446C****
&RegionId=cn-hangzhou
&<Common request parameters>
```

Sample success responses

`XML` format

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

`JSON` format

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

## Error codes

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|403|Params.Illegal|The specified parameters are wrongly formed..|The error message returned because the format of the specified parameters is invalid.|
|403|User.OtherUserResource.NotAllow|It is not allowed to operate other user's resource|The error message returned because you are not authorized to manage the resources of other users.|
|403|Invalid.Cluster.Status|Invalid cluster status %s in status list|The error message returned because the specified cluster status is invalid.|
|403|Invalid.Cluster.Type|Invalid cluster type %s in cluster type list|The error message returned because the type of the specified cluster is invalid.|
|500|InternalError|The request processing has failed due to some unknown error.|The error message returned because the request processing has failed due to an internal error. Submit a ticket.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/Emr).

