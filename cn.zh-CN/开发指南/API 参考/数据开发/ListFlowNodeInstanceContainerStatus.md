# ListFlowNodeInstanceContainerStatus {#concept_stk_jqk_ggb .concept}

调用 ListFlowNodeInstanceContainerStatus 接口查询节点实例的容器状态详情

## 请求参数 {#section_obj_yxl_2gb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|NodeInstanceId|String|是|FNI-FE4BD156E939CF1F|节点实例 ID|
|ProjectId|String|是|7A1018ADE9179EE1|项目 ID|
|RegionId|String|是|cn-hangzhou|区域 ID|
|PageNumber|Integer|否|1|页码|
|PageSize|Integer|否|20|每页查询数量|

## 返回参数 {#section_vbj_yxl_2gb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|E82E504E-F1A8-4D05-AA8F-360BD9DF423A|请求 ID|
|PageNumber|Integer|1|页码|
|PageSize|Integer|20|每页数量|
|Total|Integer|1|总数|
|ContainerStatusList| | |容器状态列表|
|ApplicationId|String|application\_1542333181298\_0016|Application ID|
|ContainerId|String|container\_1542333181298\_0016\_01\_000015|Container ID|
|HostName|String|192.168.128.53|机器 IP 名称|
|Status|String|KILLED|容器状态|

## 示例 {#section_dcj_yxl_2gb .section}

-   请求示例

    ```
    
    /?Action=ListFlowNodeInstanceContainerStatus
    &NodeInstanceId=FNI-FE4BD156E939CF1F
    &ProjectId=7A1018ADE9179EE1
    &RegionId=cn-hangzhou
    &PageNumber=1
    &PageSize=20
    &<公共请求参数>
    ```

-   正常返回示例

    JSON 格式

    ```
    {
    	"ContainerStatusList":{
    		"ContainerStatus":[{
    			"ApplicationId":"application_1542333181298_0016",
    			"ContainerId":"container_1542333181298_0016_01_000015",
    			"HostName":"192.168.128.53",
    			"Status":"KILLED"
    		}]
    	},
    	"PageNumber":1,
    	"PageSize":200,
    	"RequestId":"E82E504E-F1A8-4D05-AA8F-360BD9DF423A",
    	"Total":1
    }
    ```

-   异常返回示例

    JSON 格式

    ```
    {
    	"code":"FLOW_API_FAILED",
    	"message":"flow node instance does not exist [FNI-AFF866DB3A0BB0133]",
    	"requestId":"B759F5A4-1ABF-42F0-9496-43286048D4BE",
    	"successResponse":false
    }
    ```


## 错误码 {#section_fcj_yxl_2gb .section}

[查看本产品错误码](https://error-center.alibabacloud.com/status/product/Emr)

