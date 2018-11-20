# ListJobs {#concept_cjb_1jb_kfb .concept}

You can call this operation to query a list of jobs.

## Request parameters {#section_eld_bjb_kfb .section}

|Field|Type|Required|Default value|Description|
|-----|----|--------|-------------|-----------|
|RegionId|String|Yes|None| |
|PageNumber|Integer|No|1|The page number to query. The minimum value is 1.|
|PageSize|Integer|No|10|The number of rows to display per page.|

## Response parameters {#section_usr_djb_kfb .section}

|Field|Type|Description|
|-----|----|-----------|
|Jobs|Array<Job\>|The array of the jobs.|
|TotalCount|Integer|The total number of rows.|
|PageNumber|Integer|The current page number.|
|PageSize|Integer|The number of rows to display per page.|

## Examples {#section_uqm_3jb_kfb .section}

-   Sample requests

    ```
    https://emr.aliyuncs.com/?Action=ListJobs
    &RegionId=cn-hangzhou
    &PageNumber=1
    &PageSize=20
    &Common request parameters
    ```

-   Sample responses

    JSON format


