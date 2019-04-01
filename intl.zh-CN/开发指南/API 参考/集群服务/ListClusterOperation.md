# ListClusterOperation {#concept_qrc_12m_2gb .concept}

查询集群的操作历史列表

## 请求参数 {#section_obj_yxl_2gb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|ClusterId|String|是|C-EBD62A703A430E23|集群 ID|
|RegionId|String|是|cn-hangzhou|集群对应的区域 ID|
|PageNumber|Integer|否|1|分页查询的查询页码|
|PageSize|Integer|否|100|分页查询的每页记录数|
|ServiceName|String|否|YARN|服务名称|
|Status|String|否|SUCCESS|状态|
|AccessKeyId|String|否|LTAI8ljWyu7y\*\*\*\*|阿里云 AccessKey ID 信息|

## 返回参数 {#section_vbj_yxl_2gb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|DF202AC2-5D5D-4288-B608-B7B1595B5C7C|请求 ID|
|TotalCount|Integer|1|记录总数|
|PageNumber|Integer|1|当前的页号|
|PageSize|Integer|100|每页的记录数|
|ClusterOperationList| | |操作历史列表|
|OperationId|String|1111|操作 ID|
|OperationName|String|CREATE|操作名称|
|StartTime|String|1543312720000|操作的开始执行时间|
|Duration|String|500|持续时间|
|Status|String|SUCCESS|操作的当前执行状态|
|Percentage|String|100|执行进度（百分比）|
|Comment|String|Create cluster|操作的备注信息|

## 示例 {#section_dcj_yxl_2gb .section}

-   请求示例

    ```
    /?ClusterId=C-F32FB31D82954C64
    &RegionId=cn-hangzhou
    &AccessKeyId=LTAI8ljWyu7y****
    &PageNumber=1
    &PageSize=100
    &ServiceName=YARN
    &Status=SUCCESS
    &<公共请求参数>
    ```

-   正常返回示例

    JSON 格式

    ```
    {
    	"ClusterOperationList":{
    		"ClusterOperation":[{
    			"Comment":"Create cluster",
    			"Duration":"303",
    			"OperationId":"41027",
    			"OperationName":"CREATE",
    			"Percentage":"100",
    			"StartTime":"1543312720000",
    			"Status":"SUCCESS"
    		}]
    	},
    	"PageNumber":1,
    	"PageSize":10,
    	"RequestId":"FB29CC76-D7DC-4A08-AE96-E1F96F9FFC4E",
    	"TotalCount":1
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


## 错误码 {#section_fcj_yxl_2gb .section}

[查看本产品错误码](https://error-center.alibabacloud.com/status/product/Emr)

