# ListExecutionPlans {#concept_ext_skb_kfb .concept}

You can call this operation to query a list of execution plans.

## Request parameters {#section_dvy_tkb_kfb .section}

|Field|Type|Required|Default value|Description|
|-----|----|--------|-------------|-----------|
|RegionId|String|Yes|None| |
|PageNumber|Integer|No|1|The page number to query.|
|PageSize|Integer|No|10|The number of rows to display per page.|

## Response parameters {#section_fcm_xkb_kfb .section}

|Field|Type|Required|
|-----|----|--------|
|ExecutionPlans|Array<[ExecutionPlanInfo](EN-US_TP_18032.dita#concept_ykd_qsb_kfb)\>|The list of the execution plans.|
|TotalCount|Integer|The total number of rows.|
|PageNumber|Integer|The current page number.|
|PageSize|Integer  |The number of rows to display per page.|

## Examples {#section_ohm_zkb_kfb .section}

-   Sample requests

    ```
    https://emr.aliyuncs.com/?Action=ListExecutionPlans
    &PageSize=50
    &PageNumber=1
    &RegionId=cn-hangzhou
    &Common request parameters
    ```

-   Sample responses

    JSON format


