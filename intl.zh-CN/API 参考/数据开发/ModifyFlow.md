# ModifyFlow {#doc_api_Emr_ModifyFlow .reference}

调用ModifyFlow接口修改工作流。

## 调试 {#api_explorer .section}

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=ModifyFlow&type=RPC&version=2016-04-08)

## 请求参数 {#parameters .section}

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ModifyFlow|系统规定参数。取值：ModifyFlow。

 |
|Id|String|是|F-7A39731FE719\*\*\*\*|工作流ID。

 |
|ProjectId|String|是|FP-3535FE0BE522\*\*\*\*|项目ID。

 |
|RegionId|String|是|cn-hangzhou|区域ID。

 |
|AlertConf|String|否|\{"items":\[\{"enable":true,"eventId":"EMR-210401001","alertUserGroupIdList":\["AUG-b79bb29bb6e14ddd89674a242623851b"\],"alertDingDingGroupList":\["ADG-af1f9689d6194e2dbd89927d5c515172"\]\},\{"enable":true,"eventId":"EMR-110401015","alertUserGroupIdList":\["AUG-b79bb29bb6e14ddd89674a242623851b"\],"alertDingDingGroupList":\["ADG-af1f9689d6194e2dbd89927d5c515172"\]\},\{"enable":true,"eventId":"EMR-110401002","alertUserGroupIdList":\["AUG-b79bb29bb6e14ddd89674a242623851b"\],"alertDingDingGroupList":\["ADG-af1f9689d6194e2dbd89927d5c515172"\]\}\]\}|报警通知配置, eventId目前支持：

 -   **EMR-210401001**（工作流失败报警）。
-   **EMR-110401002**（工作流成功通知）。
-   **EMR-110401015**（工作流节点失败报警）。

 |
|AlertDingDingGroupBizId|String|否|已过期|已过期。

 |
|AlertUserGroupBizId|String|否|已过期|已过期。

 |
|Application|String|否|\{"nodeDefMap":\{":start:":\{"name":":start:","type":":start:","transitions":\["cluster"\]\},"cluster":\{"id":"CT-0C74281682CF03B4","name":"cluster","type":":Cluster:","transitions":\["job1"\]\},"job1":\{"jobId":"FJ-242AB240DBAF4195","name":"job1","type":":action:","transitions":\["end"\]\},"end":\{"name":"end","type":":end:"\}\}\}|工作流构造信息，有一组节点定义nodeDefMap组成。

 -   **type**：类型包括开始节点（:start:）、构建按需集群节点（:Cluster:）、动作节点（:action:）和结束节点（:end:）。
-   **transitions**：下游的节点。

 多个以逗号分隔。

 |
|ClusterId|String|否|C-F32FB31D8295\*\*\*\*|集群ID。

 |
|CreateCluster|Boolean|否|false|是否通过集群模板创建集群，**ture**表示通过集群模板创建，集群ClusterId应设置为集群模板ID（CT-xxx），否则为已有集群ID（C-xxx）。

 |
|CronExpr|String|否|0 0 0-23/1 \* \* ?|时间周期调度的Cron表达式，请参见[Cron表达式](https://docs.oracle.com/cd/E12058_01/doc/doc.1014/e12030/cron_expressions.htm)。

 |
|Description|String|否|my flow description|工作流描述, 长度限制为156个字符。

 |
|EndSchedule|Long|否|1542783967503|调度失效时间，长整型时间戳，例如，System.currentTimeMillis\(\)。

 |
|HostName|String|否|emr-header-1.cluster-12345|工作流运行所在的机器Hostname，支持Master和Gateway机器。Hostname格式为**emr-header-1.cluster-12345**，您可登录机器使用**hostname**命令查看对应的值。

 |
|Name|String|否|my\_flow|工作流名称，长度限制为64个字符，并且同一个项目中不允许重名。

 |
|ParentCategory|String|否|FC-FC396F988E07C06F|父目录ID, 空则默认为root目录。

 |
|ParentFlowList|String|否|F-62ECFC6E1BF6EAD2,F-1E6528634E67B615,F-7E0A84332E9D9A89|依赖的上游工作流ID列表，以逗号分隔。

 |
|Periodic|Boolean|否|true|是否周期调度。

 |
|StartSchedule|Long|否|1542783867503|开始时间, 长整型时间戳，例如，System.currentTimeMillis\(\)。开始时间的限制如下：

 -   开始时间必须小于EndSchedule。
-   当CronExpr不为空时， 此项必填。

 |
|Status|String|否|UNDER\_SCHEDULE|工作流状态。

 |

## 返回数据 {#resultMapping .section}

|名称|类型|示例值|描述|
|--|--|---|--|
|Data|Boolean|true|修改结果，**true**表示成功，**false**表示失败。

 |
|RequestId|String|ECC2D0D1-B6D5-468D-B698-30E8805EB574|请求ID。

 |

## 示例 {#demo .section}

请求示例

``` {#request_demo}

http(s)://[Endpoint]/?Action=ModifyFlow
&Id=F-7A39731FE719****
&ProjectId=FP-3535FE0BE522****
&RegionId=cn-hangzhou
&<公共请求参数>

```

正常返回示例

`XML` 格式

``` {#xml_return_success_demo}
<ModifyFlowResponse>
	  <RequestId>ECC2D0D1-B6D5-468D-B698-30E8805EB574</RequestId>
	  <Data>true</Data>
</ModifyFlowResponse>
```

`JSON` 格式

``` {#json_return_success_demo}
{
	"Data":true,
	"RequestId":"ECC2D0D1-B6D5-468D-B698-30E8805EB574"
}
```

## 错误码 { .section}

访问[错误中心](https://error-center.alibabacloud.com/status/product/Emr)查看更多错误码。

