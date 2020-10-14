# ListScalingConfigItemV2

调用ListScalingConfigItemV2接口，查看伸缩配置项列表。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=ListScalingConfigItemV2&type=RPC&version=2016-04-08)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ListScalingConfigItemV2|系统规定参数。对于您自行拼凑HTTP或HTTPS URL发起的API请求，该参数为必选参数。取值：ListScalingConfigItemV2。 |
|ConfigItemType|String|是|SCALING\_RULE|配置项类型。取值如下：

 -   SCALING\_RULE：伸缩规则
-   SCALING\_STRATEGY：伸缩策略 |
|RegionId|String|是|cn-hangzhou|地域ID。您可以调用[DescribeRegions](~~25609~~)接口查看最新的阿里云地域列表。 |
|ScalingGroupBizId|String|是|SGB-16ABA17988F39\*\*\*\*|伸缩组ID。你可以调用[ListScalingGroupV2](~~AAAA~~)查看伸缩组ID。 |
|ResourceGroupId|String|否|rg-acfmv6jutt6\*\*\*\*|资源组ID。你可以调用[ListResourceGroups](~~158855~~)查看资源组ID。 |
|Limit|Integer|否|0|废弃字段。 |
|PageNumber|Integer|否|1|分页页码。 |
|PageSize|Integer|否|100|单页记录条数。 |
|CurrentSize|Integer|否|0|废弃字段。 |
|PageCount|Integer|否|0|废弃字段。 |
|OrderField|String|否|id|废弃字段。 |
|OrderMode|String|否|desc|废弃字段。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Items|Array of Item| |配置项列表 |
|Item| | | |
|ConfigItemInformation|String|\{ "ruleName": "按负载触发", "adjustmentType": "QUANTITY\_CHANGE\_IN\_CAPACITY", "coolDownTime": 100, "ruleParam": \{ "metricName": "YarnFreeCores", "period": 10, "statistics": "xxx", "comparisonOperator": "\\u003e\\u003d", "threshold": 10, "evaluationCount": 1 \}, "adjustmentValue": 1, "ruleType": "BY\_LOAD", "configItemType": "SCALING\_RULE" \}|配置项信息。配置项类型不同信息不同。取值如下：

 -   SCALING\_RULE：
    -   按负载触发`{ "ruleName": "按负载触发", "adjustmentType": "QUANTITY_CHANGE_IN_CAPACITY", "coolDownTime": 100, "ruleParam": { "metricName": "YarnFreeCores", "period": 10, "statistics": "xxx", "comparisonOperator": "\u003e\u003d", "threshold": 10, "evaluationCount": 1 }, "adjustmentValue": 1, "ruleType": "BY_LOAD", "configItemType": "SCALING_RULE" }`
    -   定时调度`{ "ruleName": "定时调度", "adjustmentType": "QUANTITY_CHANGE_IN_CAPACITY", "coolDownTime": 100, "ruleParam": { "recurrenceType": "Daily", "recurrenceValue": "1", "recurrenceEndTime": "2020-07-22T03:01Z", "launchTime": "2020-07-22T03:07Z", "launchExpirationTime": 0 }, "adjustmentValue": 1, "ruleType": "SCHEDULED", "configItemType": "SCALING_RULE" }`
    -   定点执行`{ "ruleName": "定点执行", "adjustmentType": "QUANTITY_CHANGE_IN_CAPACITY", "coolDownTime": 100, "ruleParam": { "launchTime": "2020-07-22T03:09Z", "launchExpirationTime": 1 }, "adjustmentValue": 1, "ruleType": "BY_TIME_ONCE", "configItemType": "SCALING_RULE" }`
