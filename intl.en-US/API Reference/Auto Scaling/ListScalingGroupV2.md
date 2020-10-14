# ListScalingGroupV2

Call ListScalingGroupV2 to view the scaling group list.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=Emr&api=ListScalingGroupV2&type=RPC&version=2016-04-08)

## Request parameters

|Prameter|Type|Required|Example|Description|
|--------|----|--------|-------|-----------|
|Action|String|Required|ListScalingGroupV2|The operation that you want to perform. For API requests using the HTTP or HTTPS URL, this parameter is required. Value: ListScalingGroupV2. |
|RegionId|String|Required|cn-hangzhou|The region ID of the resource. You can call the [DescribeRegions](~~25609~~)API to view the latest region list. |
|ResourceGroupId|String|No|rg-bp67acfmxazb4p\*\*\*\*|The ID of the resource group. You can call the [ListResourceGroups](~~158855~~) to view the resource group ID. |
|Limit|Integer|No|0|Deprecated field. |
|PageNumber|Integer|No|1|The page number of the returned page. |
|PageSize|Integer|No|100|The number of records on a single page. |
|CurrentSize|Integer|No|0|Deprecated field. |
|PageCount|Integer|No|0|Deprecated field. |
|OrderField|String|No|id|Deprecated field. |
|OrderMode|String|No|desc|Deprecated field. |
|ClusterBizId|String|No|C-ABC12323\*\*\*\*|The ID of the cluster. You can call the [ListClusters](~~28147~~)operation to query the cluster ID. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Items|Array of Item| |Scaling group list |
|Item| | | |
|ActiveStatus|String|ACTIVE|The activity status of the scaling group. Valid values:

-   ACTIVE: The scaling group is activated.
-   INACTIVE: the scaling group is inactived. |
|Description|String|This is an example scaling Group|The description of the scaling group. |
|HostGroupBizId|String|G-3242ABC24\*\*\*\*|The ID of the host group. You can call the [ListClusterHostGroup](~~128432~~)view Machine Group ID. |
|Name|String|test|The name of the scaling group. |
|ScalingGroupId|String|SGB-242342ABC\*\*\*\*|The ID of the scaling group. You can call [ListScalingGroupV2](~~184367~~) to view the scaling group ID. |
|ScalingInMode|String|GRACEFUL|The scaling mode. Valid values:

-   GRACEFUL: GRACEFUL disconnection.
-   DEFAULT: DEFAULT value. |
|ScalingMaxSize|Integer|10|The maximum number of nodes in the scaling group. Maximum Value: 500. |
|ScalingMinSize|Integer|0|The minimum number of nodes in the scaling group. The minimum value is 0. |
|NextToken|String|None|Deprecated field. |
|PageNumber|Integer|1|The page number of the returned page. |
|PageSize|Integer|100|The number of records on a single page. |
|RequestId|String|123DF0CD-C5F5-4A8D-984A-C5A4331EB84F|The ID of the request. |
|TotalCount|Integer|1|The total number of entries returned. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=ListScalingGroupV2
&RegionId=cn-hangzhou
&<Common request parameters>
```

Sample success responses

`XML` format

```
<TotalCount>100</TotalCount>
<RequestId>123DF0CD-C5F5-4A8D-984A-C5A4331EB84F</RequestId>
<PageSize>100</PageSize>
<PageNumber>1</PageNumber>
<Items>
    <Item>
        <Description> this is an example scaling group </Description>
        <HostGroupBizId>G-3242ABC24****</HostGroupBizId>
        <ScalingInMode>GRACEFUL</ScalingInMode>
        <ScalingGroupId>SGB-242342ABC****</ScalingGroupId>
        <ScalingMaxSize>10</ScalingMaxSize>
        <ScalingMinSize>0</ScalingMinSize>
        <Name>test</Name>
        <ActiveStatus>ACTIVE</ActiveStatus>
    </Item>
</Items>
```

`JSON` format

```
{
    "TotalCount":"100",
    "RequestId":"123DF0CD-C5F5-4A8D-984A-C5A4331EB84F",
    "PageSize":"100",
    "PageNumber":"1",
    "Items":
    {
        "Item":
        [
            {
                "Description":"This is a sample scaling group.",
                "HostGroupBizId":"G-3242ABC24****",
                "ScalingInMode":"GRACEFUL",
                "ScalingGroupId":"SGB-242342ABC****",
                "ScalingMaxSize":"10",
                "ScalingMinSize":"0",
                "Name":"test",
                "ActiveStatus":"ACTIVE"
                }
         ]
       }
}
```

## Error code

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|403|Params.Illegal|The specified parameters are wrongly formed..|The error message returned because the format of the specified parameters is invalid.|
|500|InternalError|The request processing has failed due to some unknown error.|The error message returned because the request processing has failed due to an internal error. Submit a ticket.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/Emr).

