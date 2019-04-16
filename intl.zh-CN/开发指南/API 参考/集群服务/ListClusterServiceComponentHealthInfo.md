# ListClusterServiceComponentHealthInfo {#concept_gmh_vwm_2gb .concept}

调用 ListClusterServiceComponentHealthInfo 接口获取集群指定服务对应的组件健康信息列表

## 请求参数 {#section_obj_yxl_2gb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|ClusterId|String|是|C-F32FB31D82954C64|集群 ID|
|ServiceName|String|否|TEZ|服务名称|
|RegionId|String|是|cn-hangzhou|区域 ID|
|AccessKeyId|String|是|LTAI8ljWyu7y\*\*\*\*|阿里云AccessKey ID信息|

## 返回参数 {#section_vbj_yxl_2gb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|DF202AC2-5D5D-4288-B608-B7B1595B5C7C|请求 ID|
|ClusterId|String|C-F32FB31D82954C64|集群 ID|
|HealthInfoList| | |健康信息列表|
|ServiceName|String|YARN|服务名称|
|ComponentName|String|ResourceManger|组件名称|
|HealthLevel|String|NORMAL|健康级别|
|StoppedNum|Integer|0|已停止的组件个数|
|ManualStoppedNum|Integer|0|手动停止的组件个数|
|NormalNum|Integer|7|正常状态的组件个数|
|TotalNum|Integer|7|总数|
|AgentHeartBeatLostNum|Integer|0|EMR 的管控 Agent 心跳不正常的主机个数|
|HealthDetailList| | |健康详情信息|
|code|String|200|错误信息编码|
|HealthRuleParam| | |检查规格信息|
|Service|String|YARN|服务名称|
|Component|String|Ecm-Agent|组件名|
|RuleTitle|String|AgentHeartBeatCheck|健康检查规格标题|
|Pass|String|""|规则监测通过信息|
|RuleId|String|111|规则 ID|
|RuleDescription|String|Agent信息监测|规则描述|
|HostNames|String|emr-worker-1|主机名|

## 示例 {#section_dcj_yxl_2gb .section}

-   请求示例

    ```
    /?ClusterId=C-F32FB31D82954C64
    &RegionId=ccn-hangzhou
    &AccessKeyId=LTAI8ljWyu7y****
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

