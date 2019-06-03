# CreateFlowJob {#concept_vvh_t2p_fgb .concept}

You can call this operation to create a job.

## Request parameters {#section_obj_yxl_2gb .section}

|Name|Type|Required|Example|Description|
|----|----|--------|-------|-----------|
|Description|String|Yes|"This is a job description."|The description of the job.|
|Name|String|Yes|my\_shell\_job|The name of the job.|
|ProjectId|String|Yes|FP-257A173659F59685|The ID of the project.|
|RegionId|String|Yes|cn-hangzhou|The ID of the region.|
|Type|String|Yes|SHELL|The type of the job. Valid values: MR, SPARK, HIVE\_SQL, HIVE, PIG, SQOOP, SPARK\_SQL, SPARK\_STREAMING, and SHELL.|
|Adhoc|Boolean|No|false|Indicates whether the job is an ad-hoc query.|
|ClusterId|String|No|C-A23BD131A862F184|The ID of the cluster.|
|EnvConf|String|No|\{"key":"value"\}|The configuration of the environment variables.|
|FailAct|String|No|CONTINUE|The strategy to apply after failure. Valid values: CONTINUE and STOP.|
|MaxRetry|Integer|No|5|The maximum number of retries. Valid values: 0 to 5.|
|Mode|String|No|YARN|The mode of the model. Valid values: YARN and LOCAL. YARN: Packages the job to a launcher and submit it to YARN for execution. LOCAL: Runs the job as a process on the machine.|
|MonitorConf|String|No|\{"inputs":\[\{"type":"KAFKA","clusterId":"C-1234567","topics":"kafka\_topic","consumer.group":"kafka\_consumer\_group"\}\],"outputs":\[\{"type":"KAFKA","clusterId":"C-1234567","topics":"kafka\_topic"\}\]\}|The configurations of the monitor. \*|
|ParamConf|String|No|\{"date":"$\{yyyy-MM-dd\}"\}|The configurations of the parameter.|
|Params|String|No|ls -l|The content of the job.|
|ParentCategory|String|No|FC-5BD9575E34623940|The ID of the parent category.|
|ResourceList.N.Alias|String|No|demo.jar|The alias of the resource.|
|ResourceList.N.Path|String|No|oss://path/demo.jar|The path of the resource. Valid values: oss and hdfs.|
|RetryInterval|Long|No|200|The interval between retries. Valid values: 0 to 300. Unit: seconds.|
|RunConf|String|No|\{"priority":1,"userName":"hadoop","memory":2048,"cores":1\}|The run configurations. priority: The priority to run the job. userName: The Linux user that submits the job. memory: The size of the allocated memory for running the job. Unit: megabytes. cores: The number of allocated vCPUs for running the job.|

## Response parameters {#section_vbj_yxl_2gb .section}

|Name|Type|Example|Description|
|----|----|-------|-----------|
|RequestId|String|1549175a-6d14-4c8a-89f9-5e28300f6d7e|The ID of the request.|
|Id|String|FJ-A23BD131A862F184|The ID of the job.|

## Examples {#section_dcj_yxl_2gb .section}

-   Sample requests

    ``` {#codeblock_u5m_duv_k6y}
    /? Description=This is a job description.
    &Name=my_shell_job
    &ProjectId=FP-257A173659F59685
    &RegionId=cn-hangzhou
    &Type=SHELL
    &Adhoc=false
    &ClusterId=C-A23BD131A862F184
    &EnvConf={"key":"value"}
    &FailAct=CONTINUE
    &MaxRetry=5
    &Mode=YARN
    &MonitorConf={"inputs":[{"type":"KAFKA","clusterId":"C-1234567","topics":"kafka_topic","consumer.group":"kafka_consumer_group"}],"outputs":[{"type":"KAFKA","clusterId":"C-1234567","topics":"kafka_topic"}]}
    &ParamConf={"date":"${yyyy-MM-dd}"}
    &Params=ls -l
    &ParentCategory=FC-5BD9575E34623940
    &RetryInterval=200
    &RunConf={"priority":1,"userName":"hadoop","memory":2048,"cores":1}
    &ResourceList. 1 Alias=demo.jar
    &ResourceList. 1. Path=oss://path/demo.jar
    &<Common request parameters>
    ```

-   Successful response examples

    JSON format

    ``` {#codeblock_zjt_2np_7sm}
    {
        "Id":"FJ-BBCAE48B90CCB37B",
        "RequestId":"2670BCFB-925D-4C3E-9994-8D12F7A9F538"
    }
    ```

-   Error response examples

    JSON format

    ``` {#codeblock_o3y_p2b_o53}
    {
        "code":"FLOW_API_FAILED",
        "message":"Invalid job type [INVALID_TYPE]",
        "requestId":"11BAFBD8-8509-4177-A26D-407505E73713",
        "successResponse":false
    }
    ```


## Error codes {#section_fcj_yxl_2gb .section}

