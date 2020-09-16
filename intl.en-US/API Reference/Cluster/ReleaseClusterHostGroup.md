# ReleaseClusterHostGroup

Call the ReleaseClusterHostGroup operation to remove nodes from an EMR cluster.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=Emr&api=ReleaseClusterHostGroup&type=RPC&version=2016-04-08)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|ReleaseClusterHostGroup|The operation that you want to perform. For API requests using the HTTP or HTTPS URL, this parameter is required. Valid values: ReleaseClusterHostGroup |
|ClusterId|String|Yes|C-D7958B72E59B\*\*\*\*|The ID of the PolarDB cluster. |
|HostGroupId|String|Yes|G-EF460256A55F\*\*\*\*|The ID of the machine Group, which can be obtained by using the ListClusterHostGroup. |
|RegionId|String|Yes|cn-hangzhou|The ID of the region. |
|InstanceIdList|String|No|\["i-bp1bm7y86rscdx1a\*\*\*\*"\]|The IDs of the ECS instances. |

## Response parameters

|Parameter|Command|Example|Description|
|---------|-------|-------|-----------|
|RequestId|String|991B3409-6C8D-48CB-903C-3B9C166E17A8|The ID of the request. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=ReleaseClusterHostGroup
&ClusterId=C-D7958B72E59B****
&HostGroupId=G-EF460256A55F****
&RegionId=cn-hangzhou
&<Common request parameters>
```

Sample success responses

`XML` format

```
<RequestId>991B3409-6C8D-48CB-903C-3B9C166E17A8</RequestId>
```

`JSON` format

```
{
    "RequestId":"991B3409-6C8D-48CB-903C-3B9C166E17A8"
}
```

## Error codes

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|403|Params.Illegal|The specified parameters are wrongly formed..|The error message returned because the format of the specified parameters is invalid.|
|500|InternalError|The request processing has failed due to some unknown error.|The error message returned because the request processing has failed due to an internal error. Submit a ticket.|
|404|ClusterId.NotFound|The ClusterId provided does not exist in our records.|The error message returned because the specified cluster ID does not exist. Enter a valid value.|
|500|InternalError|The request processing has failed due to some unknown error\(code:5100\)|The error message returned because the request processing has failed due to an internal error. Submit a ticket.|
|403|User.OtherUserResource.NotAllow|It is not allowed to operate other user's resource|The error message returned because you are not authorized to manage the resources of other users.|
|403|User.Account.Abnormal|The User Account maybe is out of service!|The error message returned because the Apsara Stack tenant account is out of service.|
|403|Cluster.Role.NotConfirm|The Role for EMR service to operate is Not confirmed!|The error message returned because the Resource Access Management \(RAM\) role you created for the EMR service is not granted the required permissions. Grant the required permissions and try again.|
|404|ClusterId.NotFound|ClusterId \[%s\] does not exist.|The error message returned because the specified cluster ID does not exist. Make sure that the cluster ID is valid.|
|400|Prepaid.Cluster.Cannot.Release|Prepaid cluster do not support release operation!|The error message returned because the nodes in a subscription cluster cannot be released.|
|400|Cluster.Cannot.Release|Cluster has been release or did not start normally!|The error message returned because the cluster is invalid. Select a valid cluster.|
|400|Cluster.RegionId.NotMatch|Cluster does not exist in region \[%s\]!|The error message returned because the cluster does not belong to the region. Switch to the region to which the cluster belongs.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/Emr).

