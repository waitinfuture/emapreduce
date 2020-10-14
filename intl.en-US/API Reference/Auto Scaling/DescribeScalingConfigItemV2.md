# DescribeScalingConfigItemV2

Call DescribeScalingConfigItemV2 to obtain the scaling configuration items details.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=Emr&api=DescribeScalingConfigItemV2&type=RPC&version=2016-04-08)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Required|DescribeScalingConfigItemV2|The operation that you want to perform. For API requests using the HTTP or HTTPS URL, this parameter is required. Value: DescribeScalingConfigItemV2. |
|ConfigItemType|String|Required|SCALING\_RULE|Configuration items types

-   SCALING\_RULE: scaling rules
-   SCALING\_STRATEGY: scaling policy |
|RegionId|String|Required|cn-hangzhou|The region ID of the resource. You can call the [DescribeRegions](~~25609~~)API to view the latest region list. |
|ScalingConfigItemId|String|Required|SRB-0F2A154CFD4D\*\*\*\*|Scaling group configuration items ID. You can call the [ListScalingConfigItemV2](~~184368~~)view the auto scaling configuration items ID. |
|ScalingGroupBizId|String|Required|SGB-8E52011DD26C\*\*\*\*|The ID of the scaling group. You can call [ListScalingGroupV2](~~AAAA~~) to view the scaling group ID. |
|ResourceGroupId|String|No|-1|The ID of the resource group. You can call the [ListResourceGroups](~~158855~~) to view the resource group ID. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|ConfigItemInformation|String|\{"ruleName": "triggered by load", "adjustmentType": "QUANTITY\_CHANGE\_IN\_CAPACITY", "coolDownTime": 100, "ruleParam": \{ "metricName": "YarnFreeCores", "period": 10, "statistics": "xxx", "comparisonOperator": ""\\u003e\\u003d ", " threshold ": 10, " evaluationCount ": 1 \}, " adjustmentValue ": 1, "ruleType": "BY\_LOAD", "configItemType": "SCALING\_RULE"\}|Configuration Items information. The information varies with the configuration items type.

-   SCALING\_RULE:
    -   Trigger by load`: { "ruleName": "Trigger by load", "adjustmentType": "QUANTITY_CHANGE_IN_CAPACITY", "coolDownTime": 100, "ruleParam": { "metricName": "YarnFreeCores", "period": 10, "statistics": "xxx", "comparisonOperator": ""\u003e\u003d ", " threshold ": 10, " evaluationCount ": 1 }, "adjustmentValue": 1, "ruleType": "BY_LOAD", "configItemType": "SCALING_RULE"}`
    -   Scheduled node`: { "ruleName": "scheduled node", "adjustmentType": "QUANTITY_CHANGE_IN_CAPACITY", "Cooldown": 100, "ruleParam": { "recurrenceType": "Daily", "recurrenceValue": "1", "recurrenceEndTime": "2007-22t03:" 01Z ", " launchTime ": " 2020-07-22T03:07Z ", " launchExpirationTime ": 0 }, " adjustmentValue": 1, "ruleType": "SCHEDULED", "configItemType": "SCALING_RULE"}`
    -   `Set-`point execution`{"ruleName": "specified execution", "adjustmentType": "QUANTITY_CHANGE_IN_CAPACITY", "coolDownTime": 100, "ruleParam": { "launchTime": "2020-07-22T03:09Z", "launchExpirationTime": 1 }, "adjustmentValue": 1, "ruleType:" "BY_TIME_ONCE", "configItemType": "SCALING_RULE"}`
-   SCALING\_STRATEGY:`{"spotStrategy": "NoSpot", "spotPriceLimits": 0.01, "instanceTypeList":[],"sysDiskCategory": "cloud_essd", "sysDiskSize": { "value": 40.0, "unit": "GIGABYTE" }, "dataDiskCategory": "cloud_essd", "dataDiskSize": { "value": 40.0," unit": "GIGABYTE" }, "dataDiskCount": 4, "scalingMaxSize": 1, "scalingMinSize": 1, "defaultCoolDownTime": 0, "scalingTimeoutPolice": { "timeoutPolicy": "ROLLBACK" }, "nodeOfflineMode": "NORMAL", "nodeOfflineModeParam": { "timeoutMs": 0 }, "triggerMode":" Scheduled", "multiAvailablePolicy": "PRIORITY", "multiAvailablePolicyParam": { "onDemandBaseCapacity": 0, "onDemandPercentageAboveBaseCapacity": 0, "spotInstanceRemedy": 0, "spotInstance": false }, "configItemType": "SCALING_STRATEGY"}` |
|ConfigItemType|String|SCALING\_RULE|Configuration items types

-   Scaling\_ruby: a scaling rule.
-   SCALING\_STRATEGY: scaling policy |
|RequestId|String|6C96FD2C-95A0-4C03-8A19-7D84A4BAAA1E|The ID of the request. |
|ScalingConfigItemBizId|String|SRB-0F2A154CFD4D\*\*\*\*|Scaling group configuration items ID. |
|ScalingGroupBizId|String|SGB-0F2A154CFD4D\*\*\*\*|The ID of the scaling group. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=DescribeScalingConfigItemV2
&ConfigItemType=SCALING_RULE
&RegionId=cn-hangzhou
&ScalingConfigItemId=SCB-8E52011DD26123****
&ScalingGroupBizId=SGB-8E52011DD26C****
&<Common request parameters>
```

Sample success responses

`XML` format

```
<RequestId>6C96FD2C-95A0-4C03-8A19-7D84A4BAAA1E</RequestId>
<ScalingConfigItemBizId>SRB-0F2A154CFD4D****</ScalingConfigItemBizId>
<ScalingGroupBizId>SGB-0F2A154CFD4D****</ScalingGroupBizId>
<ConfigItemInformation> {"ruleName": "triggered by load", "adjustmentType": "QUANTITY_CHANGE_IN_CAPACITY", "coolDownTime": 100, "ruleParam": { "metricName": "YarnFreeCores", "period": 10, "statistics": "xxx", "comparisonOperator": "\u003e\u003d", "threshold": 10, "evaluationCount": 1 }, " adjustmentValue": 1, "ruleType": "BY_LOAD", "configItemType": "SCALING_RULE"} </ConfigItemInformation>
<ConfigItemType>SCALING_RULE</ConfigItemType>
```

`JSON` format

```
{
    "RequestId":"6C96FD2C-95A0-4C03-8A19-7D84A4BAAA1E",
    "ScalingConfigItemBizId":"SRB-0F2A154CFD4D****",
    "ScalingGroupBizId":"SGB-0F2A154CFD4D****",
    "ConfigItemInformation":"{ \" ruleName\": \" triggered by load \", \" adjustmentType\": \" QUANTITY_CHANGE_IN_CAPACITY\", \" cooldownload \", \" ruleParam\": { \" metrname \": \" yarnfreecolors \", \" period\": 10, \" statistics\": \" xxx\", \" comparisonOperator\": \"\\u003e\\u003d\", \" threshold\": 10, \" evaluationCount\": 1 }, \" adjustmentValue\": 1, \" ruleType\": \" BY_LOAD\", \" configItemType\": \" SCALING_RULE\" }",
    "ConfigItemType":"SCALING_RULE"
}
```

## Error codes

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|403|Params.Illegal|The specified parameters are wrongly formed..|The error message returned because the format of the specified parameters is invalid.|
|500|InternalError|The request processing has failed due to some unknown error.|The error message returned because the request processing has failed due to an internal error. Submit a ticket.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/Emr).

