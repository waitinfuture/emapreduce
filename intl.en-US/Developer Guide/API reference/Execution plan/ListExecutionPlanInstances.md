# ListExecutionPlanInstances {#concept_jgt_gnb_kfb .concept}

You can call this operation to query a list of execution plan instances.

## Request parameters {#section_fws_hnb_kfb .section}

|Field|Type|Required|Default value|Description|
|-----|----|--------|-------------|-----------|
|Id|String|Yes|None|The ID of the execution plan.|
|RegionId|String|Yes|None| |
|ExecutionPlanIdList|Array<String\>|Yes|None|A list of the IDs of the execution plans to query. It is a string array. For example, \[“WF-5D93B00901730B5E”,”WF-5D93B0090173ABCD”\]|
|OnlyLastInstance|Boolean|No|false|Returns the last running logs \(instances\) of all the execution plans on the list if onlyLastInstance=true. If you set the value of the onlyLastInstance parameter to true, then you do not need to specify the PageNumber parameter and the PageSize parameter, and the settings do not take effect.|
|PageNumber|Integer|No|1|The page number to query.|
|PageSize|Integer|No|10|The number of rows to display per page.|

## Response parameters {#section_btp_mnb_kfb .section}

|Field|Type|Required|
|-----|----|--------|
|ExecutionPlanInstances|Array<[ExecutionPlanInstance](EN-US_TP_18033.dita#concept_vtw_btb_kfb)\>|The details of the execution plan instances.|
|TotalCount|Integer|The total number of records.|
|PageNumber|Integer|The current page number.|
|PageSize |Integer  |The number of rows to display per page.|

## Examples {#section_okt_4nb_kfb .section}

-   Sample requests

    ```
    https://emr.aliyuncs.com/?Action=ListExecutionPlanInstances
    &ExecutionPlanIdList=%5B%22WF-5D93B00901730B5E%22%2C%22WF-5D93B0090173ABCD%22%5D
    &OnlyLastInstance=true
    &PageNumber=1
    &PageSize=50
    &RegionId=cn-hangzhou
    &Common request parameters
    ```

-   Sample responses

    JSON format

    ```

    ```


