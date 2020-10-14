# RunScalingActionV2

Call RunScalingActionV2 to perform scaling group sample control actions. Such as start, stop, restart, and refresh.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=Emr&api=RunScalingActionV2&type=RPC&version=2016-04-08)

## Request parameters

|Prameter|Type|Required|Example|Description|
|--------|----|--------|-------|-----------|
|Action|String|Required|RunScalingActionV2|The operation that you want to perform. For API requests using the HTTP or HTTPS URL, this parameter is required. Value: RunScalingActionV2. |
|RegionId|String|Required|cn-hangzhou|The region ID of the resource. You can call the [DescribeRegions](~~25609~~)API to view the latest region list. |
|ScalingActionType|String|Required|START\_SCALING\_GROUP|Auto Scaling Engine actions. Valid values:

-   START\_SCALING\_GROUP: Start the scaling Group
-   STOP\_SCALING\_GROUP: stops the scaling group.
-   RESTART\_SCALING\_GROUP: Restart the scaling Group
-   UPDATE\_SCALING\_GROUP\_INSTANCE: refresh the scaling group configuration, apply the latest configuration |
|ScalingGroupBizId|String|Required|SGB-234242ABC\*\*\*\*|The ID of the scaling group. You can call [ListScalingGroupV2](~~184367~~) to view the scaling group ID. |
|ResourceGroupId|String|No|SGB-CA3A16B87BF1\*\*\*\*|The ID of the resource group. You can call the [ListResourceGroups](~~158855~~) to view the resource group ID. |
|ActionParam|String|No|\{\}|Control action parameters. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Data|String|true|Return the result. Valid values:

-   true: The operation was successful.
-   false: The operation failed. |
|RequestId|String|123DF0CD-C5F5-4A8D-984A-C5A4331EB84F|The request ID. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=RunScalingActionV2
&RegionId=cn-hangzhou
&ScalingActionType=START_SCALING_GROUP
&ScalingGroupBizId=SGB-234242ABC****
&<Common request parameters>
```

Sample success responses

`XML` format

```
<RequestId>123DF0CD-C5F5-4A8D-984A-C5A4331EB84F</RequestId>
<Data>true</Data>
```

`JSON` format

```
{
    "RequestId":"123DF0CD-C5F5-4A8D-984A-C5A4331EB84F",
    "Data":"true"
}
```

## Error code

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|403|Params.Illegal|The specified parameters are wrongly formed..|The error message returned because the format of the specified parameters is invalid.|
|500|InternalError|The request processing has failed due to some unknown error.|The error message returned because the request processing has failed due to an internal error. Submit a ticket.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/Emr).

