# ListFlowClusterAllHosts {#concept_yqv_snj_ggb .concept}

查询给定集群可在项目设置中设置到白名单中的主机列表， 目前支持 master 和gateway 节点

## 请求参数 {#section_obj_yxl_2gb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|RegionId|String|是|cn-hangzhou|区域 ID|
|ClusterId|String|是|C-F32FB31D82954C64|集群 ID|
|ProjectId|String|是|FP-3535FE0BE5224A47|项目 ID|

## 返回参数 {#section_vbj_yxl_2gb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|2E429517-6A50-4082-9A30-47755CE9F9A5|请求 ID|
|HostList| | |客户端列表|
|HostId|String|119555|机器 ID|
|HostName|String|emr-header-1.cluster-500159692|机器名称|
|PublicIp|String|192.0.0.1|公网 IP|
|PrivateIp|String|127.0.0.1|内网 IP|
|Role|String|CORE|角色|
|InstanceType|String|ecs.n4.xlarge|实例类型|
|Cpu|Integer|4|核数|
|Memory|Integer|8|内存, 单位为 G|
|Status|String|STARTING|状态|
|Type|String|VM|类型|
|HostInstanceId|String|i-bp1agi4417dx08sig82j|机器实例 ID|
|SerialNumber|String|6dc097b0-5e92-4e08-a49e-37c26c78cf9f|序列号|

## 示例 {#section_dcj_yxl_2gb .section}

-   请求示例

    ```
    
    /?Action=ListFlowClusterAllHosts
    ClusterId=C-F32FB31D82954C64
    &ProjectId=FP-3535FE0BE5224A47
    &RegionId=cn-hangzhou
    &<公共请求参数>
    ```

-   正常返回示例

    JSON 格式

    ```
    {
    	"HostList":{
    		"Host":[{
    			"Cpu":4,
    			"HostId":"119555",
    			"HostInstanceId":"i-bp1agi4417dx08sig82j",
    			"HostName":"emr-header-1.cluster-500159692",
    			"Memory":8,
    			"PrivateIp":"192.168.100.251",
    			"PublicIp":"47.99.183.218",
    			"Role":"MASTER",
    			"SerialNumber":"738d0af9-6ca3-46dd-a973-bab70da4bf74",
    			"Status":"STARTING"
    		}]
    	},
    	"RequestId":"2E429517-6A50-4082-9A30-47755CE9F9A5"
    }
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

