# DescribeClusterServiceConfig {#concept_txy_hqf_2gb .concept}

查询集群指定服务的配置详情信息

## 请求参数 {#section_p5v_ffb_kfb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|ClusterId|String|是|C-EBD62A703A430E23|集群 ID|
|RegionId|String|是|cn-hangzhou|区域 ID|
|ConfigVersion|String|否|0|配置的版本信息，可通过 DescribeClusterServiceConfigHistory 接口获取|
|GroupId|String|否|0|当前配置组的 ID 信息|
|HostInstanceId|String|否|ecsId|主机的 instanceId 信息，一般是对应的 ecsId 信息|
|ServiceName|String|否|TEZ|服务名称|
|TagValue|String|否|tez-site|待查询的配置的标签信息，可通过 DescribeClusterServiceConfigTag 接口获取|
|AccessKeyId|String|否|LTAI8ljWyu7y\*\*\*\*|阿里云AccessKey ID信息|

## 返回参数 {#section_vdw_lv4_dgb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|094585B2-13AF-4780-96B3-C8E925418B5D|请求 ID|
|Config| | |配置的详情信息|
|ServiceName|String|TEZ|服务名称|
|ConfigVersion|String|0|配置版本|
|Applied|String|true|当前是否已经生效|
|CreateTime|String|1543312717000|创建时间|
|Author|String|125046002175\*\*\*\*|修改人 ID|
|Comment|String|""|备注|
|ConfigValueList| | |服务的配置文件列表|
|ConfigName|String|tez-site|配置文件名|
|AllowCustom|Boolean|true|当前配置文件是否允许自定义配置项|
|ConfigItemValueList| | |配置项的 K-V 对|
|ItemName|String|tez.history.logging.service.class|配置项名称|
|Value|String|org.apache.tez.dag.history.logging.ats.ATSHistoryLoggingService|配置项的值|
|IsCustom|Boolean|false|是否为自定义配置项|
|Description|String|""|配置项的描述信息|
|PropertyInfoList| | |配置项的属性信息|
|Name|String|tez.lib.uris|配置项名称|
|Value|String|$\{fs.defaultFS\}/apps/tez-0.9.1-1.0.2/,$\{fs.defaultFS\}/apps/tez-0.9.1-1.0.2/lib/|配置项的 value 模板或者示例|
|Description|String|""|配置项的描述信息|
|FileName|String|tez-site|配置文件名|
|DisplayName|String|tez-site|展示名称|
|ServiceName|String|TEZ|服务名称|
|Component|String|""|组件名|
|PropertyTypes|String|\["MEMORY"\]|配置项的属性类型|
|PropertyValueAttributes| | |配置项的值的属性信息|
|Type|String|""|预留字段，暂时不生效|
|Maximum|String|10000|最大值|
|Mimimum|String|10|最小值|
|Unit|String|MB|配置项值的单位|
|ReadOnly|Boolean|true|配置项是否只读（不能修改）|
|Hidden|Boolean|true|是否展示|
|IncrememtStep|String|10|递增类型的 Value 的递增步长|
|Entries| | |预留字段|
|Value|String|""|预留字段|
|Label|String|""|预留字段|
|Description|String|""|预留字段|
|EffectWay| | |预留字段|
|EffectType|String|""|预留字段|
|InvokeServiceName|String|""|预留字段|

## 示例 {#section_ydw_lv4_dgb .section}

-   请求示例

    ```
    /?ClusterId=C-F32FB31D82954C64
    &RegionId=cn-hangzhou
    &AccessKeyId=LTAI8ljWyu7y****
    &ConfigVersion=0
    &GroupId=0
    &HostInstanceId=ecsId
    &ServiceName=TEZ
    &TagValue=tez-site
    &<公共请求参数>
    ```

