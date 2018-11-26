# ListClusters {#concept_rn1_cgb_kfb .concept}

You can call this operation to query a list of clusters.

## Request parameters {#section_kzt_fgb_kfb .section}

|Field|Type|Required|Default value|Description|
|-----|----|--------|-------------|-----------|
|StatusList|Array<String\>|No|None|Filter the clusters by status. For more information about cluster status, see [Status of a cluster](../DNemapreduce1851503/ZH-CN_TP_18065.dita#concept_zkb_3cc_pfb). Use a string array in the following format: \[“CREATING”, “RUNNING”, “IDLE”\]|
|RegionId|String|Yes|None| |
|PageNumber|Integer|No|1|The page number to query.|
|PageSize|Integer|No|10|The number of rows to display per page.|

## Response parameters {#section_s2x_hgb_kfb .section}

|Field|Type|Description|
|-----|----|-----------|
|Cluster|Array<ClusterInfoSimple\>|A ClusterInfoSimple array. For the structure of ClusterInfoSimple, see the following table.|
|TotalCount|Integer|The total number of records.|
|PageNumber|Integer|The current page number.|
|PageSize|Integer|The number of records that appear on the current page.|

ClusterInfoSimple

|Field|Type|Description|
|-----|----|-----------|
|Id|String|The ID of the cluster.|
|Name|String|The name of the cluster.|
|ChargeType|String|The billing method for the cluster. PrePaid: Subscription. PostPaid: Pay-As-You-Go.|
|ExpiredTime|Long|The expiration time of the cluster. The data type is TIMESTAMP. For example, 1459236255438.|
|Type|String|The type of the cluster. Valid values: HADOOP and HBASE.|
|CreateTime|Long|The creation time of the cluster. The data type is TIMESTAMP. For example, 1459236255438.|
|RunningTime|Integer|The running time of the cluster. Unit: seconds.|
|Status|String|The status of the cluster. For more information about cluster status, see [Status of a cluster](../DNemapreduce1851503/ZH-CN_TP_18065.dita#concept_zkb_3cc_pfb).|
|FailReason|[FailReason](EN-US_TP_18038.dita#concept_gct_ttb_kfb)|The cause of cluster creation failure.|
|CreateType|String|ON-DEMAND: Clusters that are created dynamically using execution plans. MANUAL: Clusters that are created manually.|

-   Sample requests

    ```
    https://emr.aliyuncs.com/?Action=ListClusters
    &PageSize=100
    &StatusList=%5B%22CREATING%22%2C+%22RUNNING%22%2C+%22IDLE%22%2C+%22RELEASING%22%2C+%22CREATE_FAILED%22%2C+%22RELEASE_FAILED%22%5D
    &PageNumber=1
    &RegionId=cn-hangzhou
    &Common request parameters
    ```

-   Sample responses

    JSON format


