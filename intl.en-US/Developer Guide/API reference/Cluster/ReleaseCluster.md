# ReleaseCluster {#concept_ip5_dfb_kfb .concept}

You can call this operation to release a cluster.

## Request parameters {#section_p5v_ffb_kfb .section}

|Field|Type|Required|Default value|Description|
|-----|----|--------|-------------|-----------|
|Id|String|Yes|None|The ID of the cluster.|
|ForceRelease|Boolean|No|false|Specifies whether to force the cluster to release. For more information, see [Release a cluster](../DNemapreduce1876943/EN-US_TP_17855.dita#concept_vrx_3pn_y2b).|
|RegionId|String|Yes|None|The region in which the cluster is deployed.|

## Response parameters {#section_mdk_3fb_kfb .section}

Common response parameters

## Examples {#section_chx_3fb_kfb .section}

-   Sample requests

    ```
    https://emr.aliyuncs.com/?Action=ReleaseCluster
    &ForceRelease=false
    &Id=C-13A570B821D4BAB3
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


