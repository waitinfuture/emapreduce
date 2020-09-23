# ListFlowNodeInstance

调用ListFlowNodeInstance接口，查询工作流节点实例列表。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=ListFlowNodeInstance&type=RPC&version=2016-04-08)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ListFlowNodeInstance|系统规定参数。对于您自行拼凑HTTP/HTTPS URL发起的API请求，该参数为必选参数。取值：ListFlowNodeInstance。 |
|RegionId|String|是|cn-hangzhou|地域ID。您可以调用[DescribeRegions](~~25609~~)查看最新的阿里云地域列表。 |
|ProjectId|String|否|FP-257A173659F5\*\*\*\*|项目ID。你可以调用[ListFlowProject](~~101204~~)查看项目的ID。 |
|StartTime|Long|否|1540796248000|开始时间。 |
|StatusList.N|RepeatList|否|FAILED|状态列表：

 -   SUBMITTED（已提交）
-   RUNNING（运行中）
-   SUCCESS（执行成功）
-   FAILED（执行失败）
-   KILL\_FAILED（终止失败）
-   KILL\_SUCCESS（终止成功） |
|OrderBy|String|否|start\_time|排序字段。 |
|OrderType|String|否|desc|排序类型：

 -   ASC
-   DESC |
|PageNumber|Integer|否|1|分页页数。 |
|PageSize|Integer|否|20|分页查询的每页数量。 |
|ResourceGroupId|String|否|rg-bp67acfmxazb4p\*\*\*\*|目标资源组ID。你可以调用[ListResourceGroups](~~158855~~)查看资源组ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|FlowNodeInstances|Array of FlowNodeInstance| |节点实例列表。 |
|FlowNodeInstance| | | |
|ClusterId|String|C-A6C9F4F1E9E\*\*\*\*|集群ID。 |
|Duration|Long|200|运行持续时间。 |
|EndTime|Long|1540796248000|运行结束时间。 |
|ExternalChildIds|String|application\_1541559535023\_34028|任务的Application列表。 |
|ExternalId|String|application\_1541559535023\_3\*\*\*\*|启动器的Application的ID。 |
|ExternalInfo|String|empty|外部信息，例如运行作业的错误诊断信息。 |
|ExternalStatus|String|SUCCESS|实例对应的Container的状态：

 -   SUBMITTED（已提交）
-   RUNNING（运行中）
-   SUCCESS（执行成功）
-   FAILED（执行失败）
-   KILL\_FAILED（终止失败）
-   KILL\_SUCCESS（终止成功） |
|ExternalSubId|String|container\_1541559535023\_34027\_01\_00\*\*\*\*|启动器Container的ID。 |
|FailAct|String|STOP|失败策略：

 -   STOP（终止工作流）
-   CONTINUE（跳过） |
|FlowId|String|F-35683D0E4573\*\*\*\*|工作流ID。 |
|FlowInstanceId|String|FI-7CAF9709CD32\*\*\*\*|工作流实例ID。 |
|GmtCreate|Long|1540796236000|创建时间。 |
|GmtModified|Long|1540796247000|修改时间。 |
|HostName|String|emr-header-1.cluster-12345|节点实例运行所在的机器HostName，支持Master和Gateway机器。Hostname格式为**emr-header-1.cluster-12345**，可登录机器用**hostname**命令查看对应的值。 |
|Id|String|FNI-9D14A7CCF268\*\*\*\*|节点实例ID。 |
|JobId|String|FJ-A23BD131A862\*\*\*\*|作业ID。 |
|JobName|String|myJob|作业名称。 |
|JobParams|String|ls -l|作业内容。 |
|JobType|String|HIVE\_SQL|作业类型，目前支持：MR、Spark、Hive\_SQL、Hive、Pig、Sqoop、Spark\_SQL、Spark\_Streaming及Shell。 |
|MaxRetry|String|0|最大重试次数。 |
|NodeName|String|node|节点名称。 |
|Pending|Boolean|false|是否已经结束。 |
|ProjectId|String|FP-7A1018ADE917\*\*\*\*|项目ID。 |
|Retries|Integer|0|重试次数。 |
|RetryInterval|String|200|重试间隔。 |
|StartTime|Long|1540796237000|开始运行时间。 |
|Status|String|OK|实例的执行状态：

 -   PREP（等待启动）
