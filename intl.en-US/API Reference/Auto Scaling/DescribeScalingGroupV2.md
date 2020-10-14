# DescribeScalingGroupV2

Call DescribeScalingGroupV2 to obtain the scaling group details.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=Emr&api=DescribeScalingGroupV2&type=RPC&version=2016-04-08)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Required|DescribeScalingGroupV2|The operation that you want to perform. For API requests using the HTTP or HTTPS URL, this parameter is required. Value: DescribeScalingGroupV2. |
|RegionId|String|Required|cn-hangzhou|The region ID of the security group to be queried. You can call the [DescribeRegions](~~25609~~) operation to query the most recent region list. |
|ResourceGroupId|String|No|rg-acfmv6jutt6\*\*\*\*|The ID of the resource group. You can call the [ListResourceGroups](~~158855~~) to view the resource group ID. |
|ScalingGroupBizId|String|No|SGB-232432123245\*\*\*\*|The ID of the scaling group. You can call [ListScalingGroupV2](~~184367~~) to view the scaling group ID. |
|HostGroupBizId|String|No|G-AB342535346545\*\*\*\*|The ID of the host group. You can call the [ListClusterHostGroup](~~128432~~)view Machine Group ID. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|ActiveStatus|String|ACTIVE|Scaling group activity status:

-   ACTIVE: The scaling group is activated.
-   INACTIVE: the scaling group is INACTIVE. |
|ConfigState|String|APPLIED|Scaling Configuration Status:

-   APPLIED: APPLIED
-   MODIFIED: The fields have been MODIFIED and have not taken effect. |
|Description|String|This is an example|The description of the scaling group. |
|HostGroupBizId|String|G-5011FB3E4928\*\*\*\*|The machine group associated with the scaling group. |
|Name|String|Sample scaling Group|The name of the scaling group. |
|RequestId|String|C390A685-2707-4F42-BCFA-E4BC40E4B7A3|The ID of the request. |
|ScalingGroupId|String|SGB-16ABA17988F3\*\*\*\*|The ID of the scaling group. |
|ScalingInMode|String|GRACEFUL|Scaling type:

-   NORMAL: Auto Scaling
-   GRACEFUL: GRACEFUL disconnection. |
|ScalingMaxSize|Integer|10|The maximum number of nodes in the scaling group. |
|ScalingMinSize|Integer|1|The minimum number of nodes in the scaling group. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=DescribeScalingGroupV2
&RegionId=cn-hangzhou
&<Common request parameters>
```

Sample success responses

`XML` format

```
<Description> this is an example </Description>
<HostGroupBizId>G-5011FB3E4928****</HostGroupBizId>
<RequestId>C390A685-2707-4F42-BCFA-E4BC40E4B7A3</RequestId>
<ConfigState>APPLIED</ConfigState>
<ScalingInMode>GRACEFUL</ScalingInMode>
<ScalingGroupId>SGB-16ABA17988F39****</ScalingGroupId>
<ScalingMaxSize>10</ScalingMaxSize>
<ScalingMinSize>1</ScalingMinSize>
<Name> sample scaling group </Name>
<ActiveStatus>ACTIVE</ActiveStatus>
```

`JSON` format

```
{
    "Description":"This is an example.",
    "HostGroupBizId":"G-5011FB3E4928****",
    "RequestId":"C390A685-2707-4F42-BCFA-E4BC40E4B7A3",
    "ConfigState":"APPLIED",
    "ScalingInMode":"GRACEFUL",
    "ScalingGroupId":"SGB-16ABA17988F39****",
    "ScalingMaxSize":"10",
    "ScalingMinSize":"1",
    "Name":"sample scaling group",
    "ActiveStatus":"ACTIVE"
    }
```

## Error codes

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|403|Params.Illegal|The specified parameters are wrongly formed..|The error message returned because the format of the specified parameters is invalid.|
|500|InternalError|The request processing has failed due to some unknown error.|The error message returned because the request processing has failed due to an internal error. Submit a ticket.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/Emr).

