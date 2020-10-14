# RemoveScalingConfigItemV2

Call RemoveScalingConfigItemV2 to delete the scaling configuration items.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=Emr&api=RemoveScalingConfigItemV2&type=RPC&version=2016-04-08)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Required|RemoveScalingConfigItemV2|The operation that you want to perform. For API requests using the HTTP or HTTPS URL, this parameter is required. Value: RemoveScalingConfigItemV2. |
|ConfigItemBizId|String|Required|SCB-12313ABC1\*\*\*\*|configuration items ID. You can call the [ListScalingConfigItemV2](~~184368~~)view configuration items ID. |
|ConfigItemType|String|Required|SCALING\_RULE|Configuration items types

-   SCALING\_RULE: scaling rules
-   SCALING\_STRATEGY: scaling policy |
|RegionId|String|Required|cn-hangzhou|The region ID of the resource. You can call the [DescribeRegions](~~25609~~)API to view the latest region list. |
|ScalingGroupBizId|String|Required|SGB-32242E323\*\*\*\*|The ID of the scaling group. You can call [ListScalingGroupV2](~~184367~~) to view the scaling group ID. |
|ResourceGroupId|String|No|rg-acfmv6jutt6\*\*\*\*|The ID of the resource group. You can call the [ListResourceGroups](~~158855~~) to view the resource group ID. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Data|Boolean|true|Return the result. Valid values:

-   true: The operation was successful.
-   false: The operation failed. |
|RequestId|String|7B804810-3C11-4927-B116-1D1A98A60BDD|The ID of the request. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=RemoveScalingConfigItemV2
&ConfigItemBizId=SCB-12313ABC1****
&ConfigItemType=SCALING_RULE
&RegionId=cn-hangzhou
&ScalingGroupBizId=SGB-32242E323****
&<Common request parameters>
```

Sample success responses

`XML` format

```
<RequestId>7B804810-3C11-4927-B116-1D1A98A60BDD</RequestId>
<Data>true</Data>
```

`JSON` format

```
{
    "RequestId":"7B804810-3C11-4927-B116-1D1A98A60BDD",
    "Data":"true"
}
```

## Error code

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|403|Params.Illegal|The specified parameters are wrongly formed..|The error message returned because the format of the specified parameters is invalid.|
|500|InternalError|The request processing has failed due to some unknown error.|The error message returned because the request processing has failed due to an internal error. Submit a ticket.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/Emr).

