# ListScalingGroupV2

调用ListScalingGroupV2接口，查看伸缩组列表。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=ListScalingGroupV2&type=RPC&version=2016-04-08)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ListScalingGroupV2|系统规定参数。对于您自行拼凑HTTP或HTTPS URL发起的API请求，该参数为必选参数。取值：ListScalingGroupV2。 |
|RegionId|String|是|cn-hangzhou|地域ID。您可以调用[DescribeRegions](~~25609~~)接口查看最新的阿里云地域列表。 |
|ResourceGroupId|String|否|rg-bp67acfmxazb4p\*\*\*\*|资源组ID。你可以调用[ListResourceGroups](~~158855~~)查看资源组ID。 |
|Limit|Integer|否|0|废弃字段。 |
|PageNumber|Integer|否|1|分页页数。 |
|PageSize|Integer|否|100|单页记录数。 |
|CurrentSize|Integer|否|0|废弃字段。 |
|PageCount|Integer|否|0|废弃字段。 |
|OrderField|String|否|id|废弃字段。 |
|OrderMode|String|否|desc|废弃字段。 |
|ClusterBizId|String|否|C-ABC12323\*\*\*\*|集群ID。您可以调用[ListClusters](~~28147~~)接口查看集群的ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Items|Array of Item| |伸缩组列表 |
|Item| | | |
|ActiveStatus|String|ACTIVE|伸缩组活动状态。取值如下：

 -   ACTIVE：伸缩组已启动
-   INACTIVE：伸缩组未启动 |
|Description|String|这是一个示例伸缩组|伸缩组描述。 |
|HostGroupBizId|String|G-3242ABC24\*\*\*\*|机器组ID。你可以调用[ListClusterHostGroup](~~128432~~)查看机器组ID。 |
|Name|String|test|伸缩组名称。 |
|ScalingGroupId|String|SGB-242342ABC\*\*\*\*|伸缩组ID。你可以调用[ListScalingGroupV2](~~184367~~)查看伸缩组ID。 |
|ScalingInMode|String|GRACEFUL|伸缩模式。取值如下：

 -   GRACEFUL：优雅下线
-   DEFAULT：默认 |
|ScalingMaxSize|Integer|10|伸缩组最大节点数。最大值为500。 |
|ScalingMinSize|Integer|0|伸缩组最小节点数。最小值为0。 |
|NextToken|String|None|废弃字段。 |
|PageNumber|Integer|1|分页页数。 |
|PageSize|Integer|100|单页记录数。 |
|RequestId|String|123DF0CD-C5F5-4A8D-984A-C5A4331EB84F|请求ID。 |
|TotalCount|Integer|1|记录总数。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=ListScalingGroupV2
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
        <Description>这是一个示例伸缩组</Description>
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

`JSON` 格式

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
                "Description":"这是一个示例伸缩组",
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

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|403|Params.Illegal|The specified parameters are wrongly formed..|指定参数格式错误|
|500|InternalError|The request processing has failed due to some unknown error.|内部错误，请提工单|

访问[错误中心](https://error-center.alibabacloud.com/status/product/Emr)查看更多错误码。

