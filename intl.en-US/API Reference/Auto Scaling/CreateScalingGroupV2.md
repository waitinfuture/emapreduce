# CreateScalingGroupV2

Call CreateScalingGroupV 2 to create a scaling group.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=Emr&api=CreateScalingGroupV2&type=RPC&version=2016-04-08)

## Request parameters

|Prameter|Type|Required|Example|Description|
|--------|----|--------|-------|-----------|
|Action|String|Required|CreateScalingGroupV2|The operation that you want to perform. For API requests using the HTTP or HTTPS URL, this parameter is required. Value: CreateScalingGroupV2. |
|Description|String|Required|This is an example scaling Group|The scaling group description. |
|HostGroupId|String|Required|G-AB1234567\*\*\*\*|The ID of the host group. You can call the [ListClusterHostGroup](~~128432~~)view Machine Group ID. |
|Name|String|Required|test|The name of the scaling group. The account can be used to monitor specific types of resources. |
|RegionId|String|Required|cn-hangzhou|The ID of the region to which the cluster belongs. You can call the [DescribeRegions](~~25609~~)API to view the latest region list. |
|ResourceGroupId|String|No|-1|The ID of the resource group. You can call the [ListResourceGroups](~~158855~~) to view the resource group ID. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Data|String|SG-123456789\*\*\*\*|The ID of the scaling group. |
|RequestId|String|6C96FD2C-95A0-4C03-8A19-7D84A4BAAA1E|The request ID. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=CreateScalingGroupV2
&Description=This is a sample scaling Group
&HostGroupId=G-AB1234567****
&Name=test
&RegionId=cn-hangzhou
&<Common request parameters>
```

Sample success responses

`XML` format

```
<RequestId>6C96FD2C-95A0-4C03-8A19-7D84A4BAAA1E</RequestId>
<Data>SG-123456789****</Data>
```

`JSON` format

```
{
    "RequestId":"6C96FD2C-95A0-4C03-8A19-7D84A4BAAA1E",
    "Data":"SG-123456789****"
}
```

## Error code

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|403|Params.Illegal|The specified parameters are wrongly formed..|The error message returned because the format of the specified parameters is invalid.|
|500|InternalError|The request processing has failed due to some unknown error.|The error message returned because the request processing has failed due to an internal error. Submit a ticket.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/Emr).

