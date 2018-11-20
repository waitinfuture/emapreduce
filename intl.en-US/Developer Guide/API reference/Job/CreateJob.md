# CreateJob {#concept_ims_lhb_kfb .concept}

You can call this operation to create a job.

## Request parameters {#section_st5_mhb_kfb .section}

|Field|Type|Required|Default value|Description|
|-----|----|--------|-------------|-----------|
|Name|String|Yes|None|The name of the job.|
|Type|String|Yes|None|The type of the job. Valid values: HADOOP, SPARK, HIVE, PIG, SQOOP, SPARK\_SQL, and SHELL.|
|RunParameter|String|Yes|None|The parameter of the job.|
|FailAct|String|Yes|None|The action to take after failure. STOP: Stops creating the job. CONTINUE: Continues creating the subsequent jobs.|
|RegionId|String|Yes|None| |

## Response parameters {#section_vw3_4hb_kfb .section}

|Field|Type|Description|
|-----|----|-----------|
|Id|String|The ID of the job.|

## Examples {#section_ngq_qhb_kfb .section}

-   Sample requests

    ```
    https://emr.aliyuncs.com/?Action=CreateJob
    &Name=CreateJobApiTest-SPARK
    &Type=SPARK
    &EnvParam=--class%20org.apache.spark.examples.SparkPi%20--master%20yarn-client%20%E2%80%93num-executors%202%20--executor-memory%202g%20--executor-cores%202%20/opt/apps/spark-1.6.0-bin-hadoop2.6/lib/spark-examples*.jar%2010
    &FailAct=STOP
    &amp;RegionId=cn-hangzhou
    &Common request parameters
    ```

-   Sample responses

    JSON format

    ```
    {
        "RequestId": "34B08619-2636-49F9-AB4E-CD8D347B1E07",
        "InstanceId": "J-13A570B821D4BAB3"
    }
    ```


