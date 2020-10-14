# ListScalingActivityV2

Call ListScalingActivityV2 to query the scaling activity list.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=Emr&api=ListScalingActivityV2&type=RPC&version=2016-04-08)

## Request parameters

|Prameter|Type|Required|Example|Description|
|--------|----|--------|-------|-----------|
|Action|String|Required|ListScalingActivityV2|The operation that you want to perform. For API requests using the HTTP or HTTPS URL, this parameter is required. Value: ListScalingActivityV2. |
|ClusterBizId|String|Required|C-12324352352\*\*\*\*|The ID of the cluster. You can call the [ListClusters](~~28147~~)operation to query the cluster ID. |
|RegionId|String|Required|cn-hangzhou|The region ID of the resource. You can call the [DescribeRegions](~~25609~~)API to view the latest region list. |
|ResourceGroupId|String|No|-1|The ID of the resource group. You can call the [ListResourceGroups](~~158855~~) to view the resource group ID. |
|Limit|Integer|No|0|Deprecated field. |
|PageNumber|Integer|No|1|The page number of the returned page. |
|PageSize|Integer|No|100|The number of records on a single page. |
|CurrentSize|Integer|No|0|Deprecated field. |
|PageCount|Integer|No|0|Deprecated field. |
|OrderField|String|No|id|The paging parameters. |
|OrderMode|String|No|desc|The paging parameters. |
|HostGroupId|String|No|G-2342423\*\*\*\*|The ID of the host group. You can call the [ListClusterHostGroup](~~128432~~)view Machine Group ID. |
|ScalingGroupBizId|String|No|SGB-A2343453\*\*\*\*|The ID of the scaling group. You can call [ListScalingGroupV2](~~AAAA~~) to view the scaling group ID. |
|ScalingRuleName|String|No|Load expansion -1|The name of the scaling rule. |
|HostGroupName|String|No|Scaling Group 1|The name of the machine group. |
|Status|String|No|Successful|The status of the activity. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Items|Array of Item| |Scaling activity list |
|Item| | | |
|BizId|String|asa-bp18jfxw7gqnkhtz\*\*\*\*|The ID of the scaling activity. |
|Cause|String|A user requests to execute scaling rule \\"asr-bp1d8mebzisgwu34mcn3\\", changing the Total Capacity from \\"0\\" to \\"1\\".|The reason for the scaling activity. |
|Description|String|\\"1\\" ECS instances are added|The description of the file. |
|EndTime|Long|1600691996000|The end of an interval. |
|ExpectNum|Integer|1|The number of expected instances. |
|HostGroupBizId|String|G-4A1833AA86EA\*\*\*\*|The ID of the host group. |
|HostGroupName|String|Scaling Group 1|The name of the machine group. |
|InstanceIds|String|\[\\"i-bp1ci2e4xqt9eqjqtfrr\\"\]|The list of instances. |
|ScalingRuleId|String|asr-bp19zruz0q7pru31\*\*\*\*|The ID of the scaling rule. |
|ScalingRuleName|String|Load expansion -1|The name of the scaling rule. |
|StartTime|Long|1600691996000|Start time. |
|Status|String|Successful|The status of the activity. Valid values:

-   Successful
-   Failed
-   Abandon
-   Reject |
|TotalCapacity|Integer|10|The total capacity of the scaling group. |
|Transition|String|SCALE\_OUT|The scaling type. Valid values:

-   SCALE\_OUT: indicates the scale-out activity.
-   SCALE\_IN: scale-in activity |
|NextToken|String|None|Deprecated field. |
|PageNumber|Integer|1|The page number of the returned page. |
|PageSize|Integer|100|The number of records on a single page. |
|RequestId|String|123DF0CD-C5F5-4A8D-984A-C5A4331EB84F|The ID of the request. |
|TotalCount|Integer|100|The total number of scaling activity records. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=ListScalingActivityV2
&ClusterBizId=C-12324352352****
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
        <Status>Successful</Status>
        <Description>\"1\" ECS instances are added</Description>
        <EndTime>1600691996000</EndTime>
        <StartTime>1600691996000</StartTime>
        <ScalingRuleId>asr-bp19zruz0q7pru31****</ScalingRuleId>
        <HostGroupName> scaling group 1 </HostGroupName>
        <ScalingRuleName> load expansion -1 </ScalingRuleName>
        <HostGroupBizId>G-4A1833AA86EA****</HostGroupBizId>
        <ExpectNum>1</ExpectNum>
        <Cause>A user requests to execute scaling rule \"asr-bp1d8mebzisgwu34mcn3\", changing the Total Capacity from \"0\" to \"1\". </Cause>
        <Transition>SCALE_OUT</Transition>
        <TotalCapacity>10</TotalCapacity>
        <InstanceIds>[\"i-bp1ci2e4xqt9eqjqtfrr\"]</InstanceIds>
        <BizId>asa-bp18jfxw7gqnkhtz****</BizId>
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
        "Item":[
            {
                "Status":"Successful",
                "Description":"\\\"1\\\" ECS instances are added",
                "EndTime":"1600691996000",
                "StartTime":"1600691996000",
                "ScalingRuleId":"asr-bp19zruz0q7pru31****",
                "HostGroupName":"Scaling Group 1",
                "ScalingRuleName":"Load expansion -1",
                "HostGroupBizId":"G-4A1833AA86EA****",
                "ExpectNum":"1",
                "Cause":"A user requests to execute scaling rule \\\"asr-bp1d8mebzisgwu34mcn3\\\", changing the Total Capacity from \\\"0\\\" to \\\"1\\\".",
                "Transition":"SCALE_OUT",
                "TotalCapacity":"10",
                "InstanceIds":"[\\\"i-bp1ci2e4xqt9eqjqtfrr\\\"]",
                "BizId":"asa-bp18jfxw7gqnkhtz****"
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

