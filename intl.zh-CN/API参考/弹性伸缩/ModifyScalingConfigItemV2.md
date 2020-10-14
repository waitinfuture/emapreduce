# ModifyScalingConfigItemV2

调用ModifyScalingConfigItemV2接口，修改伸缩配置项。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=ModifyScalingConfigItemV2&type=RPC&version=2016-04-08)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ModifyScalingConfigItemV2|系统规定参数。对于您自行拼凑HTTP或HTTPS URL发起的API请求，该参数为必选参数。取值：ModifyScalingConfigItemV2。 |
|ConfigItemBizId|String|是|SCB-232423A\*\*\*\*|伸缩配置项ID。你可以调用[ListScalingConfigItemV2](~~184368~~)查看伸缩配置项ID。 |
|ConfigItemInformation|String|是|\{ "ruleName": "按负载触发", "adjustmentType": "QUANTITY\_CHANGE\_IN\_CAPACITY", "coolDownTime": 100, "ruleParam": \{ "metricName": "YarnFreeCores", "period": 10, "statistics": "xxx", "comparisonOperator": "\\u003e\\u003d", "threshold": 10, "evaluationCount": 1 \}, "adjustmentValue": 1, "ruleType": "BY\_LOAD", "configItemType": "SCALING\_RULE" \}|配置项值JSON格式不同类型不一样。取值如下：

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
|ScalingGroupBizId|String|是|SGB-1232423AB\*\*\*\*|伸缩组ID。你可以调用[ListScalingGroupV2](~~184367~~)查看伸缩组ID。 |
|ResourceGroupId|String|否|rg-acfmv6jutt6\*\*\*\*|资源组ID。你可以调用[ListResourceGroups](~~158855~~)查看资源组ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Data|Boolean|true|返回结果。 取值如下：

 -   true：成功
-   false：失败 |
|RequestId|String|7B804810-3C11-4927-B116-1D1A98A60BDD|请求ID。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=ModifyScalingConfigItemV2
&ConfigItemBizId=SCB-232423A****
&ConfigItemInformation=
&ConfigItemType=SCALING_RULE
&RegionId=cn-hangzhou
&ScalingGroupBizId=SGB-1232423AB****
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<RequestId>7B804810-3C11-4927-B116-1D1A98A60BDD</RequestId>
<Data>true</Data>
```

`JSON` 格式

```
{
    "RequestId":"7B804810-3C11-4927-B116-1D1A98A60BDD",
    "Data":"true"
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|403|Params.Illegal|The specified parameters are wrongly formed..|指定参数格式错误|
|500|InternalError|The request processing has failed due to some unknown error.|内部错误，请提工单|

访问[错误中心](https://error-center.alibabacloud.com/status/product/Emr)查看更多错误码。

