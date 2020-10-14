# DescribeScalingGroupV2

调用DescribeScalingGroupV2接口，获取伸缩组详情。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=DescribeScalingGroupV2&type=RPC&version=2016-04-08)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DescribeScalingGroupV2|系统规定参数。对于您自行拼凑HTTP或HTTPS URL发起的API请求，该参数为必选参数。取值：DescribeScalingGroupV2。 |
|RegionId|String|是|cn-hangzhou|地域ID。您可以调用[DescribeRegions](~~25609~~)查看最新的阿里云地域列表。 |
|ResourceGroupId|String|否|rg-acfmv6jutt6\*\*\*\*|资源组ID。你可以调用[ListResourceGroups](~~158855~~)查看资源组ID。 |
|ScalingGroupBizId|String|否|SGB-232432123245\*\*\*\*|伸缩组ID。你可以调用[ListScalingGroupV2](~~184367~~)查看伸缩组ID。 |
|HostGroupBizId|String|否|G-AB342535346545\*\*\*\*|机器组ID。你可以调用[ListClusterHostGroup](~~128432~~)查看机器组ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|ActiveStatus|String|ACTIVE|伸缩组活动状态。取值如下：

 -   ACTIVE：伸缩组已启动
-   INACTIVE：伸缩组未启动 |
|ConfigState|String|APPLIED|伸缩配置生效状态。取值如下：

 -   APPLIED：已生效
-   MODIFIED：已修改，未生效 |
|Description|String|这是一个示例|伸缩组描述信息。 |
|HostGroupBizId|String|G-5011FB3E4928\*\*\*\*|伸缩组关联的机器组。 |
|Name|String|示例伸缩组|伸缩组名称。 |
|RequestId|String|C390A685-2707-4F42-BCFA-E4BC40E4B7A3|请求ID。 |
|ScalingGroupId|String|SGB-16ABA17988F3\*\*\*\*|伸缩组ID。 |
|ScalingInMode|String|GRACEFUL|伸缩类型。取值如下：

 -   NORMAL：常规伸缩
-   GRACEFUL：优雅下线 |
|ScalingMaxSize|Integer|10|伸缩组最大节点个数。 |
|ScalingMinSize|Integer|1|伸缩组最小节点个数。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=DescribeScalingGroupV2
&RegionId=cn-hangzhou
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<Description>这是一个示例</Description>
<HostGroupBizId>G-5011FB3E4928****</HostGroupBizId>
<RequestId>C390A685-2707-4F42-BCFA-E4BC40E4B7A3</RequestId>
<ConfigState>APPLIED</ConfigState>
<ScalingInMode>GRACEFUL</ScalingInMode>
<ScalingGroupId>SGB-16ABA17988F39****</ScalingGroupId>
<ScalingMaxSize>10</ScalingMaxSize>
<ScalingMinSize>1</ScalingMinSize>
<Name>示例伸缩组</Name>
<ActiveStatus>ACTIVE</ActiveStatus>
```

`JSON` 格式

```
{
    "Description":"这是一个示例",
    "HostGroupBizId":"G-5011FB3E4928****",
    "RequestId":"C390A685-2707-4F42-BCFA-E4BC40E4B7A3",
    "ConfigState":"APPLIED",
    "ScalingInMode":"GRACEFUL",
    "ScalingGroupId":"SGB-16ABA17988F39****",
    "ScalingMaxSize":"10",
    "ScalingMinSize":"1",
    "Name":"示例伸缩组",
    "ActiveStatus":"ACTIVE"
    }
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|403|Params.Illegal|The specified parameters are wrongly formed..|指定参数格式错误|
|500|InternalError|The request processing has failed due to some unknown error.|内部错误，请提工单|

访问[错误中心](https://error-center.alibabacloud.com/status/product/Emr)查看更多错误码。

