# KillExecutionJobInstance {#concept_u5w_p4b_kfb .concept}

You can call this operation to stop a job instance.

## Request parameters {#section_ugn_54b_kfb .section}

|Field|Type|Required|Default value|Description|
|-----|----|--------|-------------|-----------|
|RegionId|String|Yes|None| |
|JobInstanceId|String|Yes|None|The ID of the job instance.|

## Response parameters {#section_lys_w4b_kfb .section}

Common response parameters

## Examples {#section_mng_y4b_kfb .section}

-   Sample requests

    ```
    https://emr.aliyuncs.com/?Action=KillExecutionJobInstance
    &JobInstanceId=WNE-F18B014FF410410F
    &RegionId=cn-hangzhou
    &Common request parameters
    ```

-   Sample responses

    JSON format

    ```
    {
        "RequestId": "34B08619-2636-49F9-AB4E-CD8D347B1E07"
    }
    ```


