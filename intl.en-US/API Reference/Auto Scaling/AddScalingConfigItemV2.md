# AddScalingConfigItemV2

Call AddScalingConfigItemV2 to create a auto scaling configuration items.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=Emr&api=AddScalingConfigItemV2&type=RPC&version=2016-04-08)

## Request parameters

|Prameter|Type|Required|Example|Description|
|--------|----|--------|-------|-----------|
|Action|String|Required|AddScalingConfigItemV2|The operation that you want to perform. For API requests using the HTTP or HTTPS URL, this parameter is required. Value: AddScalingConfigItemV2. |
|ConfigItemInformation|String|Required|\{"ruleName": "triggered by load", "adjustmentType": "QUANTITYCHANGEINCAPACITY", "coolDownTime": 100, "ruleParam": \{ "metricName": "YarnFreeCores", "period": 10, "statistics": "xxx", "comparisonOperator": ""\\u003e\\u003d ", " threshold ": 10, " evaluationCount ": 1 \}, " adjustmentValue ": 1, "ruleType": "BYLOAD", "configItemType": "SCALING\_RULE"\}|Configuration Items values in JSON format vary depending on the data type. Valid values:

&bull; SCALING\_RULE:

-   Trigger by load

`{"ruleName": "triggered by load", "adjustmentType": "QUANTITY_CHANGE_IN_CAPACITY", "coolDownTime": 100, "ruleParam": { "metricName": "YarnFreeCores", "period": 10, "statistics": "xxx", "comparisonOperator": ""\u003e\u003d ", " threshold ": 10, " evaluationCount ": 1 }, " adjustmentValue ": 1, "ruleType": "BY_LOAD", "configItemType": "SCALING_RULE"}`-   Timed-based schedules

`{"ruleName": "Timing Schedule", "adjustmenttype": "QUANTITY_CHANGE_IN_CAPACITY", "cooldowntime": 100, "ruleparam": { "recurrencetype": "Daily", "recurrenceValue": "1", "recurrenceEndTime": "2020-07-22T03:01Z", "launchtime": "2020-07-22T03:07Z", "launchExpirationTime": 0 }, "adjustmentValue": 1, "ruleType": "SCHEDULED", "configItemType": "SCALING_RULE"}`-   Targeted execution

`{"ruleName": "targeted execution", "adjustmentType": "QUANTITY_CHANGE_IN_CAPACITY", "coolDownTime": 100, "ruleParam": { "launchTime": "2020-07-22T03:09Z", "launchExpirationTime": 1 }, "adjustmentValue": 1, "ruleType": "BY_TIME_ONCE", "configItemType": "SCALING_RULE"}`&bull; SCALING\_STRATEGY:

`{ "spotStrategy": "NoSpot", "spotPriceLimits": 0.01, "instanceTypeList": [], "sysDiskCategory": "cloud_essd", "sysDiskSize": { "value": 40.0, "unit": "GIGABYTE" }, "dataDiskCategory": "cloud_essd", "dataDiskSize": { "value": 40.0, "unit": "GIGABYTE" }, "dataDiskCount": 4, "scalingMaxSize": 1, "scalingMinSize": 1, "defaultCoolDownTime": 0, "scalingTimeoutPolice": { "timeoutPolicy": "ROLLBACK" }, "nodeOfflineMode": "NORMAL", "nodeOfflineModeParam": { "timeoutMs": 0 }, "triggerMode": "Scheduled", "multiAvailablePolicy": "PRIORITY", "multiAvailablePolicyParam": { "onDemandBaseCapacity": 0, "onDemandPercentageAboveBaseCapacity": 0, "spotInstanceRemedy": 0, "spotInstance": false }, "configItemType": "SCALING_STRATEGY" }`|
|ConfigItemType|String|Required|SCALING\_RULE|The type of the configuration parameters. Valid values:

-   SCALING\_RULE: scaling rules
-   SCALING\_STRATEGY: scaling policy |
|RegionId|String|Required|cn-hangzhou|The region ID of the resource. You can call the [DescribeRegions](~~25609~~)API to view the latest region list. |
|ScalingGroupBizId|String|Required|SGB-A4521231C\*\*\*\*|The ID of the scaling group. You can call [ListScalingGroupV2](~~184367~~) to view the scaling group ID. |
|ResourceGroupId|String|No|rg-acfmv6jutt6\*\*\*\*|The ID of the resource group. You can call the [ListResourceGroups](~~158855~~) to view the resource group ID. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Data|String|SRB-DA382429BA82\*\*\*\*|The new configuration items ID. |
|RequestId|String|6C96FD2C-95A0-4C03-8A19-7D84A4BAAA1E|The request ID. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=AddScalingConfigItemV2
&ConfigItemInformation={ "ruleName": "Trigger by load", "adjustmentType": "QUANTITYCHANGEINCAPACITY", "coolDownTime": 100, "ruleParam": { "metricName": "YarnFreeCores", "period": 10, "statistics": "xxx", "comparisonOperator": "\u003e\u003d", "threshold": 10, "evaluationCount": 1 }, "adjustmentValue": 1, "ruleType": "BYLOAD", "configItemType": "SCALING_RULE"}
&ConfigItemType=SCALING_RULE
&RegionId=cn-hangzhou
&ScalingGroupBizId=SGB-A4521231C****
&<Common request parameters>
```

Sample success responses

`XML` format

```
<RequestId>6C96FD2C-95A0-4C03-8A19-7D84A4BAAA1E</RequestId>
<Data>SRB-DA382429BA82****</Data>
```

`JSON` format

```
{
    "RequestId":"6C96FD2C-95A0-4C03-8A19-7D84A4BAAA1E",
    "Data":"SRB-DA382429BA82****"
}
```

## Error code

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|403|Params.Illegal|The specified parameters are wrongly formed..|The error message returned because the format of the specified parameters is invalid.|
|500|InternalError|The request processing has failed due to some unknown error.|The error message returned because the request processing has failed due to an internal error. Submit a ticket.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/Emr).

