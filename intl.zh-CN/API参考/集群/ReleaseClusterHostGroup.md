# ReleaseClusterHostGroup

调用ReleaseClusterHostGroup接口进行EMR集群节点缩容。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=ReleaseClusterHostGroup&type=RPC&version=2016-04-08)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ReleaseClusterHostGroup|系统规定参数。对于您自行拼凑HTTP或HTTPS URL发起的API请求，该参数为必选参数。取值：ReleaseClusterHostGroup。 |
|ClusterId|String|是|C-D7958B72E59B\*\*\*\*|集群ID。 |
|HostGroupId|String|是|G-EF460256A55F\*\*\*\*|机器组ID，可以通过ListClusterHostGroup获取。 |
|RegionId|String|是|cn-hangzhou|地域ID。 |
|InstanceIdList|String|否|\["i-bp1bm7y86rscdx1a\*\*\*\*"\]|ECS实例ID列表。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|991B3409-6C8D-48CB-903C-3B9C166E17A8|请求ID。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=ReleaseClusterHostGroup
&ClusterId=C-D7958B72E59B****
&HostGroupId=G-EF460256A55F****
&RegionId=cn-hangzhou
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<RequestId>991B3409-6C8D-48CB-903C-3B9C166E17A8</RequestId>
```

`JSON` 格式

```
{
    "RequestId":"991B3409-6C8D-48CB-903C-3B9C166E17A8"
    }
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|403|Params.Illegal|The specified parameters are wrongly formed..|指定参数格式错误|
|500|InternalError|The request processing has failed due to some unknown error.|内部错误，请提工单|
|404|ClusterId.NotFound|The ClusterId provided does not exist in our records.|Cluster Id不存在，确认集群的Cluster Id|
|500|InternalError|The request processing has failed due to some unknown error\(code:5100\)|内部错误，请提工单|
|403|User.OtherUserResource.NotAllow|It is not allowed to operate other user's resource|不能操作其它用户的资源|
|403|User.Account.Abnormal|The User Account maybe is out of service!|用户帐号已经停止服务|
|403|Cluster.Role.NotConfirm|The Role for EMR service to operate is Not confirmed!|EMR的RAM角色没有授权，在RAM中授权|
|404|ClusterId.NotFound|ClusterId \[%s\] does not exist.|Cluster Id不存在，确认集群的Cluster Id|
|400|Prepaid.Cluster.Cannot.Release|Prepaid cluster do not support release operation!|包年包月集群不支持释放操作|
|400|Cluster.Cannot.Release|Cluster has been release or did not start normally!|集群释放失败，请选择正确的集群|
|400|Cluster.RegionId.NotMatch|Cluster does not exist in region \[%s\]!|集群不属于该Region，切换到正确的Region|

访问[错误中心](https://error-center.alibabacloud.com/status/product/Emr)查看更多错误码。

