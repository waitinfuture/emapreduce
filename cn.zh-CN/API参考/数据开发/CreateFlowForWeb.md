# CreateFlowForWeb

调用CreateFlowForWeb接口，创建工作流。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=CreateFlowForWeb&type=RPC&version=2016-04-08)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|CreateFlowForWeb|系统规定参数。对于您自行拼凑HTTP或HTTPS URL发起的API请求，该参数为必选参数。取值：CreateFlowForWeb。 |
|ClusterId|String|是|C-A23BD131A862\*\*\*\*|集群ID。您可以调用[ListClusters](~~28147~~)查看集群的ID。 |
|CreateCluster|Boolean|是|false|是否通过集群模板创建集群，ture表示通过集群模板创建集群，ClusterId应设置为集群模板 ID\(CT-xxx\)，否则为已有集群 ID（C-xxx）。 |
|Description|String|是|这是一个创建工作流描述|工作流描述。 |
|Name|String|是|my\_flow\_demo|工作流名称。 |
|ProjectId|String|是|FP-257A173659F5\*\*\*\*|项目ID。你可以调用[ListFlowProject](~~101204~~)查看项目的ID。 |
|RegionId|String|是|cn-hangzhou|地域ID。您可以调用[DescribeRegions](~~25609~~)查看最新的阿里云地域列表。 |
|StartSchedule|Long|否|1538017814000|开始调度时间，长整型时间戳.。例如：System.currentTimeMillis\(\)。

 -   必须小于EndSchedule。
-   当CronExpr不为空时，此项必填。 |
|EndSchedule|Long|否|1538018814000|调度失效时间，长整型时间戳，例如System.currentTimeMillis\(\)。 |
|CronExpr|String|否|0 0 0-23/1 \* \* ?|时间周期调度的cron表达式。 |
|HostName|String|否|emr-header-1.cluster-123456|节点实例运行所在主机的名称。您可以调用[ListFlow](~~101073~~)或登录主机使用`hostname`命令查看。 |
|Graph|String|否|\{"nodes":\[\{"id":"48d474ea","index":0,"spmAnchorId":"0.0.0.i0.766645eb2cmNtQ","attribute":\{"type":"START"\},"shape":"startControlNode","type":"node","y":250,"size":"80\*34","x":500\},\{"id":"7ba480b3","index":1,"spmAnchorId":"5176.8250060.0.i19.771e28d0IPNQGE","attribute":\{"jobType":"SHELL","jobId":"FJ-7BE1062897B19D25","type":"JOB"\},"config":\{"hostName":""\},"label":"fail\_job","shape":"shellJobNode","type":"node","y":398.5,"size":"170\*34","x":470.5\},\{"id":"33202d60","index":2,"spmAnchorId":"5176.8250060.0.i23.771e28d0IPNQGE","attribute":\{"type":"END"\},"shape":"endControlNode","type":"node","y":562.5,"size":"80\*34","x":430.5\}\],"edges":\[\{"id":"28167ea0","index":3,"source":"48d474ea","sourceAnchor":0,"target":"7ba480b3","targetAnchor":0\},\{"id":"e8d5ff52","index":4,"source":"7ba480b3","sourceAnchor":1,"target":"33202d60","targetAnchor":0\}\]\}|DAG图形信息。 |
|AlertConf|String|否|\{"items":\[\{"enable":true,"eventId":"EMR-210401001","alertUserGroupIdList":\["AUG-b79bb29bb6e14ddd89674a242623851b"\],"alertDingDingGroupList":\["ADG-af1f9689d6194e2dbd89927d5c515172"\]\},\{"enable":true,"eventId":"EMR-110401015","alertUserGroupIdList":\["AUG-b79bb29bb6e14ddd89674a242623851b"\],"alertDingDingGroupList":\["ADG-af1f9689d6194e2dbd89927d5c515172"\]\},\{"enable":true,"eventId":"EMR-110401002","alertUserGroupIdList":\["AUG-b79bb29bb6e14ddd89674a242623851b"\],"alertDingDingGroupList":\["ADG-af1f9689d6194e2dbd89927d5c515172"\]\}\]\}|报警通知配置，eventId目前支持以下告警：

 -   EMR-210401001（工作流失败报警）。
-   EMR-110401002（工作流成功通知）。
-   EMR-110401015（工作流节点失败报警）。 |
|AlertUserGroupBizId|String|否|已过期|报警用户组信息。 |
|AlertDingDingGroupBizId|String|否|已过期|报警钉钉群信息。 |
|ParentFlowList|String|否|F-62ECFC6E1BF6EAD2,F-1E6528634E67B615,F-7E0A84332E9D9A89|依赖的上游工作流列表，以逗号分隔。您可以调用[ListFlowInstance](~~101166~~)查看工作流ID。 |
|ParentCategory|String|否|FC-F2495319DA05\*\*\*\*|父目录ID。您可以调用[DescribeFlowCategory](~~100977~~)查看。为空时默认为root目录。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Id|String|F-7A39731FE719\*\*\*\*|新创建的工作流ID。 |
|RequestId|String|243D5A48-96A5-4C0C-8966-93CBF65635ED|请求ID。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=CreateFlowForWeb
&Description=这是一个创建工作流描述
&Name=my_flow_demo
&ProjectId=FP-257A173659F5****
&RegionId=cn-hangzhou
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<RequestId>2670BCFB-925D-4C3E-9994-8D12F7A9F538</RequestId>
<Id>F-7A39731FE719****</Id>
```

`JSON` 格式

```
{
    "RequestId": "2670BCFB-925D-4C3E-9994-8D12F7A9F538",
    "Id": "F-7A39731FE719****"
}
```

