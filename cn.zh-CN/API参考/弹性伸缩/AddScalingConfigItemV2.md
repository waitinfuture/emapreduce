# AddScalingConfigItemV2

调用AddScalingConfigItemV2接口，新建弹性伸缩配置项。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=AddScalingConfigItemV2&type=RPC&version=2016-04-08)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|AddScalingConfigItemV2|系统规定参数。对于您自行拼凑HTTP或HTTPS URL发起的API请求，该参数为必选参数。取值：AddScalingConfigItemV2。 |
|ConfigItemInformation|String|是|\{ "ruleName": "按负载触发", "adjustmentType": "QUANTITYCHANGEINCAPACITY", "coolDownTime": 100, "ruleParam": \{ "metricName": "YarnFreeCores", "period": 10, "statistics": "xxx", "comparisonOperator": "\\u003e\\u003d", "threshold": 10, "evaluationCount": 1 \}, "adjustmentValue": 1, "ruleType": "BYLOAD", "configItemType": "SCALING\_RULE" \}|配置项值JSON格式不同类型不一样。取值如下：

 • SCALING\_RULE：

 -   按负载触发

 `{ "ruleName": "按负载触发", "adjustmentType": "QUANTITY_CHANGE_IN_CAPACITY", "coolDownTime": 100, "ruleParam": { "metricName": "YarnFreeCores", "period": 10, "statistics": "xxx", "comparisonOperator": "\u003e\u003d", "threshold": 10, "evaluationCount": 1 }, "adjustmentValue": 1, "ruleType": "BY_LOAD", "configItemType": "SCALING_RULE" }` -   定时调度

 `{ "ruleName": "定时调度", "adjustmentType": "QUANTITY_CHANGE_IN_CAPACITY", "coolDownTime": 100, "ruleParam": { "recurrenceType": "Daily", "recurrenceValue": "1", "recurrenceEndTime": "2020-07-22T03:01Z", "launchTime": "2020-07-22T03:07Z", "launchExpirationTime": 0 }, "adjustmentValue": 1, "ruleType": "SCHEDULED", "configItemType": "SCALING_RULE" }` -   定点执行

 `{ "ruleName": "定点执行", "adjustmentType": "QUANTITY_CHANGE_IN_CAPACITY", "coolDownTime": 100, "ruleParam": { "launchTime": "2020-07-22T03:09Z", "launchExpirationTime": 1 }, "adjustmentValue": 1, "ruleType": "BY_TIME_ONCE", "configItemType": "SCALING_RULE" }` • SCALING\_STRATEGY：

 `{ "spotStrategy": "NoSpot", "spotPriceLimits": 0.01, "instanceTypeList": [], "sysDiskCategory": "cloud_essd", "sysDiskSize": { "value": 40.0, "unit": "GIGABYTE" }, "dataDiskCategory": "cloud_essd", "dataDiskSize": { "value": 40.0, "unit": "GIGABYTE" }, "dataDiskCount": 4, "scalingMaxSize": 1, "scalingMinSize": 1, "defaultCoolDownTime": 0, "scalingTimeoutPolice": { "timeoutPolicy": "ROLLBACK" }, "nodeOfflineMode": "NORMAL", "nodeOfflineModeParam": { "timeoutMs": 0 }, "triggerMode": "Scheduled", "multiAvailablePolicy": "PRIORITY", "multiAvailablePolicyParam": { "onDemandBaseCapacity": 0, "onDemandPercentageAboveBaseCapacity": 0, "spotInstanceRemedy": 0, "spotInstance": false }, "configItemType": "SCALING_STRATEGY" }`|
|ConfigItemType|String|是|SCALING\_RULE|配置项类型。取值如下：

 -   SCALING\_RULE：伸缩规则
-   SCALING\_STRATEGY：伸缩策略 |
|RegionId|String|是|cn-hangzhou|地域ID。您可以调用[DescribeRegions](~~25609~~)接口查看最新的阿里云地域列表。 |
|ScalingGroupBizId|String|是|SGB-A4521231C\*\*\*\*|伸缩组ID。你可以调用[ListScalingGroupV2](~~184367~~)查看伸缩组ID。 |
|ResourceGroupId|String|否|rg-acfmv6jutt6\*\*\*\*|资源组ID。你可以调用[ListResourceGroups](~~158855~~)查看资源组ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Data|String|SRB-DA382429BA82\*\*\*\*|新建的配置项ID。 |
|RequestId|String|6C96FD2C-95A0-4C03-8A19-7D84A4BAAA1E|请求ID。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=AddScalingConfigItemV2
&ConfigItemInformation={ "ruleName": "按负载触发", "adjustmentType": "QUANTITYCHANGEINCAPACITY", "coolDownTime": 100, "ruleParam": { "metricName": "YarnFreeCores", "period": 10, "statistics": "xxx", "comparisonOperator": "\u003e\u003d", "threshold": 10, "evaluationCount": 1 }, "adjustmentValue": 1, "ruleType": "BYLOAD", "configItemType": "SCALING_RULE" }
&ConfigItemType=SCALING_RULE
&RegionId=cn-hangzhou
&ScalingGroupBizId=SGB-A4521231C****
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<RequestId>6C96FD2C-95A0-4C03-8A19-7D84A4BAAA1E</RequestId>
<Data>SRB-DA382429BA82****</Data>
```

`JSON` 格式

```
{
    "RequestId":"6C96FD2C-95A0-4C03-8A19-7D84A4BAAA1E",
    "Data":"SRB-DA382429BA82****"
}
```

