# ListClusterServiceConfigHistory {#concept_xzv_kxr_2gb .concept}

调用 ListClusterServiceConfigHistory 接口查询集群指定服务的配置修改历史信息的接口

## 请求参数 {#section_obj_yxl_2gb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|ClusterId|String|是|C-F32FB31D82954C64|集群 ID|
|ServiceName|String|否|TEZ|服务名称|
|RegionId|String|是|cn-hangzhou|区域 ID|
|ConfigVersion|String|否|0|配置的版本|
|PageNumber|Integer|否|1|分页查询的查询页码|
|PageSize|Integer|否|100|分页查询的每页记录数|
|AccessKeyId|String|是|LTAI8ljWyu7y\*\*\*\*|阿里云 AccessKey ID 信息|

## 返回参数 {#section_vbj_yxl_2gb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|DF202AC2-5D5D-4288-B608-B7B1595B5C7C|请求 ID|
|TotalCount|Integer|30|记录总数|
|PageNumber|Integer|1|当前的页号|
|PageSize|Integer|100|每页的记录数|
|ConfigHistoryList| | |配置历史列表|
|ServiceName|String|TEZ|服务名称|
|ConfigVersion|String|15433205029440|配置版本，一般为修改的时间戳|
|ConfigFileName|String|tez-site|配置文件名|
|ConfigItemName|String|tez.am.resource.memory.mb|配置项名称|
|NewValue|String|1280|修改后的值|
|OldValue|String|640|修改前的值|
|Applied|Boolean|true|是否生效|
|CreateTime|Long|1543320503000|操作时间|
|Author|String|1111|修改人|
|Comment|String|modifyConfig|本次修改的备注信息|

## 示例 {#section_dcj_yxl_2gb .section}

-   请求示例

    ```
    /?ClusterId=C-F32FB31D82954C64
    &RegionId=cn-hangzhou
    &AccessKeyId=LTAI8ljWyu7y****
    &ConfigVersion=0
    &PageNumber=1
    &PageSize=100
    &ServiceName=TEZ
    &<公共请求参数>
    ```

-   正常返回示例

    JSON 格式

    ```
    {
    	"ConfigHistoryList":{
    		"ConfigHistory":[{
    			"Applied":true,
    			"Author":****,
    			"Comment":"xxx",
    			"ConfigFileName":"tez-site",
    			"ConfigItemName":"tez.am.resource.memory.mb",
    			"ConfigVersion":"1543320502944",
    			"CreateTime":1543320503000,
    			"NewValue":"1280",
    			"OldValue":"640",
    			"ServiceName":"TEZ"
    		}]
    	},
    	"PageNumber":1,
    	"PageSize":500,
    	"RequestId":"8CC97161-4A0C-4EDB-9799-F0A51C40B070",
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

