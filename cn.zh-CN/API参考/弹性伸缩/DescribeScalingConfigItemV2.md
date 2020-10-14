# DescribeScalingConfigItemV2

调用DescribeScalingConfigItemV2接口，获取伸缩配置项详情。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=DescribeScalingConfigItemV2&type=RPC&version=2016-04-08)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DescribeScalingConfigItemV2|系统规定参数。对于您自行拼凑HTTP或HTTPS URL发起的API请求，该参数为必选参数。取值：DescribeScalingConfigItemV2。 |
|ConfigItemType|String|是|SCALING\_RULE|配置项类型。取值如下：

 -   SCALING\_RULE：伸缩规则
-   SCALING\_STRATEGY：伸缩策略 |
|RegionId|String|是|cn-hangzhou|地域ID。您可以调用[DescribeRegions](~~25609~~)接口查看最新的阿里云地域列表。 |
|ScalingConfigItemId|String|是|SRB-0F2A154CFD4D\*\*\*\*|伸缩配置项ID。你可以调用[ListScalingConfigItemV2](~~184368~~)查看伸缩配置项ID。 |
|ScalingGroupBizId|String|是|SGB-8E52011DD26C\*\*\*\*|伸缩组ID。你可以调用[ListScalingGroupV2](~~AAAA~~)查看伸缩组ID。 |
|ResourceGroupId|String|否|-1|资源组ID。你可以调用[ListResourceGroups](~~158855~~)查看资源组ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|ConfigItemInformation|String|\{ "ruleName": "按负载触发", "adjustmentType": "QUANTITY\_CHANGE\_IN\_CAPACITY", "coolDownTime": 100, "ruleParam": \{ "metricName": "YarnFreeCores", "period": 10, "statistics": "xxx", "comparisonOperator": "\\u003e\\u003d", "threshold": 10, "evaluationCount": 1 \}, "adjustmentValue": 1, "ruleType": "BY\_LOAD", "configItemType": "SCALING\_RULE" \}|配置项信息。配置项类型不同信息不同。取值如下：

 -   SCALING\_RULE：
    -   按负载触发`{ "ruleName": "按负载触发", "adjustmentType": "QUANTITY_CHANGE_IN_CAPACITY", "coolDownTime": 100, "ruleParam": { "metricName": "YarnFreeCores", "period": 10, "statistics": "xxx", "comparisonOperator": "\u003e\u003d", "threshold": 10, "evaluationCount": 1 }, "adjustmentValue": 1, "ruleType": "BY_LOAD", "configItemType": "SCALING_RULE" }`
    -   定时调度`{ "ruleName": "定时调度", "adjustmentType": "QUANTITY_CHANGE_IN_CAPACITY", "coolDownTime": 100, "ruleParam": { "recurrenceType": "Daily", "recurrenceValue": "1", "recurrenceEndTime": "2020-07-22T03:01Z", "launchTime": "2020-07-22T03:07Z", "launchExpirationTime": 0 }, "adjustmentValue": 1, "ruleType": "SCHEDULED", "configItemType": "SCALING_RULE" }`
    -   定点执行`{ "ruleName": "定点执行", "adjustmentType": "QUANTITY_CHANGE_IN_CAPACITY", "coolDownTime": 100, "ruleParam": { "launchTime": "2020-07-22T03:09Z", "launchExpirationTime": 1 }, "adjustmentValue": 1, "ruleType": "BY_TIME_ONCE", "configItemType": "SCALING_RULE" }`
-   SCALING\_STRATEGY：`{ "spotStrategy": "NoSpot", "spotPriceLimits": 0.01, "instanceTypeList":[],"sysDiskCategory": "cloud_essd", "sysDiskSize": { "value": 40.0, "unit": "GIGABYTE" }, "dataDiskCategory": "cloud_essd", "dataDiskSize": { "value": 40.0, "unit": "GIGABYTE" }, "dataDiskCount": 4, "scalingMaxSize": 1, "scalingMinSize": 1, "defaultCoolDownTime": 0, "scalingTimeoutPolice": { "timeoutPolicy": "ROLLBACK" }, "nodeOfflineMode": "NORMAL", "nodeOfflineModeParam": { "timeoutMs": 0 }, "triggerMode": "Scheduled", "multiAvailablePolicy": "PRIORITY", "multiAvailablePolicyParam": { "onDemandBaseCapacity": 0, "onDemandPercentageAboveBaseCapacity": 0, "spotInstanceRemedy": 0, "spotInstance": false }, "configItemType": "SCALING_STRATEGY" }` |
|ConfigItemType|String|SCALING\_RULE|配置项类型。取值如下：

 -   SCALING\_RUL：伸缩规则
-   SCALING\_STRATEGY：伸缩策略 |
|RequestId|String|6C96FD2C-95A0-4C03-8A19-7D84A4BAAA1E|请求ID。 |
|ScalingConfigItemBizId|String|SRB-0F2A154CFD4D\*\*\*\*|伸缩配置项ID。 |
|ScalingGroupBizId|String|SGB-0F2A154CFD4D\*\*\*\*|伸缩组ID。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=DescribeScalingConfigItemV2
&ConfigItemType=SCALING_RULE
&RegionId=cn-hangzhou
&ScalingConfigItemId=SCB-8E52011DD26123****
&ScalingGroupBizId=SGB-8E52011DD26C****
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<RequestId>6C96FD2C-95A0-4C03-8A19-7D84A4BAAA1E</RequestId>
<ScalingConfigItemBizId>SRB-0F2A154CFD4D****</ScalingConfigItemBizId>
<ScalingGroupBizId>SGB-0F2A154CFD4D****</ScalingGroupBizId>
<ConfigItemInformation>{   "ruleName": "按负载触发",   "adjustmentType": "QUANTITY_CHANGE_IN_CAPACITY",   "coolDownTime": 100,   "ruleParam": {     "metricName": "YarnFreeCores",     "period": 10,     "statistics": "xxx",     "comparisonOperator": "\u003e\u003d",     "threshold": 10,     "evaluationCount": 1   },   "adjustmentValue": 1,   "ruleType": "BY_LOAD",   "configItemType": "SCALING_RULE" }</ConfigItemInformation>
<ConfigItemType>SCALING_RULE</ConfigItemType>
```

`JSON` 格式

```
{
    "RequestId":"6C96FD2C-95A0-4C03-8A19-7D84A4BAAA1E",
    "ScalingConfigItemBizId":"SRB-0F2A154CFD4D****",
    "ScalingGroupBizId":"SGB-0F2A154CFD4D****",
    "ConfigItemInformation":"{   \"ruleName\": \"按负载触发\",   \"adjustmentType\": \"QUANTITY_CHANGE_IN_CAPACITY\",   \"coolDownTime\": 100,   \"ruleParam\": {     \"metricName\": \"YarnFreeCores\",     \"period\": 10,     \"statistics\": \"xxx\",     \"comparisonOperator\": \"\\u003e\\u003d\",     \"threshold\": 10,     \"evaluationCount\": 1   },   \"adjustmentValue\": 1,   \"ruleType\": \"BY_LOAD\",   \"configItemType\": \"SCALING_RULE\" }",
    "ConfigItemType":"SCALING_RULE"
}
```

