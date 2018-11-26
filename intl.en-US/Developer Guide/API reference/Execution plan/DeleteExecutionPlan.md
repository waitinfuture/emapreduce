# DeleteExecutionPlan {#concept_xwc_4lb_kfb .concept}

You can call this operation to delete an execution plan.

## Request parameters {#section_mz5_4lb_kfb .section}

|Field|Type|Required|Default value|Description|
|-----|----|--------|-------------|-----------|
|Id|String|Yes|None|The ID of the execution plan.|
|RegionId|String|Yes|None|The region in which the cluster is deployed.|

## Response parameters {#section_ew2_qlb_kfb .section}

Common response parameters

## Examples {#section_py5_tlb_kfb .section}

-   Sample requests

    ```
    https://emr.aliyuncs.com/?Action=DeleteExecutionPlan
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


