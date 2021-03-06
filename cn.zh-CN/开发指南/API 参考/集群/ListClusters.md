# ListClusters {#concept_rn1_cgb_kfb .concept}

分页查询集群列表参数以及示例。

## 请求参数 {#section_pdw_lv4_dgb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|RegionId|String|是|cn-hangzhou|区域|
|AccessKeyId|String|否|LTAI8ljWyu7y\*\*\*\*|AccessKeyId|
|ClusterTypeList.N|RepeatList|否|\["HADOOP","KAFKA"\]|集群类型列表|
|CreateType|String|否|ON-DEMAND|集群创建类型。可选值：ON-DEMAND，MANUAL|
|DefaultStatus|Boolean|否|true|是否查询默认状态\(初始化中、等待构建、空闲、运行中、释放中、创建失败\)的集群|
|IsDesc|Boolean|否|false|是否倒序排列|
|PageNumber|Integer|否|1|分页分数，从 1 开始|
|PageSize|Integer|否|10|分页大小|
|StatusList.N|RepeatList|否|\["CREATING","IDLE"\]|状态列表|

## 返回参数 {#section_vdw_lv4_dgb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|BF4FBAC6-B03E-4BFB-B6DB-EB53C34F2E22|请求 ID|
|TotalCount|Integer|12|查询总数|
|PageNumber|Integer|1|分页页数|
|PageSize|Integer|10|分页大小|
|Clusters| | |集群列表|
|Id|String|C-010A704DA760072A|集群 ID|
|Name|String|cluster\_name|集群名|
|Type|String|HADOOP|集群类型|
|CreateTime|Long|1542784048000|创建时间|
|RunningTime|Integer|2345|已运行时间\(秒\)|
|Status|String|IDEL|集群状态|
|ChargeType|String|PostPaid|付费类型|
|ExpiredTime|Long|1542784048000|包年包月集群超时时间|
|Period|Integer|10|包年包月时长（天）|
|HasUncompletedOrder|Boolean|false|是否有未完成的订单|
|OrderList|String|0|订单列表|
|CreateResource|String|ECM\_EMR|ECM\_EMR|
|OrderTaskInfo| | |保留字段，订单任务信息|
|TargetCount|Integer|0|保留字段|
|CurrentCount|Integer|0|保留字段|
|OrderIdList|String|0|保留字段|
|FailReason| | |创建失败原因|
|ErrorCode|String|InvalidImageId.NotFound|错误码|
|ErrorMsg|String|The specified ImageId does not exist.|错误信息|
|RequestId|String|B8DC3A91-3953-4444-92BB-DBC29C47EC1A|请求 ID|

## 示例 {#section_ydw_lv4_dgb .section}

-   请求示例

    ```
    /?RegionId=cn-hangzhou
    &AccessKeyId=LTAI8ljWyu7y****
    &ClusterTypeList.1=["HADOOP","KAFKA"]
    &CreateType=ON-DEMAND
    &DefaultStatus=
    &IsDesc=false
    &PageNumber=1
    &PageSize=10
    &StatusList.1=["CREATING","IDLE"]
    &<公共请求参数>
    ```

-   正常返回示例

    JSON 格式

    ```
    {
    	"clusters":[
    		{
    			"chargeType":"PostPaid",
    			"createResource":"ECM_EMR",
    			"createTime":1542784048000,
    			"failReason":{},
    			"hasUncompletedOrder":false,
    			"id":"C-010A704DA760072A",
    			"name":"jy_test_d1_test",
    			"orderTaskInfo":{},
    			"runningTime":3138,
    			"status":"RELEASED",
    			"type":"HADOOP"
    		},
    		{
    			"chargeType":"PostPaid",
    			"createResource":"ECM_EMR",
    			"createTime":1538107586000,
    			"failReason":{},
    			"hasUncompletedOrder":false,
    			"id":"C-B9712209060CAB62",
    			"name":"intelligence-313-yp",
    			"orderTaskInfo":{},
    			"runningTime":2069400,
    			"status":"RELEASED",
    			"type":"HADOOP"
    		},
    		{
    			"chargeType":"PostPaid",
    			"createResource":"ECM_EMR",
    			"createTime":1536546078000,
    			"failReason":{},
    			"hasUncompletedOrder":false,
    			"id":"C-4CD9EBBD6B23C81E",
    			"name":"mg-storm",
    			"orderTaskInfo":{},
    			"runningTime":1382155,
    			"status":"RELEASED",
    			"type":"HADOOP"
    		},
    		{
    			"chargeType":"PostPaid",
    			"createResource":"ECM_EMR",
    			"createTime":1535363759000,
    			"failReason":{},
    			"hasUncompletedOrder":false,
    			"id":"C-75D6EE95D722E5CA",
    			"name":"df-3101-upgrade-test",
    			"orderTaskInfo":{},
    			"runningTime":676284,
    			"status":"RELEASED",
    			"type":"HADOOP"
    		},
    		{
    			"chargeType":"PostPaid",
    			"createResource":"ECM_EMR",
    			"createTime":1534492361000,
    			"failReason":{
    				"errorCode":"InvalidImageId.NotFound",
    				"errorMsg":"The specified ImageId does not exist.",
    				"requestId":"B8DC3A91-3953-4444-92BB-DBC29C47EC1A"
    			},
    			"hasUncompletedOrder":false,
    			"id":"C-5EF0F3C257B16CB7",
    			"name":"测试集群1",
    			"orderTaskInfo":{},
    			"runningTime":0,
    			"status":"CREATE_FAILED",
    			"type":"HADOOP"
    		}
    	],
    	"pageNumber":1,
    	"pageSize":5,
    	"requestId":"5443DB14-4641-4DFF-9226-4888EC5A2EA9",
    	"totalCount":11
    }
    ```

-   异常返回示例

    JSON 格式

    ```
    {
    	"code":"RAM.Permission.NotAllow",
    	"message":"It is not allow to execute this operation,please use RAM to authorize!",
    	"requestId":"9AEDC439-1F63-491D-B8C6-9737C372BF3A",
    	"successResponse":false
    }
    ```


## 错误码 {#section_a2w_lv4_dgb .section}

[查看本产品错误码](https://error-center.alibabacloud.com/status/product/Emr)

