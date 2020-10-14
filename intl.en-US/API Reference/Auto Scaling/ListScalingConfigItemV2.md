# ListScalingConfigItemV2

Call ListScalingConfigItemV2 to query the list of scaling configuration items.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=Emr&api=ListScalingConfigItemV2&type=RPC&version=2016-04-08)

## Request parameters

|Prameter|Type|Required|Example|Description|
|--------|----|--------|-------|-----------|
|Action|String|Required|ListScalingConfigItemV2|The operation that you want to perform. For API requests using the HTTP or HTTPS URL, this parameter is required. Value: ListScalingConfigItemV2. |
|ConfigItemType|String|Required|SCALING\_RULE|The type of the configuration parameters. Valid values:

-   SCALING\_RULE: scaling rules
-   SCALING\_STRATEGY: scaling policy |
|RegionId|String|Required|cn-hangzhou|The region ID of the resource. You can call the [DescribeRegions](~~25609~~)API to view the latest region list. |
|ScalingGroupBizId|String|Required|SGB-16ABA17988F39\*\*\*\*|The ID of the scaling group. You can call [ListScalingGroupV2](~~AAAA~~) to view the scaling group ID. |
|ResourceGroupId|String|No|rg-acfmv6jutt6\*\*\*\*|The ID of the resource group. You can call the [ListResourceGroups](~~158855~~) to view the resource group ID. |
|Limit|Integer|No|0|Deprecated field. |
|PageNumber|Integer|No|1|The page number of the returned page. |
|PageSize|Integer|No|100|The number of records on a single page. |
|CurrentSize|Integer|No|0|Deprecated field. |
|PageCount|Integer|No|0|Deprecated field. |
|OrderField|String|No|id|Deprecated field. |
|OrderMode|String|No|desc|Deprecated field. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Items|Array of Item| |Configuration Items list |
|Item| | | |
|ConfigItemInformation|String|\{"ruleName": "triggered by load", "adjustmentType": "QUANTITY\_CHANGE\_IN\_CAPACITY", "coolDownTime": 100, "ruleParam": \{ "metricName": "YarnFreeCores", "period": 10, "statistics": "xxx", "comparisonOperator": ""\\u003e\\u003d ", " threshold ": 10, " evaluationCount ": 1 \}, " adjustmentValue ": 1, "ruleType": "BY\_LOAD", "configItemType": "SCALING\_RULE"\}|Configuration Items information. The information varies with the configuration items type. Valid values:

-   SCALING\_RULE:
    -   Trigger by load`: { "ruleName": "Trigger by load", "adjustmentType": "QUANTITY_CHANGE_IN_CAPACITY", "coolDownTime": 100, "ruleParam": { "metricName": "YarnFreeCores", "period": 10, "statistics": "xxx", "comparisonOperator": ""\u003e\u003d ", " threshold ": 10, " evaluationCount ": 1 }, "adjustmentValue": 1, "ruleType": "BY_LOAD", "configItemType": "SCALING_RULE"}`
    -   Scheduled node`: { "ruleName": "scheduled node", "adjustmentType": "QUANTITY_CHANGE_IN_CAPACITY", "Cooldown": 100, "ruleParam": { "recurrenceType": "Daily", "recurrenceValue": "1", "recurrenceEndTime": "2007-22t03:" 01Z ", " launchTime ": " 2020-07-22T03:07Z ", " launchExpirationTime ": 0 }, " adjustmentValue": 1, "ruleType": "SCHEDULED", "configItemType": "SCALING_RULE"}`
    -   `Set-`point execution`{"ruleName": "specified execution", "adjustmentType": "QUANTITY_CHANGE_IN_CAPACITY", "coolDownTime": 100, "ruleParam": { "launchTime": "2020-07-22T03:09Z", "launchExpirationTime": 1 }, "adjustmentValue": 1, "ruleType:" "BY_TIME_ONCE", "configItemType": "SCALING_RULE"}`
-   SCALING\_STRATEGY:`{"spotStrategy": "NoSpot", "spotPriceLimits": 0.01, "instanceTypeList":[],"sysDiskCategory": "cloud_essd", "sysDiskSize": { "value": 40.0, "unit": "GIGABYTE" }, "dataDiskCategory": "cloud_essd", "dataDiskSize": { "value": 40.0," unit": "GIGABYTE" }, "dataDiskCount": 4, "scalingMaxSize": 1, "scalingMinSize": 1, "defaultCoolDownTime": 0, "scalingTimeoutPolice": { "timeoutPolicy": "ROLLBACK" }, "nodeOfflineMode": "NORMAL", "nodeOfflineModeParam": { "timeoutMs": 0 }, "triggerMode":" Scheduled", "multiAvailablePolicy": "PRIORITY", "multiAvailablePolicyParam": { "onDemandBaseCapacity": 0, "onDemandPercentageAboveBaseCapacity": 0, "spotInstanceRemedy": 0, "spotInstance": false }, "configItemType": "SCALING_STRATEGY"}` |
|ConfigItemType|String|SCALING\_RULE|The type of the configuration parameters. |
|ScalingConfigItemBizId|String|SRB-54CCB030511A\*\*\*\*|configuration items ID. |
|ScalingGroupBizId|String|SGB-CA3A16B87BF1\*\*\*\*|The ID of the scaling group. |
|NextToken|String|None|Deprecated field. |
|PageNumber|Integer|1|The page number of the returned page. |
|PageSize|Integer|100|The number of records on a single page. |
|RequestId|String|7B804810-3C11-4927-B116-1D1A98A60BDD|The ID of the request. |
|TotalCount|Integer|100|The total number of entries returned. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=ListScalingConfigItemV2
&ConfigItemType=SCALING_RULE
&RegionId=cn-hangzhou
&ScalingGroupBizId=SGB-16ABA17988F3****
&<Common request parameters>
```

Sample success responses

`XML` format

```
<TotalCount>100</TotalCount>
<RequestId>7B804810-3C11-4927-B116-1D1A98A60BDD</RequestId>
<PageSize>100</PageSize>
<NextToken>None</NextToken>
<PageNumber>1</PageNumber>
<Items>
    <Item>
        <ScalingConfigItemBizId>SRB-54CCB030511A****</ScalingConfigItemBizId>
        <ScalingGroupBizId>SGB-CA3A16B87BF1****</ScalingGroupBizId>
        <ConfigItemInformation> {"ruleName":"triggered by load","ruleType":"BY_LOAD","ruleParam":{"metricName":"YarnFreeCores","statistics":"xxx","comparisonOperator":"\u003e\u003d","period":10,"threshold":10,"evaluationCount":1},"adjustmentType":"QUANTITY_CHANGE_IN_CAPACITY","adjustmentValue":1,"coolDownTime":100,"configItemType":"SCALING_RULE","scalingConfigBizId":"SCB-3580B6EA94A1 threshold threshold parameter *"} </ConfigItemInformation>
        <ConfigItemType>SCALING_RULE</ConfigItemType>
    </Item>
    <Item>
        <ScalingConfigItemBizId>SRB-DA382429BA82****</ScalingConfigItemBizId>
        <ScalingGroupBizId>SGB-CA3A16B87BF1****</ScalingGroupBizId>
        <ConfigItemInformation> {"ruleName":"triggered by load","ruleType":"BY_LOAD","ruleParam":{"metricName":"YarnFreeCores","statistics":"xxx","comparisonOperator":"\u003e\u003d","period":10,"threshold":10,"evaluationCount":1},"adjustmentType":"QUANTITY_CHANGE_IN_CAPACITY","adjustmentValue":1,"coolDownTime":100,"configItemType":"SCALING_RULE","scalingConfigBizId":"SCB-3580B6EA94A1 threshold threshold parameter *"} </ConfigItemInformation>
        <ConfigItemType>SCALING_RULE</ConfigItemType>
    </Item>
</Items>
```

`JSON` format

```
{
    "TotalCount":"100",
    "RequestId":"7B804810-3C11-4927-B116-1D1A98A60BDD",
    "PageSize":"100",
    "NextToken":"None",
    "PageNumber":"1",
    "Items":
    {
        "Item": [
            {
                "ScalingConfigItemBizId": "SRB-54CCB030511A****",
                "ScalingGroupBizId": "SGB-CA3A16B87BF1****",
                "ConfigItemInformation": "{\" ruleName\":\" triggered by load \",\" ruleType\":\" BY_LOAD\",\" ruleParam\":[\" metricName\":\" YarnFreeCores\",\" statistics\":\" xxx\",\" comparisonOperator\":\"\\u003e\\u003d\",\" period\":10,\" threshold\":10,\" evaluationCount\":1 },\" adjustmentType\":\" QUANTITY_CHANGE_IN_CAPACITY\",\" adjustmentValue\":1,\" cooldown \":100,\" configItemType\":\" SCALING_RULE\",\" scalingConfigBizId\":\" SCB-3580B6EA94A1 cooldown *\"}",
                "ConfigItemType": "SCALING_RULE"
            },
            {
                "ScalingConfigItemBizId": "SRB-DA382429BA82****",
                "ScalingGroupBizId": "SGB-CA3A16B87BF1****",
                "ConfigItemInformation": "{\" ruleName\":\" triggered by load \",\" ruleType\":\" BY_LOAD\",\" ruleParam\":[\" metricName\":\" YarnFreeCores\",\" statistics\":\" xxx\",\" comparisonOperator\":\"\\u003e\\u003d\",\" period\":10,\" threshold\":10,\" evaluationCount\":1 },\" adjustmentType\":\" QUANTITY_CHANGE_IN_CAPACITY\",\" adjustmentValue\":1,\" cooldown \":100,\" configItemType\":\" SCALING_RULE\",\" scalingConfigBizId\":\" SCB-3580B6EA94A1 cooldown *\"}",
                "ConfigItemType": "SCALING_RULE"
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