-   SUBMITTING（提交中）
-   RUNNING（运行中）
-   DONE（已完成）
-   OK（执行成功）
-   FAILED（执行失败）
-   KILLED（已终止）
-   KILL\_FAILED（终止失败）
-   START\_RETRY（开始重试） |
|Type|String|JOB|节点类型。 |
|PageNumber|Integer|1|页码。 |
|PageSize|Integer|20|每页数量。 |
|RequestId|String|83B256D4-4E95-454B-AD08-799DF31D5556|请求ID。 |
|Total|Integer|12|总数。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=ListFlowNodeInstance
&RegionId=cn-hangzhou
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<RequestId>83B256D4-4E95-454B-AD08-799DF31D5556</RequestId>
<PageSize>20</PageSize>
<PageNumber>1</PageNumber>
<FlowNodeInstances>
    <FlowNodeInstance>
        <FailAct>STOP</FailAct>
        <EndTime>1540796248000</EndTime>
        <NodeName>node</NodeName>
        <GmtModified>1540796247000</GmtModified>
        <JobName>myJob</JobName>
        <FlowId>F-35683D0E4573****</FlowId>
        <ExternalStatus>SUCCESS</ExternalStatus>
        <ExternalInfo>empty</ExternalInfo>
        <Retries>0</Retries>
        <JobId>FJ-A23BD131A862****</JobId>
        <JobParams>ls -l</JobParams>
        <HostName>emr-header-1.cluster-12345</HostName>
        <Status>OK</Status>
        <ClusterId>C-A6C9F4F1E9E****</ClusterId>
        <ExternalId>application_1541559535023_3****</ExternalId>
        <ProjectId>FP-7A1018ADE917****</ProjectId>
        <StartTime>1540796237000</StartTime>
        <FlowInstanceId>FI-7CAF9709CD32****</FlowInstanceId>
        <Duration>200</Duration>
        <MaxRetry>0</MaxRetry>
        <ExternalSubId>container_1541559535023_34027_01_00****</ExternalSubId>
        <GmtCreate>1540796236000</GmtCreate>
        <Type>JOB</Type>
        <JobType>HIVE_SQL</JobType>
        <ExternalChildIds>application_1541559535023_34028</ExternalChildIds>
        <Id>FNI-9D14A7CCF268****</Id>
        <RetryInterval>200</RetryInterval>
        <Pending>false</Pending>
    </FlowNodeInstance>
</FlowNodeInstances>
<Total>12</Total>
```

`JSON` 格式

```
{
    "RequestId":"83B256D4-4E95-454B-AD08-799DF31D5556",
    "PageSize":"20",
    "PageNumber":"1",
    "FlowNodeInstances":{
        "FlowNodeInstance":[
            {
                "FailAct":"STOP",
                "EndTime":"1540796248000",
                "NodeName":"node",
                "GmtModified":"1540796247000",
                "JobName":"myJob",
                "FlowId":"F-35683D0E4573****",
                "ExternalStatus":"SUCCESS",
                "ExternalInfo":"empty",
                "Retries":"0",
                "JobId":"FJ-A23BD131A862****",
                "JobParams":"ls -l",
                "HostName":"emr-header-1.cluster-12345",
                "Status":"OK",
                "ClusterId":"C-A6C9F4F1E9E****",
                "ExternalId":"application_1541559535023_3****",
                "ProjectId":"FP-7A1018ADE917****",
                "StartTime":"1540796237000",
                "FlowInstanceId":"FI-7CAF9709CD32****",
                "Duration":"200","MaxRetry":"0",
                "ExternalSubId":"container_1541559535023_34027_01_00****",
                "GmtCreate":"1540796236000",
                "Type":"JOB",
                "JobType":"HIVE_SQL",
                "ExternalChildIds":"application_1541559535023_34028",
                "Id":"FNI-9D14A7CCF268****",
                "RetryInterval":"200",
                "Pending":"false"
                }
            ]
        },
        "Total":"12"
}
```

