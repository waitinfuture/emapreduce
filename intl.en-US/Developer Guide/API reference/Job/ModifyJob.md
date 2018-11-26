# ModifyJob {#concept_pdd_whb_kfb .concept}

You can call this operation to modify a job.

## Request parameters {#section_ubn_xhb_kfb .section}

|Field|Type|Required|Default value|Description|
|-----|----|--------|-------------|-----------|
|Id|String|Yes|None|The ID of the job.|
|Name|String|Yes|None|The name of the job.|
|Type|String|Yes|None|The type of the job. Valid values: HADOOP, HIVE, PIG, SPARK, SQOOP, SPARK\_SQL, and SHELL.|
|RunParameter|String|Yes|None|The parameter of the job.|
|FailAct|String|Yes|None|The action to take after failure. STOP: Stops modifying the job. CONTINUE: Continues modifying the subsequent jobs.|
|RegionId|String|Yes|None| |

## Response parameters {#section_dnf_zhb_kfb .section}

Common response parameters

## Examples {#section_fxq_zhb_kfb .section}

-   Sample requests

    ```
    https://emr.aliyuncs.com/?Action=ModifyJob
    &Id=J-13A570B821D4BAB3
    &Name=test
    &Type=HIVE
    &EnvParam=-f%20ossref://emr/count.sql
    &FailAct=STOP
    &amp;RegionId=cn-hangzhou
    &Common request parameters
    ```

-   Sample responses

    JSON format

    ```
    {
        "RequestId": "34B08619-2636-49F9-AB4E-CD8D347B1E07"
    }
    ```


