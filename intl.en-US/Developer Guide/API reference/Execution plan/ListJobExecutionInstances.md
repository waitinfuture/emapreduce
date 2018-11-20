# ListJobExecutionInstances {#concept_prc_5nb_kfb .concept}

You can call this operation to query a list of job instances.

## Request parameters {#section_ws4_vnb_kfb .section}

|Field|Type|Required|Default value|Description|
|-----|----|--------|-------------|-----------|
|RegionId|String|Yes|None| |
|ExecutionPlanInstanceId|String|Yes|None|The ID of the execution plan instance.|
|pageNumber|Integer|No|1|The page number to query.|
|pageSize|Integer|No|10|The number of rows to display per page.|

## Response parameters {#section_sxg_znb_kfb .section}

|Field|Type|Required|
|-----|----|--------|
|JobInstances|Array<[JobInstance](EN-US_TP_18035.dita#concept_fb5_jtb_kfb)\>|The information about the job instances.|
|TotalCount|Integer|The total number of rows.|
|PageNumber|Integer|The current page number.|
|PageSize |Integer  |The number of rows to display per page.|

## Examples {#section_g5x_g4b_kfb .section}

-   Sample requests

    ```
    https://emr.aliyuncs.com/?Action=ListJobExecutionInstances
    &ExecutionPlanInstanceId=500006685
    &PageSize=100
    &PageNumber=1
    &RegionId=cn-hangzhou
    &Common request parameters
    ```

-   Sample responses

    JSON format

    ```

    ```


