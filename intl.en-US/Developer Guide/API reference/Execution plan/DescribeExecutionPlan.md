# DescribeExecutionPlan {#concept_cjn_hkb_kfb .concept}

You can call this operation to query the details of an execution plan.

## Request parameters {#section_i4t_3kb_kfb .section}

|Field|Type|Required|Default value|Description|
|-----|----|--------|-------------|-----------|
|Id|String|Yes|None|The ID of the execution plan.|
|RegionId|String|Yes|None|The ID of the region.|

## Response parameters {#section_gdd_kkb_kfb .section}

|Field|Type|Description|
|-----|----|-----------|
|Id|String |The ID of the execution plan.|
|Name|String|The name of the execution plan.|
|Strategy|String|The running policy of the execution plan. RUN\_MANUALLY: Manual execution. Run the execution plan only when triggered by users. SCHEDULE: Periodic scheduling. Run the execution plan on the set interval.|
|StartTime|Long|The start time of the periodic scheduling.|
|TimeUnit|String|The time interval for periodic scheduling. DAY: The unit is the day. HOUR: The unit is the hour.|
|TimeInterval|Integer|The time interval. If the unit is the day, set the value to 1. If the unit is the hour, set the value to a number in the range from 1 to 23.|
|JobInfoList|[JobInfo](EN-US_TP_18034.dita#concept_jqv_gtb_kfb)|The list of the associated jobs.|
|CreateClusterOnDemand|Boolean|Indicates whether to create a dynamic cluster on demand.|
|ClusterId|String|The ID of the cluster. If CreateClusterOnDemand==false, this value is returned. Otherwise, no value is returned because it is a cluster created on demand.|
|ClusterInfo|[ClusterInfo](EN-US_TP_18028.dita#concept_xw1_3qb_kfb)|The detailed information about the cluster. If CreateClusterOnDemand==true, this value is returned.|

## Examples {#section_jwt_lkb_kfb .section}

-   Sample requests

    ```
    https://emr.aliyuncs.com//?Action=DescribeExecutionPlan
    &Id=WF-13A570B821D4BAB3
    &RegionId=cn-hangzhou
    &Common request parameters
    ```

-   Sample responses

    JSON format

    ```

    ```


