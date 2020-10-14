# ListScalingActivityV2

调用ListScalingActivityV2接口，查看伸缩活动列表。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=ListScalingActivityV2&type=RPC&version=2016-04-08)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ListScalingActivityV2|系统规定参数。对于您自行拼凑HTTP或HTTPS URL发起的API请求，该参数为必选参数。取值：ListScalingActivityV2。 |
|ClusterBizId|String|是|C-12324352352\*\*\*\*|集群ID。您可以调用[ListClusters](~~28147~~)接口查看集群的ID。 |
|RegionId|String|是|cn-hangzhou|地域ID。您可以调用[DescribeRegions](~~25609~~)接口查看最新的阿里云地域列表。 |
|ResourceGroupId|String|否|-1|资源组ID。你可以调用[ListResourceGroups](~~158855~~)查看资源组ID。 |
|Limit|Integer|否|0|废弃字段。 |
|PageNumber|Integer|否|1|分页页码。 |
|PageSize|Integer|否|100|单页记录条数。 |
|CurrentSize|Integer|否|0|废弃字段。 |
|PageCount|Integer|否|0|废弃字段。 |
|OrderField|String|否|id|翻页参数。 |
|OrderMode|String|否|desc|翻页参数。 |
|HostGroupId|String|否|G-2342423\*\*\*\*|机器组ID。你可以调用[ListClusterHostGroup](~~128432~~)查看机器组ID。 |
|ScalingGroupBizId|String|否|SGB-A2343453\*\*\*\*|伸缩组ID。你可以调用[ListScalingGroupV2](~~AAAA~~)查看伸缩组ID。 |
|ScalingRuleName|String|否|负载扩容-1|伸缩规则名称。 |
|HostGroupName|String|否|伸缩组1|机器组名称。 |
|Status|String|否|Successful|活动状态。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Items|Array of Item| |伸缩活动列表 |
|Item| | | |
|BizId|String|asa-bp18jfxw7gqnkhtz\*\*\*\*|伸缩活动ID。 |
|Cause|String|A user requests to execute scaling rule \\"asr-bp1d8mebzisgwu34mcn3\\", changing the Total Capacity from \\"0\\" to \\"1\\".|伸缩活动原因。 |
|Description|String|\\"1\\" ECS instances are added|描述信息。 |
|EndTime|Long|1600691996000|结束时间。 |
|ExpectNum|Integer|1|期望伸缩个数。 |
|HostGroupBizId|String|G-4A1833AA86EA\*\*\*\*|机器组ID。 |
|HostGroupName|String|伸缩组1|机器组名称。 |
|InstanceIds|String|\[\\"i-bp1ci2e4xqt9eqjqtfrr\\"\]|实例列表。 |
|ScalingRuleId|String|asr-bp19zruz0q7pru31\*\*\*\*|伸缩规则ID。 |
|ScalingRuleName|String|负载扩容-1|伸缩规则名称。 |
|StartTime|Long|1600691996000|启动时间。 |
|Status|String|Successful|活动状态。取值如下：

 -   Successful：成功
-   Failed；失败
-   Abandon：异常
-   Reject：拒绝 |
|TotalCapacity|Integer|10|伸缩组总容量。 |
|Transition|String|SCALE\_OUT|伸缩类型。取值如下：

 -   SCALE\_OUT：扩容伸缩活动。
-   SCALE\_IN：缩容伸缩活动。 |
|NextToken|String|None|废弃字段。 |
|PageNumber|Integer|1|分页页码。 |
|PageSize|Integer|100|单页记录条数。 |
|RequestId|String|123DF0CD-C5F5-4A8D-984A-C5A4331EB84F|请求ID。 |
|TotalCount|Integer|100|伸缩活动记录总数。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=ListScalingActivityV2
&ClusterBizId=C-12324352352****
&RegionId=cn-hangzhou
&<公共请求参数>
```

正常返回示例

`XML` 格式

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
        <HostGroupName>伸缩组1</HostGroupName>
        <ScalingRuleName>负载扩容-1</ScalingRuleName>
        <HostGroupBizId>G-4A1833AA86EA****</HostGroupBizId>
        <ExpectNum>1</ExpectNum>
        <Cause>A user requests to execute scaling rule \"asr-bp1d8mebzisgwu34mcn3\", changing the Total Capacity from \"0\" to \"1\".</Cause>
        <Transition>SCALE_OUT</Transition>
        <TotalCapacity>10</TotalCapacity>
        <InstanceIds>[\"i-bp1ci2e4xqt9eqjqtfrr\"]</InstanceIds>
        <BizId>asa-bp18jfxw7gqnkhtz****</BizId>
    </Item>
</Items>
```

`JSON` 格式

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
                "HostGroupName":"伸缩组1",
                "ScalingRuleName":"负载扩容-1",
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

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|403|Params.Illegal|The specified parameters are wrongly formed..|指定参数格式错误|
|500|InternalError|The request processing has failed due to some unknown error.|内部错误，请提工单|

访问[错误中心](https://error-center.alibabacloud.com/status/product/Emr)查看更多错误码。

