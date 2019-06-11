# ModifyClusterName {#concept_n1l_qcb_kfb .concept}

You can call this operation to modify the name of a cluster.

## Request parameters {#section_atg_5cb_kfb .section}

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Id|String|Yes|C-D7958B72E59BAB88|The ID of the cluster.|
|Name|String|Yes|bi\_hadoop|The new name of the cluster. The same naming rules for creating the cluster are required. Length constraints: Minimum length of 1 character. Maximum length of 64 characters. Only Chinese characters, letters, numbers, hyphens \(-\), and underscores \(\_\) are allowed.|
|RegionId|String|Yes|cn-hangzhou|The region ID.|
|AccessKeyId|String|Yes|LTAI8ljWyu7y\*\*\*\*|The AccessKey ID.|

## Response parameters {#section_sby_zcb_kfb .section}

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|588E7707-D8C8-4618-9B74-FE54B6903A9A|The ID of the request.|

## Examples {#section_s4t_2db_kfb .section}

-   Sample requests

    ```
    https://emr.aliyuncs.com/?Action=ModifyClusterName
    &AccessKeyId=LTAI8ljWyu7y****
    &Id=C-13A570B821D4BAB3 
    &Name=bi_hadoop
    &RegionId=cn-hangzhou
    &<Common request parameters>
    ```

-   Sample responses

    JSON format

    ```
    {
        "RequestId": "588E7707-D8C8-4618-9B74-FE54B6903A9A"
    }
    ```


