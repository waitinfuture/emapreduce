# DescribeJob {#concept_nyd_h3b_kfb .concept}

You can call this operation to query the details of a job.

## Request parameters {#section_s3t_h3b_kfb .section}

|Field|Type|Required|Default value|Description|
|-----|----|--------|-------------|-----------|
|Id|String|Yes|None|The ID of the job.|
|RegionId|String|Yes|None|The ID of the region.|

## Response parameters {#section_tg2_j3b_kfb .section}

|Field|Type|Description|
|-----|----|-----------|
|Id|String|The ID of the job.|
|Name|String|The name of the job.|
|Type|String|The type of the job. Valid values: HADOOP, SPARK, HIVE, PIG, SQOOP, SPARK\_SQL, and SHELL.|
|RunParameter|String|The parameter of the job.|
|FailAct|String|The action to take after failure. STOP: Stops querying the details of a job. CONTINUE: Continues querying the details of the subsequent jobs.|

## Examples {#section_c21_l3b_kfb .section}

-   Sample requests

    ```
    https://emr.aliyuncs.com/?Action=DescribeJob
    &Id=J-13A570B821D4BAB3
    &RegionId=cn-hangzhou
    &Common request parameters
    ```

-   Sample responses

    JSON format

    ```
    {
        "RequestId": "34B08619-2636-49F9-AB4E-CD8D347B1E07",
        "Id": "J-13A570B821D4BAB3",
        "Name": "test",
        "FailAct": "CONTINUE",
        "Type": "HIVE",
        "RunParameter": "-f%20ossref://emr/count.sql"
    }
    ```


