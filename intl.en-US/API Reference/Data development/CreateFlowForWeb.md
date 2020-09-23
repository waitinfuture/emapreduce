# CreateFlowForWeb

Call CreateFlowForWeb to create a workflow.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=Emr&api=CreateFlowForWeb&type=RPC&version=2016-04-08)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|CreateFlowForWeb|The operation that you want to perform. For API requests using the HTTP or HTTPS URL, this parameter is required. Set the value to CreateFlowForWeb. |
|ClusterId|String|Yes|C-A23BD131A862\*\*\*\*|The ID of the PolarDB cluster. You can call [ListClusters](~~28147~~) view the cluster ID. |
|CreateCluster|Boolean|Yes|false|Specifies whether to create a cluster through the cluster template. If this parameter is set to true, a cluster is created through the cluster template. The ID of the cluster must be the ID of the cluster template.\(CT-xxx\) otherwise, it is the existing cluster ID\(C-xxx\). |
|Description|String|Yes|This is the description of a workflow|The description of the workflow. |
|Name|String|Yes|my\_flow\_demo|The name of the workflow. |
|ProjectId|String|Yes|FP-257A173659F5\*\*\*\*|The ID of the project. You can call [ListFlowProject](~~101204~~) view the project ID. |
|RegionId|String|Yes|cn-hangzhou|The region ID of the resource. You can call the [DescribeRegions](~~25609~~) operation to query the most recent region list. |
|StartSchedule|Long|No|1538017814000|The start time of scheduling. It is a timestamp whose data type is long. Example: System.currentTimeMillis \(\) .

 -   It must be less than EndSchedule.
-   It is required when CronExpr is not empty. |
|EndSchedule|Long|No|1538018814000|The end time of the scheduling of the workflow. It is a timestamp whose data type is long. Example: System.currentTimeMillis.\(\) . |
|CronExpr|String|No|0 0 0-23/1 \* \* ?|The cron expression of the time-based scheduling of the workflow. |
|HostName|String|No|emr-header-1.cluster-123456|The name of the host on which the node instance runs. You can call [ListFlow](~~101073~~) or log on to the host to use`hostname` command. |
|Graph|String|No|\{"nodes":\[\{"id":"48d474ea","index":0,"spmAnchorId":"0.0.0.i0.766645eb2cmNtQ","attribute":\{"type":"START"\},"shape":"startControlNode","type":"node","y":250,"size":"80\*34","x":500\},\{"id":"7ba480b3","index":1,"spmAnchorId":"5176.8250060.0.i19.771e28d0IPNQGE","attribute":\{"jobType":"SHELL","jobId":"FJ-7BE1062897B19D25","type":"JOB"\},"config":\{"hostName":""\},"label":"fail\_job","shape":"shellJobNode","type":"node","y":398.5,"size":"170\*34","x":470.5\},\{"id":"33202d60","index":2,"spmAnchorId":"5176.8250060.0.i23.771e28d0IPNQGE","attribute":\{"type":"END"\},"shape":"endControlNode","type":"node","y":562.5,"size":"80\*34","x":430.5\}\],"edges":\[\{"id":"28167ea0","index":3,"source":"48d474ea","sourceAnchor":0,"target":"7ba480b3","targetAnchor":0\},\{"id":"e8d5ff52","index":4,"source":"7ba480b3","sourceAnchor":1,"target":"33202d60","targetAnchor":0\}\]\}|DAG graphic information. |
|AlertConf|String|No|\{"items":\[\{"enable":true,"eventId":"EMR-210401001","alertUserGroupIdList":\["AUG-b79bb29bb6e14ddd89674a242623851b"\],"alertDingDingGroupList":\["ADG-af1f9689d6194e2dbd89927d5c515172"\]\},\{"enable":true,"eventId":"EMR-110401015","alertUserGroupIdList":\["AUG-b79bb29bb6e14ddd89674a242623851b"\],"alertDingDingGroupList":\["ADG-af1f9689d6194e2dbd89927d5c515172"\]\},\{"enable":true,"eventId":"EMR-110401002","alertUserGroupIdList":\["AUG-b79bb29bb6e14ddd89674a242623851b"\],"alertDingDingGroupList":\["ADG-af1f9689d6194e2dbd89927d5c515172"\]\}\]\}|The alert notification configurations. eventId can be set to the following alerts:

 -   EMR-210401001: indicates that the workflow instance execution failed.
-   EMR-110401002: indicates that the workflow instance execution succeeded.
-   EMR-110401015: indicates that a workflow node failed. |
|AlertUserGroupBizId|String|No|-|A deprecated parameter. |
|AlertDingDingGroupBizId|String|No|-|A deprecated parameter. |
|ParentFlowList|String|No|F-62ECFC6E1BF6EAD2,F-1E6528634E67B615,F-7E0A84332E9D9A89|The parent workflows that the created workflow relies on. Separate the workflows with commas \(,\). You can call [ListFlowInstance](~~101166~~) view the workflow ID. |
|ParentCategory|String|No|FC-F2495319DA05\*\*\*\*|The ID of the parent directory. You can call [DescribeFlowCategory](~~100977~~) view. If it is empty, the default is the root directory. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Id|String|F-7A39731FE719\*\*\*\*|The ID of the workflow. |
|RequestId|String|243D5A48-96A5-4C0C-8966-93CBF65635ED|The ID of the request. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=CreateFlowForWeb
&Description=This is the description of a created workflow
&Name=my_flow_demo
&ProjectId=FP-257A173659F5****
&RegionId=cn-hangzhou
&<Common request parameters>
```

Sample success responses

`XML` format

```
<RequestId>2670BCFB-925D-4C3E-9994-8D12F7A9F538</RequestId>
<Id>F-7A39731FE719****</Id>
```

`JSON` format

```
{
    "RequestId": "2670BCFB-925D-4C3E-9994-8D12F7A9F538",
    "Id": "F-7A39731FE719****"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/Emr).

