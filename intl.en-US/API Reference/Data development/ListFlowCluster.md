# ListFlowCluster {#concept_myd_mk2_ggb .concept}

You can call this operation to view the list of the project clusters that are available for data development.

## Request parameters {#section_obj_yxl_2gb .section}

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|ProjectId|String|Yes|FP-5D55DA9DEDF22ED3|The ID of the project.|
|PageNumber|Integer|No|1|The number of the page to request.|
|RegionId|String|Yes|cn-hangzhou|The ID of the region.|
|PageSize|Integer|No|20|The number of records to show on each page.|

## Response parameters {#section_vbj_yxl_2gb .section}

|Name|Type|Example|Description|
|----|----|-------|-----------|
|RequestId|String|0d18b019-00ab-455f-b60c-2891bf02f538|The ID of the request.|
|TotalCount|Integer|42|The total number of records.|
|PageNumber|Integer|1|The page number to request.|
|PageSize|Integer|20|The number of records to show on each page.|
|Clusters| | |The list of the clusters.|
|Id|String|C-2E6ABBD39BD09F97|The ID of the cluster.|
|Name|String|String|The name of the cluster.|
|Type|String|HADOOP|The type of the cluster.|
|CreateTime|Long|1542953603000|The creation time.|
|RunningTime|Integer|200|The running time. Unit: seconds.|
|Description|String|my flow demo|The description of the workflow.|
|Status|String|SUCCESS|Status|
|ChargeType|String|PostPaid|The billing method.|
|ExpiredTime|Long|1542953603000|The time when the cluster expires.|
|Period|Integer|1|The length of subscription.|
|HasUncompletedOrder|Boolean|false|Indicates whether uncompleted orders exist.|
|OrderList|String|1,2|Order List|
|CreateResource|String|ECM\_EMR|The resources.|
|OrderTaskInfo| | |The information of the order task.|
|TargetCount|Integer|2|The target number.|
|CurrentCount|Integer|2|The current number.|
|OrderIdList|String|1,2|The list of orders.|
|FailReason| | |The causes of failure.|
|Error code|String|123|Error codes|
|ErrorMsg|String|failed|The cause of the error.|
|RequestId|String|0d18b019-00ab-455f-b60c-2891bf02f538|The ID of the request.|

## Examples {#section_dcj_yxl_2gb .section}

-   Sample requests

    ``` {#codeblock_13s_5ot_c7y}
    
    /? ProjectId=FP-5D55DA9DEDF22ED3
    &RegionId=cn-hangzhou
    &PageNumber=1
    &PageSize=20
    &<Common request parameters>
    ```

-   Successful response examples

    JSON format

    ``` {#codeblock_yhw_lmz_lc0}
    {
        "Clusters":{
            "ClusterInfo":[
                {
                    "ChargeType":"PostPaid",
                    "CreateResource":"ECM_EMR",
                    "CreateTime":1542953603000,
                    "FailReason":{},
                    "HasUncompletedOrder":false,
                    "Id":"C-4042079DFF01A3F5",
                    "Name":"zx-presto-1",
                    "OrderTaskInfo":{},
                    "RunningTime":1322,
                    "Status":"IDLE",
                    "Type":"HADOOP"
                },
                {
                    "ChargeType":"PostPaid",
                    "CreateResource":"ECM_EMR",
                    "CreateTime":1542950611000,
                    "FailReason":{},
                    "HasUncompletedOrder":false,
                    "Id":"C-3D0B53253595DBB0",
                    "Name":"zx-presto-213",
                    "OrderTaskInfo":{},
                    "RunningTime":4314,
                    "Status":"IDLE",
                    "Type":"HADOOP"
                }
            ]
        },
        "PageNumber":1,
        "PageSize":10,
        "TotalCount":2
    }
    ```

-   Error response examples

    JSON format

    ``` {#codeblock_8x9_d7s_4qt}
    {
        "Clusters":{
            "ClusterInfo":[
                {
                    "ChargeType":"PostPaid",
                    "CreateResource":"ECM_EMR",
                    "CreateTime":1542953603000,
                    "FailReason":{},
                    "HasUncompletedOrder":false,
                    "Id":"C-4042079DFF01A3F5",
                    "Name":"zx-presto-1",
                    "OrderTaskInfo":{},
                    "RunningTime":1322,
                    "Status":"IDLE",
                    "Type":"HADOOP"
                },
                {
                    "ChargeType":"PostPaid",
                    "CreateResource":"ECM_EMR",
                    "CreateTime":1542950611000,
                    "FailReason":{},
                    "HasUncompletedOrder":false,
                    "Id":"C-3D0B53253595DBB0",
                    "Name":"zx-presto-213",
                    "OrderTaskInfo":{},
                    "RunningTime":4314,
                    "Status":"IDLE",
                    "Type":"HADOOP"
                }
            ]
        },
        "PageNumber":1,
        "PageSize":10,
        "TotalCount":2
    }
    ```


## Error codes {#section_fcj_yxl_2gb .section}

