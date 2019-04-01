# ListFlow {#concept_zpy_xnc_ggb .concept}

查询工作流列表参数及示例

## 请求参数 {#section_obj_yxl_2gb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|ClusterId|String|否|C-F32FB31D82954C64|集群 ID|
|Id|String|否|F-A32FB31D82954C64|工作流 ID|
|RegionId|String|是|cn-hangzhou|区域 ID|
|JobId|String|否|FJ-F32FB31D82954C64|作业 ID|
|Name|String|否|my\_flow|工作流名称|
|PageNumber|Integer|否|1|页码|
|PageSize|Integer|否|20|每页查询数量|
|Periodic|Boolean|否|true|是否调度|
|ProjectId|String|否|FP-3535FE0BE5224A47|项目 ID|
|Status|String|否|STOP\_SCHEDULE|状态：STOP\_SCHEDULE\(停止调度\)，UNDER\_SCHEDULE\(调度中\)|

## 返回参数 {#section_vbj_yxl_2gb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|7DDFF4C7-3AE3-485F-BFA1-BAE0AA3689DD|请求 ID|
|PageNumber|Integer|1|页码|
|PageSize|Integer|20|每页数量|
|Total|Integer|1|总数|
|Flow| | |工作流列表|
|Id|String|7A39731FE7196358|工作流 ID|
|GmtCreate|Long|1541561048000|创建时间|
|GmtModified|Long|1542902401000|修改时间|
|Name|String|my\_flow\_demo|工作流名称|
|Description|String|my flow demo|工作流描述|
|Type|String|EMR|目前只支持 EMR|
|Status|String|UNDER\_SCHEDULE|状态, 支持 STOP\_SCHEDULE、UNDER\_SCHEDULE|
|Periodic|Boolean|true|是否周期调度|
|StartSchedule|Long|1541561443000|开始调度时间|
|EndSchedule|Long|4102416001000|调度结束时间|
|CronExpr|String|0 0 0 \* \* ?|调度 cron 时间表达式|
|CreateCluster|Boolean|false|是否通过集群模板创建集群|
|ClusterId|String|C-F32FB31D82954C64|集群 ID|
|ProjectId|String|FP-3535FE0BE5224A47|项目 ID|
|HostName|String|emr-header-1.cluster-123456|指定运行的机器信息, 格式为 emr-header-1，cluster-123456|
|Graph|String|\{"nodes":\[\{"id":"48d474ea","index":0,"spmAnchorId":"0.0.0.i0.766645eb2cmNtQ","attribute":\{"type":"START"\},"shape":"startControlNode","type":"node","y":250,"size":"80\*34","x":500\},\{"id":"7ba480b3","index":1,"spmAnchorId":"5176.8250060.0.i19.771e28d0IPNQGE","attribute":\{"jobType":"SHELL","jobId":"FJ-7BE1062897B19D25","type":"JOB"\},"config":\{"hostName":""\},"label":"fail\_job","shape":"shellJobNode","type":"node","y":398.5,"size":"170\*34","x":470.5\},\{"id":"33202d60","index":2,"spmAnchorId":"5176.8250060.0.i23.771e28d0IPNQGE","attribute":\{"type":"END"\},"shape":"endControlNode","type":"node","y":562.5,"size":"80\*34","x":430.5\}\],"edges":\[\{"id":"28167ea0","index":3,"source":"48d474ea","sourceAnchor":0,"target":"7ba480b3","targetAnchor":0\},\{"id":"e8d5ff52","index":4,"source":"7ba480b3","sourceAnchor":1,"target":"33202d60","targetAnchor":0\}\]\}|图形信息|
|AlertUserGroupBizId|String|-|已废弃|
|AlertDingDingGroupBizId|String|-|已废弃|
|CategoryId|String|FC-2CF58396A18923B6|目录 ID|

## 示例 {#section_dcj_yxl_2gb .section}

-   请求示例

    ```
    
    /?Action=ListFlow
    &RegionId=cn-hangzhou
    &ClusterId=C-F32FB31D82954C64
    &Id=F-A32FB31D82954C64
    &JobId=FJ-F32FB31D82954C64
    &Name=my_flow
    &PageNumber=1
    &PageSize=20
    &Periodic=true
    &ProjectId=FP-3535FE0BE5224A47
    &Status=STOP_SCHEDULE
    &<公共请求参数>
    ```

-   正常返回示例

    JSON 格式

    ```
    {
    	"Flow":{
    		"Flow":[{
    			"CategoryId":"FC-2CF58396A18923B6",
    			"ClusterId":"C-F32FB31D82954C64",
    			"CreateCluster":false,
    			"CronExpr":"0 0 0 * * ?",
    			"Description":"daily_0",
    			"EndSchedule":4102416001000,
    			"GmtCreate":1541561048000,
    			"GmtModified":1542902401000,
    			"HostName":"",
    			"Id":"F-D156B45E06F1CA8E",
    			"Name":"daily_0",
    			"Periodic":true,
    			"ProjectId":"FP-3535FE0BE5224A47",
    			"StartSchedule":1541561443000,
    			"Status":"UNDER_SCHEDULE",
    			"Type":"EMR"
    		}]
    	},
    	"PageNumber":1,
    	"PageSize":500,
    	"RequestId":"7DDFF4C7-3AE3-485F-BFA1-BAE0AA3689DD",
    	"Total":1
    }
    &<公共请求参数>
    ```

-   异常返回示例

    JSON 格式

    ```
    {
    	"code":"FLOW_API_FAILED",
    	"message":"Project does not exist [FP-3535FE0BE5224A4]",
    	"requestId":"93F6F3CF-B2AE-411A-948F-0F7293058CF5",
    	"successResponse":false
    }
    ```


## 错误码 {#section_fcj_yxl_2gb .section}

[查看本产品错误码](https://error-center.alibabacloud.com/status/product/Emr)

