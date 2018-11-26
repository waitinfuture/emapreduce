# DeleteJob {#concept_nh2_s3b_kfb .concept}

You can call this operation to delete a job.

## Request parameters {#section_nyd_53b_kfb .section}

|Field|Type|Required|Default value|Description|
|-----|----|--------|-------------|-----------|
|Id|String|Yes|None|The ID of the job.|
|RegionId|String|Yes|None|The ID of the region.|

## Response parameters {#section_pll_v3b_kfb .section}

Common response parameters

## Examples {#section_tz5_v3b_kfb .section}

-   Sample requests

    ```
    https://emr.aliyuncs.com/?Action=DeleteJob
    &Id=J-13A570B821D4BAB3
    &RegionId=cn-hangzhou
    &Common request parameters
    ```

-   Sample responses

    JSON format

    ```
    {
        "RequestId": "34B08619-2636-49F9-AB4E-CD8D347B1E07",
    }
    ```


