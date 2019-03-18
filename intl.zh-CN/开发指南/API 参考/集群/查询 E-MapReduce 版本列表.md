# 查询 E-MapReduce 版本列表 {#concept_ur3_mcy_dgb .concept}

查询 E-MapReduce 版本列表参数以及示例。

## 请求参数 {#section_pdw_lv4_dgb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|RegionId|String|是|cn-hangzhou|区域|
|AccessKeyId|String|否|LTAxxxxxxxxxxxxx|AccessKeyId|
|EmrVersion|String|否|EMR-3.15.0|EMR 版本|
|PageNumber|Integer|否|1|分页页数，从 1 开始|
|PageSize|Integer|否|10|分页大小|
|StackName|String|否|EMR|软件栈名字：EMR|
|StackVersion|String|否|3.15.1.1.0|软件栈版本|

## 返回参数 {#section_vdw_lv4_dgb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|BF4FBAC6-B03E-4BFB-B6DB-EB53C34F2E22|请求 ID|
|TotalCount|Integer|12|查询总数|
|PageNumber|Integer|1|分页页数|
|PageSize|Integer|10|分页大小|
|EmrMainVersionList| | |EMR 版本列表|
|RegionId|String|cn-hangzhou|区域|
|EmrVersion|String|EMR-3.16.0|EMR 版本号|
|EcmVersion|Boolean|true|是否软件栈版本号|
|ImageId|String|m-bp1hdwryap03nxpl1rcd|镜像 ID|
|Display|Boolean|true|是否开放给用户|
|StackName|String|“ ”|保留字段|
|StackVersion|String|“ ”|保留字段|
|ClusterTypeInfoList| | |集群类型列表|
|ClusterType|String|HADOOP|集群类型|
|ServiceInfoList| | |服务列表|
|ServiceName|String|HIVE|服务名|
|ServiceDisplayName|String|HIVE|服务展示名|
|ServiceVersion|String|2.3.3.0.1|服务内部版本|
|ServiceDisplayVersion|Integer|2.3.3|服务展示版本|
|Mandatory|Boolean|true|是否必填|
|Display|Boolean|true|是否展示|
|WhiteUserList|String|""|保留字段|

## 示例 {#section_ydw_lv4_dgb .section}

-   请求示例

    ```
    /?RegionId=cn-hangzhou
    &AccessKeyId=LTAxxxxxxxxxxxxx
    &EmrVersion=EMR-3.15.0
    &PageNumber=1
    &PageSize=10
    &StackName=EMR
    &StackVersion=3.15.1.1.0
    &<公共请求参数>
    ```

-   正常返回示例

    JSON 格式

    ```
    {
    	"emrMainVersionList":[
    		{
    			"clusterTypeInfoList":[],
    			"display":false,
    			"ecmVersion":true,
    			"emrVersion":"EMR-3.16.0",
    			"imageId":"m-bp1amnkvohr8eb6wud0f",
    			"regionId":"cn-hangzhou",
    			"stackName":"EMR",
    			"stackVersion":"3.16.0.1.2",
    			"whiteUserList":[]
    		},
    		{
    			"clusterTypeInfoList":[],
    			"display":false,
    			"ecmVersion":true,
    			"emrVersion":"EMR-3.16.1",
    			"imageId":"m-bp1amnkvohr8eb6wud0f",
    			"regionId":"cn-hangzhou",
    			"stackName":"EMR",
    			"stackVersion":"3.16.0.1.3",
    			"whiteUserList":[]
    		},
    		{
    			"clusterTypeInfoList":[],
    			"display":false,
    			"ecmVersion":true,
    			"emrVersion":"EMR-3.15.1",
    			"imageId":"m-bp1ddn41swc2fqjgj3ik",
    			"regionId":"cn-hangzhou",
    			"stackName":"EMR",
    			"stackVersion":"3.15.1.1.0",
    			"whiteUserList":[]
    		},
    		{
    			"clusterTypeInfoList":[],
    			"display":false,
    			"ecmVersion":true,
    			"emrVersion":"EMR-3.15.2",
    			"imageId":"m-bp1hdwryap03nxpl1rcd",
    			"regionId":"cn-hangzhou",
    			"stackName":"EMR",
    			"stackVersion":"3.15.1.1.0",
    			"whiteUserList":[]
    		},
    		{
    			"clusterTypeInfoList":[],
    			"display":false,
    			"ecmVersion":true,
    			"emrVersion":"EMR-3.15.3",
    			"imageId":"m-bp1hdwryap03nxpl1rcd",
    			"regionId":"cn-hangzhou",
    			"stackName":"EMR",
    			"stackVersion":"3.15.1.1.0",
    			"whiteUserList":[]
    		}
    	],
    	"pageNumber":1,
    	"pageSize":5,
    	"requestId":"D835118F-3198-461E-B06E-7A73C4353F59",
    	"totalCount":140
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

