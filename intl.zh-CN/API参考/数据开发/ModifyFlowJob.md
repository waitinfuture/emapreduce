# ModifyFlowJob

调用ModifyFlowJob接口，修改数据开发作业。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=ModifyFlowJob&type=RPC&version=2016-04-08)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ModifyFlowJob|系统规定参数。对于您自行拼凑HTTP或HTTPS URL发起的API请求，该参数为必选参数。取值：ModifyFlowJob。 |
|Id|String|是|FJ-BCCAE48B90CC\*\*\*\*|作业ID。 |
|ProjectId|String|是|FP-257A173659F5\*\*\*\*|项目ID。 |
|RegionId|String|是|cn-hangzhou|地域ID。 |
|ResourceList.N.Path|String|是|oss://path/demo.jar|资源的路径，支持OSS和HDFS。 |
|Name|String|否|my\_shell\_job|作业的名称。 |
|Description|String|否|这是一个数据开发作业描述|作业的描述。 |
|FailAct|String|否|CONTINUE|失败策略，支持：

 -   CONTINUE（跳过）。
-   STOP（停止工作流）。 |
|MaxRetry|Integer|否|5|最大重试次数，取值范围0~5次。 |
|RetryPolicy|String|否|无|重试策略，保留参数。 |
|MaxRunningTimeSec|Long|否|0|保留参数。 |
|RetryInterval|Long|否|200|重试间隔，取值范围0~300（秒）。 |
|Params|String|否|ls -l|作业内容。 |
|ParamConf|String|否|\{"date":"$\{yyyy-MM-dd\}"\}|参数设置。 |
|CustomVariables|String|否|\{\\"scope\\":\\"PROJECT\\",\\"entityId\\":\\"FP-80C2FDDBF35D9CC5\\",\\"variables\\":\[\{\\"name\\":\\"v1\\",\\"value\\":\\"1\\",\\"properties\\":\{\\"password\\":true\}\}\]\}|自定义变量。 |
|EnvConf|String|否|\{"key":"value"\}|环境变量设置。 |
|RunConf|String|否|\{"priority":1,"userName":"hadoop","memory":2048,"cores":1\}|运行配置：

 -   priority：优先级。
-   userName：提交作业的Linux用户。
-   memory：内存，单位为MB。
-   cores：核数。 |
|MonitorConf|String|否|\{"inputs":\[\{"type":"KAFKA","clusterId":"C-1234567","topics":"kafka\_topic","consumer.group":"kafka\_consumer\_group"\}\],"outputs":\[\{"type":"KAFKA","clusterId":"C-1234567","topics":"kafka\_topic"\}\]\}|监控配置，只有**SPARK\_STREAMING**类型作业支持。 |
|Mode|String|否|YARN|模型模式：

 -   YARN：将作业包装成一个launcher提交至YARN中执行。
    -   LOCAL：作业直接在机器上以进程方式运行。 |
|ResourceList.N.Alias|String|否|demo.jar|资源的别名。 |
|ClusterId|String|否|C-A23BD131A862\*\*\*\*|集群ID。 |
|AlertConf|String|否|无|保留参数。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Data|Boolean|true|结果。 |
|RequestId|String|549175a-6d14-4c8a-89f9-5e28300f6d7e|请求ID。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=ModifyFlowJob
&Id=FJ-BBCAE48B90CC****
&ProjectId=FP-257A173659F5****
&RegionId=cn-hangzhou
&ClusterId=C-A23BD131A862****
&Description=这是一个数据开发作业描述
&EnvConf={"key":"value"}
&FailAct=CONTINUE
&MaxRetry=5
&Mode=YARN
&MonitorConf={"inputs":[{"type":"KAFKA","clusterId":"C-1234567","topics":"kafka_topic","consumer.group":"kafka_consumer_group"}],"outputs":[{"type":"KAFKA","clusterId":"C-1234567","topics":"kafka_topic"}]}
&Name=my_shell_job
&ParamConf={"date":"${yyyy-MM-dd}"}
&Params=ls -l
&ResourceList.1.Alias=demo.jar
&ResourceList.1.Path=oss://path/demo.jar
&RetryInterval=200
&RunConf={"priority":1,"userName":"hadoop","memory":2048,"cores":1}
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<RequestId>ECC2D0D1-B6D5-468D-B698-30E8805EB574</RequestId>
<Data>true</Data>
```

`JSON` 格式

```
{
"RequestId":"ECC2D0D1-B6D5-468D-B698-30E8805EB574",
"Data":true
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/Emr)查看更多错误码。