-   正常返回示例

    JSON 格式

    ```
    {
    	"Config":{
    		"Applied":true,
    		"Author":"125046002175****",
    		"Comment":"",
    		"ConfigValueList":{
    			"ConfigValue":[{
    				"AllowCustom":true,
    				"ConfigItemValueList":{
    					"ConfigItemValue":[
    						{
    							"ItemName":"tez.history.logging.service.class",
    							"Value":"org.apache.tez.dag.history.logging.ats.ATSHistoryLoggingService"
    						},
    						{
    							"ItemName":"tez.runtime.convert.user-payload.to.history-text",
    							"Value":"true"
    						},
    						{
    							"ItemName":"tez.am.resource.memory.mb",
    							"Value":"640"
    						},
    						{
    							"ItemName":"tez.tez-ui.history-url.base",
    							"Value":"http://emr-header-1:8090/tez-ui2/"
    						},
    						{
    							"ItemName":"tez.lib.uris",
    							"Value":"${fs.defaultFS}/apps/tez-0.9.1-1.0.2/,${fs.defaultFS}/apps/tez-0.9.1-1.0.2/lib/"
    						},
    						{
    							"ItemName":"tez.container.max.java.heap.fraction",
    							"Value":"0.8"
    						},
    						{
    							"ItemName":"tez.am.java.opts",
    							"Value":"-Xmx512m"
    						},
    						{
    							"ItemName":"tez.staging-dir",
    							"Value":"/tmp/tez/staging"
    						}
    					]
    				},
    				"ConfigName":"tez-site"
    			}]
    		},
    		"ConfigVersion":"0",
    		"CreateTime":"1543312717000",
    		"PropertyInfoList":{
    			"PropertyInfo":[
    				{
    					"Description":"",
    					"DisplayName":"",
    					"EffectWay":{},
    					"FileName":"tez-site",
    					"Name":"tez.lib.uris",
    					"PropertyTypes":{
    						"propertyType":[]
    					},
    					"PropertyValueAttributes":{
    						"Entries":{
    							"ValueEntryInfo":[]
    						},
    						"Hidden":false,
    						"IncrememtStep":"",
    						"Maximum":"2147483647",
    						"Mimimum":"",
    						"ReadOnly":false,
    						"Type":"",
    						"Unit":""
    					},
    					"ServiceName":"TEZ",
    					"Value":"${fs.defaultFS}/apps/tez-0.9.1-1.0.2/,${fs.defaultFS}/apps/tez-0.9.1-1.0.2/lib/"
    				},
    				{
    					"Description":"",
    					"DisplayName":"",
    					"EffectWay":{},
    					"FileName":"tez-site",
    					"Name":"tez.staging-dir",
    					"PropertyTypes":{
    						"propertyType":[]
    					},
    					"PropertyValueAttributes":{
    						"Entries":{
    							"ValueEntryInfo":[]
    						},
    						"Hidden":false,
    						"IncrememtStep":"",
    						"Maximum":"2147483647",
    						"Mimimum":"",
    						"ReadOnly":false,
    						"Type":"",
    						"Unit":""
    					},
    					"ServiceName":"TEZ",
    					"Value":"/tmp/tez/staging"
    				},
    				{
    					"Description":"",
    					"DisplayName":"",
    					"EffectWay":{},
    					"FileName":"tez-site",
    					"Name":"tez.am.resource.memory.mb",
    					"PropertyTypes":{
    						"propertyType":["MEMORY"]
    					},
    					"PropertyValueAttributes":{
    						"Entries":{
    							"ValueEntryInfo":[]
    						},
    						"Hidden":false,
    						"IncrememtStep":"",
    						"Maximum":"2147483647",
    						"Mimimum":"",
    						"ReadOnly":false,
    						"Type":"",
    						"Unit":""
    					},
    					"ServiceName":"TEZ",
    					"Value":"#yarn_app_mapreduce_am_resource_mb#"
    				},
    				{
    					"Description":"",
    					"DisplayName":"",
    					"EffectWay":{},
    					"FileName":"tez-site",
    					"Name":"tez.container.max.java.heap.fraction",
    					"PropertyTypes":{
    						"propertyType":["MEMORY"]
    					},
    					"PropertyValueAttributes":{
    						"Entries":{
    							"ValueEntryInfo":[]
    						},
    						"Hidden":false,
    						"IncrememtStep":"",
    						"Maximum":"2147483647",
    						"Mimimum":"",
    						"ReadOnly":false,
    						"Type":"",
    						"Unit":""
    					},
    					"ServiceName":"TEZ",
    					"Value":"0.8"
    				},
    				{
    					"Description":"",
    					"DisplayName":"",
    					"EffectWay":{},
    					"FileName":"tez-site",
    					"Name":"tez.history.logging.service.class",
    					"PropertyTypes":{
    						"propertyType":[]
    					},
    					"PropertyValueAttributes":{
    						"Entries":{
    							"ValueEntryInfo":[]
    						},
    						"Hidden":false,
    						"IncrememtStep":"",
    						"Maximum":"2147483647",
    						"Mimimum":"",
    						"ReadOnly":false,
    						"Type":"",
    						"Unit":""
    					},
    					"ServiceName":"TEZ",
    					"Value":"org.apache.tez.dag.history.logging.ats.ATSHistoryLoggingService"
    				},
    				{
    					"Description":"Publish configuration information to Timeline server.",
    					"DisplayName":"",
    					"EffectWay":{},
    					"FileName":"tez-site",
    					"Name":"tez.runtime.convert.user-payload.to.history-text",
    					"PropertyTypes":{
    						"propertyType":[]
    					},
    					"PropertyValueAttributes":{
    						"Entries":{
    							"ValueEntryInfo":[]
    						},
    						"Hidden":false,
    						"IncrememtStep":"",
    						"Maximum":"2147483647",
    						"Mimimum":"",
    						"ReadOnly":false,
    						"Type":"",
    						"Unit":""
    					},
    					"ServiceName":"TEZ",
    					"Value":"true"
    				},
    				{
    					"Description":"",
    					"DisplayName":"",
    					"EffectWay":{},
    					"FileName":"tez-site",
    					"Name":"tez.tez-ui.history-url.base",
    					"PropertyTypes":{
    						"propertyType":[]
    					},
    					"PropertyValueAttributes":{
    						"Entries":{
    							"ValueEntryInfo":[]
    						},
    						"Hidden":false,
    						"IncrememtStep":"",
    						"Maximum":"2147483647",
    						"Mimimum":"",
    						"ReadOnly":false,
    						"Type":"",
    						"Unit":""
    					},
    					"ServiceName":"TEZ",
    					"Value":"http://emr-header-1:8090/tez-ui2/"
    				},
    				{
    					"Description":"",
    					"DisplayName":"tez.am.java.opts",
    					"EffectWay":{},
    					"FileName":"tez-site",
    					"Name":"tez.am.java.opts",
    					"PropertyTypes":{
    						"propertyType":[]
    					},
    					"PropertyValueAttributes":{
    						"Entries":{
    							"ValueEntryInfo":[]
    						},
    						"Hidden":false,
    						"IncrememtStep":"",
    						"Maximum":"2147483647",
    						"Mimimum":"",
    						"ReadOnly":false,
    						"Type":"",
    						"Unit":""
    					},
    					"ServiceName":"TEZ",
    					"Value":"-Xmx512m"
    				},
    				{
    					"Description":"",
    					"DisplayName":"tomcat_heapsize",
    					"EffectWay":{},
    					"FileName":"user_params",
    					"Name":"tomcat_heapsize",
    					"PropertyTypes":{
    						"propertyType":[]
    					},
    					"PropertyValueAttributes":{
    						"Entries":{
    							"ValueEntryInfo":[]
    						},
    						"Hidden":false,
    						"IncrememtStep":"",
    						"Maximum":"2147483647",
    						"Mimimum":"",
    						"ReadOnly":false,
    						"Type":"",
    						"Unit":""
    					},
    					"ServiceName":"TEZ",
    					"Value":"512"
    				},
    				{
    					"Description":"",
    					"DisplayName":"do_init_host",
    					"EffectWay":{},
    					"FileName":"user_params",
    					"Name":"do_init_host",
    					"PropertyTypes":{
    						"propertyType":[]
    					},
    					"PropertyValueAttributes":{
    						"Entries":{
    							"ValueEntryInfo":[]
    						},
    						"Hidden":false,
    						"IncrememtStep":"",
    						"Maximum":"2147483647",
    						"Mimimum":"",
    						"ReadOnly":false,
    						"Type":"",
    						"Unit":""
    					},
    					"ServiceName":"TEZ",
    					"Value":"emr-header-1"
    				}
    			]
    		},
    		"ServiceName":"TEZ"
    	},
    	"RequestId":"094585B2-13AF-4780-96B3-C8E925418B5D"
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

