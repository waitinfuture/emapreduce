# SuspendExecutionPlanScheduler {#concept_bbb_smb_kfb .concept}

You can call this operation to stop the periodic scheduling of an execution plan.

## Request parameters {#section_v2k_5mb_kfb .section}

|Field|Type|Required|Default value|Description|
|-----|----|--------|-------------|-----------|
|RegionId|String|Yes|None|The ID of the region.|
|Id|String|Yes|None|The ID of the execution plan, of which the periodic scheduling is stopped.|

## Response parameters {#section_tkr_zmb_kfb .section}

Common response parameters

## Examples {#section_orv_zmb_kfb .section}

-   Sample requests

    ```
    https://emr.aliyuncs.com/?Action=SuspendExecutionPlanScheduler
    &Id=WF-13A570B821D4BAB3
    &RegionId=cn-hangzhou
    ```

-   Sample responses

    JSON format

    ```
    {
        "RequestId": "34B08619-2636-49F9-AB4E-CD8D347B1E07"
    }
    ```


