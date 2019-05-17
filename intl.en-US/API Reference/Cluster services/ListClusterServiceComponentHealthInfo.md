# ListClusterServiceComponentHealthInfo {#concept_gmh_vwm_2gb .concept}

You can call this operation to view the health information of the components of the specified service on a cluster.

## Request parameters {#section_obj_yxl_2gb .section}

|Name|Type|Required|Example|Description|
|----|----|--------|-------|-----------|
|ClusterId|String|Yes|C-F32FB31D82954C64|The ID of the cluster.|
|ServiceName|String|No|TEZ|The name of the service.|
|RegionId|String|是|cn-hangzhou|RegionID|
|AccessKeyId|String|否|xxx|阿里云AccessKey ID信息|

## 返回参数 {#section_vbj_yxl_2gb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|DF202AC2-5D5D-4288-B608-B7B1595B5C7C|请求ID|
|ClusterId|String|C-F32FB31D82954C64|集群ID|
|HealthInfoList| | |The list of health information.|
|ServiceName|String|YARN|服务名称|
|ComponentName|String|ResourceManger|The name of the component.|
|HealthLevel|String|NORMAL|The level of health.|
|StoppedNum|Integer|0|The number of stopped components.|
|ManualStoppedNum|Integer|0|The number of components that are manually stopped.|
|NormalNum|Integer|7|The number of normal components.|
|TotalNum|Integer|7|总数|
|AgentHeartBeatLostNum|Integer|0|The number of hosts on which the agents have lost heartbeats.|
|HealthDetailList| | |The list of health details.|
|code|String|200|The error code.|
|HealthRuleParam| | |The parameter of the health rule.|
|Service|String|YARN|服务名称|
|Component|String|Ecm-Agent|The name of the component.|
|RuleTitle|String|AgentHeartBeatCheck|The title of the health rule.|
|Pass|String|""|The information about the passed check.|
|RuleId|String|111|The ID of the health rule.|
|RuleDescription|String|Agent monitoring|The description of the rule.|
|HostNames|String|emr-worker-1|The hostname.|

## 示例 {#section_dcj_yxl_2gb .section}

-   请求示例

    ```
    /? ClusterId=C-F32FB31D82954C64
    &RegionId=ccn-hangzhou
    &AccessKeyId=111
    &ServiceName=TEZ
    &<公共请求参数>
    ```

