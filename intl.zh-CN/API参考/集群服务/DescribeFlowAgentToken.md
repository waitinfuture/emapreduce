# DescribeFlowAgentToken

调用DescribeFlowAgentToken，获取FlowAgent的Token。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=DescribeFlowAgentToken&type=RPC&version=2016-04-08)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DescribeFlowAgentToken|系统规定参数。对于您自行拼凑HTTP或HTTPS URL发起的API请求，该参数为必选参数。取值：DescribeFlowAgentToken。 |
|ClusterBizId|String|是|C-3179ACD5B\*\*\*\*|集群ID。您可以调用[ListClusters](~~28147~~)查看集群的ID。 |
|InnerIP|String|是|192.168.\*\*.\*\*|集群内网IP地址。您可以调用[DescribeClusterV2](~~28144~~)查看。 |
|RegionId|String|是|cn-hangzhou|地域ID。您可以调用[DescribeRegions](~~25609~~)查看最新的阿里云地域列表。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Data|String|B5E1C2BA6F4469DDE810AD44027428D196313BCDE6D14E458F73151C314E\*\*\*\*|返回结果。

 FlowAgent的最新Token。 |
|RequestId|String|7B804810-3C11-4927-B116-1D1A98A60BDD|请求ID。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=DescribeFlowAgentToken
&clusterBizId=C-3179ACD5B****
&innerIP=192.168.**.**
&RegionId=cn-hangzhou
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<RequestId>7B804810-3C11-4927-B116-1D1A98A60BDD</RequestId>
<Data>B5E1C2BA6F4469DDE810AD44027428D196313BCDE6D14E458F73151C314E****</Data>
```

`JSON` 格式

```
{
    "RequestId":"7B804810-3C11-4927-B116-1D1A98A60BDD",
    "Data":"B5E1C2BA6F4469DDE810AD44027428D196313BCDE6D14E458F73151C314E****"
    }
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|403|Params.Illegal|The specified parameters are wrongly formed..|指定参数格式错误|
|500|InternalError|The request processing has failed due to some unknown error.|内部错误，请提工单|

访问[错误中心](https://error-center.alibabacloud.com/status/product/Emr)查看更多错误码。

