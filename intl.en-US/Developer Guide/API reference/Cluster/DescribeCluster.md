# DescribeCluster {#concept_gcm_ydb_kfb .concept}

You can call this operation to query the details of a cluster.

## Request parameters {#section_u3c_m2b_kfb .section}

|Field|Type|Required|Default value|Description|
|-----|----|--------|-------------|-----------|
|Id|String|Yes|None|The ID of the cluster.|
|RegionId|String|Yes|None|The region in which the cluster is deployed.|

## Response parameters {#section_v43_p2b_kfb .section}

|Field|Type|Description|
|-----|----|-----------|
|ClusterInfo|[ClusterInfo](EN-US_TP_18028.dita#concept_xw1_3qb_kfb)|The detailed information about the cluster.|

## Examples {#section_w5m_t2b_kfb .section}

-   Sample requests

    ```
    https://emr.aliyuncs.com/?Action=DescribeCluster
    &Id=500003771
    &PageSize=50
    &PageNumber=1
    &RegionId=cn-hangzhou
    &Common request parameters
    ```

-   Sample responses

    JSON format


