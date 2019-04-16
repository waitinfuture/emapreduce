# ListClusterOperationHost {#concept_prp_3gm_2gb .concept}

调用 ListClusterOperationHost 接口查询操作历史的操作机器列表

## 请求参数 {#section_obj_yxl_2gb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|ClusterId|String|是|C-EBD62A703A430E23|集群 ID|
|OperationId|String|是|111|操作历史 ID|
|RegionId|String|是|cn-hangzhou|集群对应的区域 ID|
|PageNumber|Integer|否|1|分页查询的查询页码|
|PageSize|Integer|否|100|分页查询的每页记录数|
|ServiceName|String|否|YARN|服务名称|
|Status|String|否|SUCCESS|状态|
|AccessKeyId|String|否|LTAI8ljWyu7y\*\*\*\*|阿里云AccessKey ID信息|

## 返回参数 {#section_vbj_yxl_2gb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|DF202AC2-5D5D-4288-B608-B7B1595B5C7C|请求 ID|
|TotalCount|Integer|1|记录总数|
|PageNumber|Integer|1|当前的页号|
|PageSize|Integer|100|每页的记录数|
|ClusterOperationHostList| | |操作历史对应的主机信息列表|
|HostId|String|111|主机 ID|
|HostName|String|emr-worker-1|主机名|
|Status|String|SUCCESS|执行状态|
|Percentage|String|100|执行进度（百分比）|

## 示例 {#section_dcj_yxl_2gb .section}

-   请求示例

    ```
    /?ClusterId=DF202AC2-5D5D-4288-B608-B7B1595B5C7C
    &OperationId=111
    &RegionId=cn-hangzhou
    &AccessKeyId=LTAI8ljWyu7y****
    &PageNumber=1
    &PageSize=100
    &Status=SUCCESS
    &<公共请求参数>
    ```

-   正常返回示例

    JSON 格式

    ```
    {
    	"ClusterOperationHostList":{
    		"ClusterOperationHost":[
    			{
    				"HostId":"124903",
    				"HostName":"emr-header-1",
    				"Percentage":"100",
    				"Status":"SUCCESS"
    			},
    			{
    				"HostId":"124902",
    				"HostName":"emr-worker-1",
    				"Percentage":"100",
    				"Status":"SUCCESS"
    			},
    			{
    				"HostId":"124901",
    				"HostName":"emr-worker-2",
    				"Percentage":"100",
    				"Status":"SUCCESS"
    			}
    		]
    	},
    	"PageNumber":1,
    	"PageSize":500,
    	"RequestId":"12B58AF8-40E0-42E9-B6E5-1F4201AFB4F8",
    	"TotalCount":3
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

