# ModifyFlowJob

You can call this operation to modify a data development job.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=Emr&api=ModifyFlowJob&type=RPC&version=2016-04-08)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes |ModifyFlowJob|The operation that you want to perform. For API requests using the HTTP or HTTPS URL, this parameter is required. Set the value to ModifyFlowJob. |
|Id|String|Yes |FJ-BCCAE48B90CC\*\*\*\*|The ID of the job. |
|ProjectId|String|Yes |FP-257A173659F5\*\*\*\*|The ID of the project. |
|RegionId|String|Yes |cn-hangzhou|The ID of the region. |
|ResourceList.N.Path|String|Yes |oss://path/demo.jar|The storage path of the resource. The resource can be stored in OSS and HDFS. |
|Name|String| No|my\_shell\_job|The name of the job. |
|Description|String| No|This is the description of a job|The description of the job. |
|FailAct|String| No|CONTINUE|The action to take upon an operation failure of the node instance. Valid values:

 -   CONTINUE: skips the node instance
-   STOP: stops the workflow instance |
|MaxRetry|Integer|Optional|5|The maximum number of retries. Value range: 0 to 5. |
|RetryPolicy|String| No|None|Retry policy and retain parameters. |
|MaxRunningTimeSec|Long|No|0|Retain parameters. |
|RetryInterval|Long|No|200|The retry interval. Value range: 0 to 300 \(seconds\). |
|Params|String| No|ls -l|The content of the job. |
|ParamConf|String| No|\{"date":"$\{yyyy-MM-dd\}"\}|The configuration parameters of the job. |
|CustomVariables|String| No|\{\\"scope\\":\\"PROJECT\\",\\"entityId\\":\\"FP-80C2FDDBF35D9CC5\\",\\"variables\\":\[\{\\"name\\":\\"v1\\",\\"value\\":\\"1\\",\\"properties\\":\{\\"password\\":true\}\}\]\}|The custom variables configured for the job. |
|EnvConf|String| No|\{"key":"value"\}|The environment variables configured for the job. |
|RunConf|String| No|\{"priority":1,"userName":"hadoop","memory":2048,"cores":1\}|The scheduling parameters configured for the job.

 -   priority: the priority of the job.
-   userName: the name of the Linux user who summits the job.
-   memory: the memory allocated to the job. Unit: MB.
-   cores: the number of vCPUs allocated to the job. |
|MonitorConf|String| No|\{"inputs":\[\{"type":"KAFKA","clusterId":"C-1234567","topics":"kafka\_topic","consumer.group":"kafka\_consumer\_group"\}\],"outputs":\[\{"type":"KAFKA","clusterId":"C-1234567","topics":"kafka\_topic"\}\]\}|Monitoring configuration, only **SPARK\_STREAMING** supported by job types. |
|Mode|String| No|YARN|Model mode:

 -   YARN: submits the job from a worker node
    -   LOCAL: submits the job from a header or gateway node |
|ResourceList.N.Alias|String| No|demo.jar|The alias of the resource. |
|ClusterId|String| No|C-A23BD131A862\*\*\*\*|The ID of the cluster. |
|AlertConf|String| No|None|Retain parameters. |

## Response parameters

|Parameter|Type|Sample response|Description|
|---------|----|---------------|-----------|
|Data|Boolean|true|The result of the operation. |
|RequestId|String|549175a-6d14-4c8a-89f9-5e28300f6d7e|The ID of the request |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=ModifyFlowJob
&Id=FJ-BBCAE48B90CC****
&ProjectId=FP-257A173659F5****
&RegionId=cn-hangzhou
&ClusterId=C-A23BD131A862****
&Description=This is the description of a job
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
&<Common request parameters>
```

Sample success responses

`XML` format

```
<RequestId>ECC2D0D1-B6D5-468D-B698-30E8805EB574</RequestId>
<Data>true</Data>
```

`JSON` format

```
{
"RequestId":"ECC2D0D1-B6D5-468D-B698-30E8805EB574",
"Data":true
}
```

## Errors codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/Emr).