-   正常返回示例

    JSON 格式

    ```
    {
    	"ClusterId":"C-F7CBF0B43D983FDB",
    	"HealthInfoList":{
    		"HealthInfo":[
    			{
    				"AgentHeartBeatLostNum":0,
    				"HealthDetailList":{
    					"HealthDetail":[]
    				},
    				"HealthLevel":"NORMAL",
    				"ManualStoppedNum":0,
    				"NormalNum":7,
    				"ServiceName":"EMR-MONITOR",
    				"StoppedNum":0,
    				"TotalNum":7
    			},
    			{
    				"AgentHeartBeatLostNum":0,
    				"HealthDetailList":{
    					"HealthDetail":[]
    				},
    				"HealthLevel":"NORMAL",
    				"ManualStoppedNum":0,
    				"NormalNum":8,
    				"ServiceName":"GANGLIA",
    				"StoppedNum":0,
    				"TotalNum":8
    			},
    			{
    				"AgentHeartBeatLostNum":0,
    				"HealthDetailList":{
    					"HealthDetail":[]
    				},
    				"HealthLevel":"NORMAL",
    				"ManualStoppedNum":0,
    				"NormalNum":4,
    				"ServiceName":"EMRDOCTOR",
    				"StoppedNum":0,
    				"TotalNum":4
    			},
    			{
    				"AgentHeartBeatLostNum":0,
    				"HealthDetailList":{
    					"HealthDetail":[]
    				},
    				"HealthLevel":"NORMAL",
    				"ManualStoppedNum":0,
    				"NormalNum":1,
    				"ServiceName":"MYSQL",
    				"StoppedNum":0,
    				"TotalNum":1
    			},
    			{
    				"AgentHeartBeatLostNum":0,
    				"HealthDetailList":{
    					"HealthDetail":[]
    				},
    				"HealthLevel":"NORMAL",
    				"ManualStoppedNum":0,
    				"NormalNum":3,
    				"ServiceName":"SQOOP",
    				"StoppedNum":0,
    				"TotalNum":3
    			},
    			{
    				"AgentHeartBeatLostNum":0,
    				"HealthDetailList":{
    					"HealthDetail":[]
    				},
    				"HealthLevel":"NORMAL",
    				"ManualStoppedNum":0,
    				"NormalNum":1,
    				"ServiceName":"APACHEDS",
    				"StoppedNum":0,
    				"TotalNum":1
    			},
    			{
    				"AgentHeartBeatLostNum":0,
    				"HealthDetailList":{
    					"HealthDetail":[]
    				},
    				"HealthLevel":"NORMAL",
    				"ManualStoppedNum":0,
    				"NormalNum":3,
    				"ServiceName":"ILOGTAIL",
    				"StoppedNum":0,
    				"TotalNum":3
    			},
    			{
    				"AgentHeartBeatLostNum":0,
    				"HealthDetailList":{
    					"HealthDetail":[]
    				},
    				"HealthLevel":"NORMAL",
    				"ManualStoppedNum":2,
    				"NormalNum":7,
    				"ServiceName":"HDFS",
    				"StoppedNum":2,
    				"TotalNum":9
    			},
    			{
    				"AgentHeartBeatLostNum":0,
    				"HealthDetailList":{
    					"HealthDetail":[]
    				},
    				"HealthLevel":"NORMAL",
    				"ManualStoppedNum":0,
    				"NormalNum":4,
    				"ServiceName":"TEZ",
    				"StoppedNum":0,
    				"TotalNum":4
    			},
    			{
    				"AgentHeartBeatLostNum":0,
    				"HealthDetailList":{
    					"HealthDetail":[]
    				},
    				"HealthLevel":"NORMAL",
    				"ManualStoppedNum":0,
    				"NormalNum":5,
    				"ServiceName":"SPARK",
    				"StoppedNum":0,
    				"TotalNum":5
    			},
    			{
    				"AgentHeartBeatLostNum":0,
    				"HealthDetailList":{
    					"HealthDetail":[]
    				},
    				"HealthLevel":"NORMAL",
    				"ManualStoppedNum":0,
    				"NormalNum":1,
    				"ServiceName":"HAPROXY",
    				"StoppedNum":0,
    				"TotalNum":1
    			},
    			{
    				"AgentHeartBeatLostNum":0,
    				"HealthDetailList":{
    					"HealthDetail":[]
    				},
    				"HealthLevel":"NORMAL",
    				"ManualStoppedNum":0,
    				"NormalNum":6,
    				"ServiceName":"PRESTO",
    				"StoppedNum":0,
    				"TotalNum":6
    			},
    			{
    				"AgentHeartBeatLostNum":0,
    				"HealthDetailList":{
    					"HealthDetail":[]
    				},
    				"HealthLevel":"NORMAL",
    				"ManualStoppedNum":0,
    				"NormalNum":9,
    				"ServiceName":"YARN",
    				"StoppedNum":0,
    				"TotalNum":9
    			},
    			{
    				"AgentHeartBeatLostNum":0,
    				"HealthDetailList":{
    					"HealthDetail":[]
    				},
    				"HealthLevel":"NORMAL",
    				"ManualStoppedNum":0,
    				"NormalNum":5,
    				"ServiceName":"EMRFLOW",
    				"StoppedNum":0,
    				"TotalNum":5
    			},
    			{
    				"AgentHeartBeatLostNum":0,
    				"HealthDetailList":{
    					"HealthDetail":[]
    				},
    				"HealthLevel":"NORMAL",
    				"ManualStoppedNum":0,
    				"NormalNum":1,
    				"ServiceName":"KNOX",
    				"StoppedNum":0,
    				"TotalNum":1
    			},
    			{
    				"AgentHeartBeatLostNum":0,
    				"HealthDetailList":{
    					"HealthDetail":[]
    				},
    				"HealthLevel":"NORMAL",
    				"ManualStoppedNum":0,
    				"NormalNum":3,
    				"ServiceName":"PIG",
    				"StoppedNum":0,
    				"TotalNum":3
    			},
    			{
    				"AgentHeartBeatLostNum":0,
    				"HealthDetailList":{
    					"HealthDetail":[]
    				},
    				"HealthLevel":"NORMAL",
    				"ManualStoppedNum":0,
    				"NormalNum":5,
    				"ServiceName":"HIVE",
    				"StoppedNum":0,
    				"TotalNum":5
    			},
    			{
    				"AgentHeartBeatLostNum":0,
    				"HealthDetailList":{
    					"HealthDetail":[]
    				},
    				"HealthLevel":"NORMAL",
    				"ManualStoppedNum":0,
    				"NormalNum":1,
    				"ServiceName":"HUE",
    				"StoppedNum":0,
    				"TotalNum":1
    			}
    		]
    	},
    	"RequestId":"F695F08B-6C37-482A-AA4F-44A07B05CE84"
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