-   SCALING\_STRATEGY：`{ "spotStrategy": "NoSpot", "spotPriceLimits": 0.01, "instanceTypeList":[],"sysDiskCategory": "cloud_essd", "sysDiskSize": { "value": 40.0, "unit": "GIGABYTE" }, "dataDiskCategory": "cloud_essd", "dataDiskSize": { "value": 40.0, "unit": "GIGABYTE" }, "dataDiskCount": 4, "scalingMaxSize": 1, "scalingMinSize": 1, "defaultCoolDownTime": 0, "scalingTimeoutPolice": { "timeoutPolicy": "ROLLBACK" }, "nodeOfflineMode": "NORMAL", "nodeOfflineModeParam": { "timeoutMs": 0 }, "triggerMode": "Scheduled", "multiAvailablePolicy": "PRIORITY", "multiAvailablePolicyParam": { "onDemandBaseCapacity": 0, "onDemandPercentageAboveBaseCapacity": 0, "spotInstanceRemedy": 0, "spotInstance": false }, "configItemType": "SCALING_STRATEGY" }` |
|ConfigItemType|String|SCALING\_RULE|配置项类型。 |
|ScalingConfigItemBizId|String|SRB-54CCB030511A\*\*\*\*|配置项ID。 |
|ScalingGroupBizId|String|SGB-CA3A16B87BF1\*\*\*\*|伸缩组ID。 |
|NextToken|String|None|废弃字段。 |
|PageNumber|Integer|1|分页页码。 |
|PageSize|Integer|100|单页记录条数。 |
|RequestId|String|7B804810-3C11-4927-B116-1D1A98A60BDD|请求ID。 |
|TotalCount|Integer|100|记录总数。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=ListScalingConfigItemV2
&ConfigItemType=SCALING_RULE
&RegionId=cn-hangzhou
&ScalingGroupBizId=SGB-16ABA17988F3****
&<公共请求参数>
```

正常返回示例

`XML` 格式

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
        <ConfigItemInformation>{"ruleName":"按负载触发","ruleType":"BY_LOAD","ruleParam":{"metricName":"YarnFreeCores","statistics":"xxx","comparisonOperator":"\u003e\u003d","period":10,"threshold":10,"evaluationCount":1},"adjustmentType":"QUANTITY_CHANGE_IN_CAPACITY","adjustmentValue":1,"coolDownTime":100,"configItemType":"SCALING_RULE","scalingConfigBizId":"SCB-3580B6EA94A1****"}</ConfigItemInformation>
        <ConfigItemType>SCALING_RULE</ConfigItemType>
    </Item>
    <Item>
        <ScalingConfigItemBizId>SRB-DA382429BA82****</ScalingConfigItemBizId>
        <ScalingGroupBizId>SGB-CA3A16B87BF1****</ScalingGroupBizId>
        <ConfigItemInformation>{"ruleName":"按负载触发","ruleType":"BY_LOAD","ruleParam":{"metricName":"YarnFreeCores","statistics":"xxx","comparisonOperator":"\u003e\u003d","period":10,"threshold":10,"evaluationCount":1},"adjustmentType":"QUANTITY_CHANGE_IN_CAPACITY","adjustmentValue":1,"coolDownTime":100,"configItemType":"SCALING_RULE","scalingConfigBizId":"SCB-3580B6EA94A1****"}</ConfigItemInformation>
        <ConfigItemType>SCALING_RULE</ConfigItemType>
    </Item>
</Items>
```

`JSON` 格式

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
				"ConfigItemInformation": "{\"ruleName\":\"按负载触发\",\"ruleType\":\"BY_LOAD\",\"ruleParam\":{\"metricName\":\"YarnFreeCores\",\"statistics\":\"xxx\",\"comparisonOperator\":\"\\u003e\\u003d\",\"period\":10,\"threshold\":10,\"evaluationCount\":1},\"adjustmentType\":\"QUANTITY_CHANGE_IN_CAPACITY\",\"adjustmentValue\":1,\"coolDownTime\":100,\"configItemType\":\"SCALING_RULE\",\"scalingConfigBizId\":\"SCB-3580B6EA94A1****\"}",
				"ConfigItemType": "SCALING_RULE"
			},
			{
				"ScalingConfigItemBizId": "SRB-DA382429BA82****",
				"ScalingGroupBizId": "SGB-CA3A16B87BF1****",
				"ConfigItemInformation": "{\"ruleName\":\"按负载触发\",\"ruleType\":\"BY_LOAD\",\"ruleParam\":{\"metricName\":\"YarnFreeCores\",\"statistics\":\"xxx\",\"comparisonOperator\":\"\\u003e\\u003d\",\"period\":10,\"threshold\":10,\"evaluationCount\":1},\"adjustmentType\":\"QUANTITY_CHANGE_IN_CAPACITY\",\"adjustmentValue\":1,\"coolDownTime\":100,\"configItemType\":\"SCALING_RULE\",\"scalingConfigBizId\":\"SCB-3580B6EA94A1****\"}",
				"ConfigItemType": "SCALING_RULE"
			}
		]
     }
}
```

