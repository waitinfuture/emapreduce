# ListClusterService {#concept_b3g_xpm_2gb .concept}

调用 ListClusterService 接口查询集群的服务列表信息

## 请求参数 {#section_obj_yxl_2gb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|ClusterId|String|是|C-F32FB31D82954C64|集群 ID|
|RegionId|String|是|cn-hangzhou|区域 ID|
|PageNumber|Integer|否|1|分页查询的查询页码|
|PageSize|Integer|否|100|分页查询的每页记录数|
|AccessKeyId|String|否|LTAI8ljWyu7y\*\*\*\*|阿里云AccessKey ID 信息|

## 返回参数 {#section_vbj_yxl_2gb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|DF202AC2-5D5D-4288-B608-B7B1595B5C7C|请求 ID|
|TotalCount|Integer|1|记录总数|
|PageNumber|Integer|1|当前的页号|
|PageSize|Integer|100|每页的记录数|
|ClusterServiceList| | |集群的服务列表|
|ServiceName|String|HDFS|服务名称|
|ServiceDisplayName|String|HDFS|服务的展示名称|
|ServiceVersion|String|2.7.2|服务版本号|
|InstallStatus|Boolean|true|安装状态，表示当前集群是否安装了该服务|
|ClientType|Boolean|false|是否 Client 类型|
|ServiceStatus|String|INSTALLED|服务状态|
|HealthStatus|String|""|服务的组件健康状态信息|
|NeedRestartInfo|String|""|服务对应的组件，当前是否需要重启的信息|
|NotStartInfo|String|""|没有启动的组件信息|
|AbnormalNum|Integer|0|异常的组件数量|
|StoppedNum|Integer|0|已停止的组件个数|
|NeedRestartNum|Integer|0|需要重启的组件数量|
|ServiceActionList| | |服务支持的 Action 列表|
|ServiceName|String|YARN|服务名称|
|ComponentName|String|ResourceManager|组件名称|
|ActionName|String|CUSTOM\_COMMAND|Action 名称|
|Command|String|refreshQueues|具体的命令，当 Action 名称为 CUSTOM\_COMMAND 时，需要指定|
|DisplayName|String|Refresh Queues|展示名称|

## 示例 {#section_dcj_yxl_2gb .section}

-   请求示例

    ```
    /?ClusterId=C-F32FB31D82954C64
    &RegionId=cn-hangzhou
    &AccessKeyId=LTAI8ljWyu7y****
    &PageNumber=1
    &PageSize=100
    &<公共请求参数>
    ```

-   正常返回示例

    JSON 格式

    ```
    {
    	"ClusterServiceList":{
    		"ClusterService":[
    			{
    				"AbnormalNum":0,
    				"ClientType":false,
    				"HealthStatus":"",
    				"InstallStatus":true,
    				"NeedRestartInfo":"",
    				"NeedRestartNum":0,
    				"NotStartInfo":"Have 0 component(s) not started.",
    				"ServiceActionList":{
    					"ServiceAction":[
    						{
    							"ActionName":"CONFIGURE",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"CONFIGURE All Components",
    							"ServiceName":"HDFS"
    						},
    						{
    							"ActionName":"START",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"START All Components",
    							"ServiceName":"HDFS"
    						},
    						{
    							"ActionName":"STOP",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"STOP All Components",
    							"ServiceName":"HDFS"
    						},
    						{
    							"ActionName":"RESTART",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"RESTART All Components",
    							"ServiceName":"HDFS"
    						},
    						{
    							"ActionName":"RESTART",
    							"ComponentName":"DataNode",
    							"DisplayName":"RESTART DataNode",
    							"ServiceName":"HDFS"
    						},
    						{
    							"ActionName":"RESTART",
    							"ComponentName":"HttpFS",
    							"DisplayName":"RESTART HttpFS",
    							"ServiceName":"HDFS"
    						},
    						{
    							"ActionName":"RESTART",
    							"ComponentName":"KMS",
    							"DisplayName":"RESTART KMS",
    							"ServiceName":"HDFS"
    						},
    						{
    							"ActionName":"RESTART",
    							"ComponentName":"NameNode",
    							"DisplayName":"RESTART NameNode",
    							"ServiceName":"HDFS"
    						},
    						{
    							"ActionName":"RESTART",
    							"ComponentName":"SecondaryNameNode",
    							"DisplayName":"RESTART SecondaryNameNode",
    							"ServiceName":"HDFS"
    						},
    						{
    							"ActionName":"CUSTOM_COMMAND",
    							"Command":"rebalance",
    							"ComponentName":"NameNode",
    							"DisplayName":"REBALANCE HDFS",
    							"ServiceName":"HDFS"
    						},
    						{
    							"ActionName":"CUSTOM_COMMAND",
    							"Command":"stop_rebalance",
    							"ComponentName":"NameNode",
    							"DisplayName":"STOP_REBALANCE HDFS",
    							"ServiceName":"HDFS"
    						}
    					]
    				},
    				"ServiceDisplayName":"HDFS",
    				"ServiceName":"HDFS",
    				"ServiceStatus":"INSTALLED",
    				"ServiceVersion":"2.7.2",
    				"StoppedNum":2
    			},
    			{
    				"AbnormalNum":0,
    				"ClientType":false,
    				"HealthStatus":"",
    				"InstallStatus":true,
    				"NeedRestartInfo":"",
    				"NeedRestartNum":0,
    				"NotStartInfo":"",
    				"ServiceActionList":{
    					"ServiceAction":[
    						{
    							"ActionName":"CONFIGURE",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"CONFIGURE All Components",
    							"ServiceName":"YARN"
    						},
    						{
    							"ActionName":"START",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"START All Components",
    							"ServiceName":"YARN"
    						},
    						{
    							"ActionName":"STOP",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"STOP All Components",
    							"ServiceName":"YARN"
    						},
    						{
    							"ActionName":"RESTART",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"RESTART All Components",
    							"ServiceName":"YARN"
    						},
    						{
    							"ActionName":"RESTART",
    							"ComponentName":"JobHistory",
    							"DisplayName":"RESTART JobHistory",
    							"ServiceName":"YARN"
    						},
    						{
    							"ActionName":"RESTART",
    							"ComponentName":"NodeManager",
    							"DisplayName":"RESTART NodeManager",
    							"ServiceName":"YARN"
    						},
    						{
    							"ActionName":"RESTART",
    							"ComponentName":"ResourceManager",
    							"DisplayName":"RESTART ResourceManager",
    							"ServiceName":"YARN"
    						},
    						{
    							"ActionName":"RESTART",
    							"ComponentName":"TimeLineServer",
    							"DisplayName":"RESTART App Timeline Server",
    							"ServiceName":"YARN"
    						},
    						{
    							"ActionName":"RESTART",
    							"ComponentName":"WebAppProxyServer",
    							"DisplayName":"RESTART WebAppProxyServer",
    							"ServiceName":"YARN"
    						},
    						{
    							"ActionName":"CUSTOM_COMMAND",
    							"Command":"refreshQueues",
    							"ComponentName":"ResourceManager",
    							"DisplayName":"Refresh Queues",
    							"ServiceName":"YARN"
    						},
    						{
    							"ActionName":"CUSTOM_COMMAND",
    							"Command":"enableCGroups",
    							"ComponentName":"NodeManager",
    							"DisplayName":"Enable CGroups",
    							"ServiceName":"YARN"
    						},
    						{
    							"ActionName":"CUSTOM_COMMAND",
    							"Command":"disableCGroups",
    							"ComponentName":"NodeManager",
    							"DisplayName":"Disable CGroups",
    							"ServiceName":"YARN"
    						}
    					]
    				},
    				"ServiceDisplayName":"YARN",
    				"ServiceName":"YARN",
    				"ServiceStatus":"INSTALLED",
    				"ServiceVersion":"2.7.2",
    				"StoppedNum":0
    			},
    			{
    				"AbnormalNum":0,
    				"ClientType":false,
    				"HealthStatus":"",
    				"InstallStatus":true,
    				"NeedRestartInfo":"",
    				"NeedRestartNum":0,
    				"NotStartInfo":"",
    				"ServiceActionList":{
    					"ServiceAction":[
    						{
    							"ActionName":"CONFIGURE",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"CONFIGURE All Components",
    							"ServiceName":"HIVE"
    						},
    						{
    							"ActionName":"START",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"START All Components",
    							"ServiceName":"HIVE"
    						},
    						{
    							"ActionName":"STOP",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"STOP All Components",
    							"ServiceName":"HIVE"
    						},
    						{
    							"ActionName":"RESTART",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"RESTART All Components",
    							"ServiceName":"HIVE"
    						},
    						{
    							"ActionName":"RESTART",
    							"ComponentName":"HiveMetaStore",
    							"DisplayName":"RESTART Hive MetaStore",
    							"ServiceName":"HIVE"
    						},
    						{
    							"ActionName":"RESTART",
    							"ComponentName":"HiveServer",
    							"DisplayName":"RESTART HiveServer2",
    							"ServiceName":"HIVE"
    						}
    					]
    				},
    				"ServiceDisplayName":"Hive",
    				"ServiceName":"HIVE",
    				"ServiceStatus":"INSTALLED",
    				"ServiceVersion":"2.3.3",
    				"StoppedNum":0
    			},
    			{
    				"AbnormalNum":0,
    				"ClientType":false,
    				"HealthStatus":"",
    				"InstallStatus":true,
    				"NeedRestartInfo":"",
    				"NeedRestartNum":0,
    				"NotStartInfo":"",
    				"ServiceActionList":{
    					"ServiceAction":[
    						{
    							"ActionName":"CONFIGURE",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"CONFIGURE All Components",
    							"ServiceName":"GANGLIA"
    						},
    						{
    							"ActionName":"START",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"START All Components",
    							"ServiceName":"GANGLIA"
    						},
    						{
    							"ActionName":"STOP",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"STOP All Components",
    							"ServiceName":"GANGLIA"
    						},
    						{
    							"ActionName":"RESTART",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"RESTART All Components",
    							"ServiceName":"GANGLIA"
    						},
    						{
    							"ActionName":"RESTART",
    							"ComponentName":"GMETAD",
    							"DisplayName":"RESTART Gmetad",
    							"ServiceName":"GANGLIA"
    						},
    						{
    							"ActionName":"RESTART",
    							"ComponentName":"GMOND",
    							"DisplayName":"RESTART Gmond",
    							"ServiceName":"GANGLIA"
    						},
    						{
    							"ActionName":"RESTART",
    							"ComponentName":"HTTPD",
    							"DisplayName":"RESTART Httpd",
    							"ServiceName":"GANGLIA"
    						}
    					]
    				},
    				"ServiceDisplayName":"Ganglia",
    				"ServiceName":"GANGLIA",
    				"ServiceStatus":"INSTALLED",
    				"ServiceVersion":"3.7.2",
    				"StoppedNum":0
    			},
    			{
    				"HealthStatus":"",
    				"InstallStatus":false,
    				"NeedRestartInfo":"",
    				"NotStartInfo":"",
    				"ServiceActionList":{
    					"ServiceAction":[]
    				},
    				"ServiceDisplayName":"ZooKeeper",
    				"ServiceName":"ZOOKEEPER",
    				"ServiceStatus":"",
    				"ServiceVersion":"3.4.13"
    			},
    			{
    				"AbnormalNum":0,
    				"ClientType":false,
    				"HealthStatus":"",
    				"InstallStatus":true,
    				"NeedRestartInfo":"",
    				"NeedRestartNum":0,
    				"NotStartInfo":"",
    				"ServiceActionList":{
    					"ServiceAction":[
    						{
    							"ActionName":"CONFIGURE",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"CONFIGURE All Components",
    							"ServiceName":"SPARK"
    						},
    						{
    							"ActionName":"START",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"START All Components",
    							"ServiceName":"SPARK"
    						},
    						{
    							"ActionName":"STOP",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"STOP All Components",
    							"ServiceName":"SPARK"
    						},
    						{
    							"ActionName":"RESTART",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"RESTART All Components",
    							"ServiceName":"SPARK"
    						},
    						{
    							"ActionName":"RESTART",
    							"ComponentName":"SparkHistory",
    							"DisplayName":"RESTART SparkHistory",
    							"ServiceName":"SPARK"
    						},
    						{
    							"ActionName":"RESTART",
    							"ComponentName":"SparkThriftServer",
    							"DisplayName":"RESTART ThriftServer",
    							"ServiceName":"SPARK"
    						}
    					]
    				},
    				"ServiceDisplayName":"Spark",
    				"ServiceName":"SPARK",
    				"ServiceStatus":"INSTALLED",
    				"ServiceVersion":"2.3.2",
    				"StoppedNum":0
    			},
    			{
    				"HealthStatus":"",
    				"InstallStatus":false,
    				"NeedRestartInfo":"",
    				"NotStartInfo":"",
    				"ServiceActionList":{
    					"ServiceAction":[]
    				},
    				"ServiceDisplayName":"HBase",
    				"ServiceName":"HBASE",
    				"ServiceStatus":"",
    				"ServiceVersion":"1.1.1"
    			},
    			{
    				"AbnormalNum":0,
    				"ClientType":false,
    				"HealthStatus":"",
    				"InstallStatus":true,
    				"NeedRestartInfo":"",
    				"NeedRestartNum":0,
    				"NotStartInfo":"",
    				"ServiceActionList":{
    					"ServiceAction":[
    						{
    							"ActionName":"CONFIGURE",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"CONFIGURE All Components",
    							"ServiceName":"HUE"
    						},
    						{
    							"ActionName":"START",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"START All Components",
    							"ServiceName":"HUE"
    						},
    						{
    							"ActionName":"STOP",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"STOP All Components",
    							"ServiceName":"HUE"
    						},
    						{
    							"ActionName":"RESTART",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"RESTART All Components",
    							"ServiceName":"HUE"
    						},
    						{
    							"ActionName":"RESTART",
    							"ComponentName":"Hue",
    							"DisplayName":"RESTART Hue",
    							"ServiceName":"HUE"
    						}
    					]
    				},
    				"ServiceDisplayName":"Hue",
    				"ServiceName":"HUE",
    				"ServiceStatus":"INSTALLED",
    				"ServiceVersion":"4.1.0",
    				"StoppedNum":0
    			},
    			{
    				"HealthStatus":"",
    				"InstallStatus":false,
    				"NeedRestartInfo":"",
    				"NotStartInfo":"",
    				"ServiceActionList":{
    					"ServiceAction":[]
    				},
    				"ServiceDisplayName":"Zeppelin",
    				"ServiceName":"ZEPPELIN",
    				"ServiceStatus":"",
    				"ServiceVersion":"0.8.0"
    			},
    			{
    				"HealthStatus":"",
    				"InstallStatus":false,
    				"NeedRestartInfo":"",
    				"NotStartInfo":"",
    				"ServiceActionList":{
    					"ServiceAction":[]
    				},
    				"ServiceDisplayName":"Oozie",
    				"ServiceName":"OOZIE",
    				"ServiceStatus":"",
    				"ServiceVersion":"4.2.0"
    			},
    			{
    				"AbnormalNum":0,
    				"ClientType":false,
    				"HealthStatus":"",
    				"InstallStatus":true,
    				"NeedRestartInfo":"",
    				"NeedRestartNum":0,
    				"NotStartInfo":"",
    				"ServiceActionList":{
    					"ServiceAction":[
    						{
    							"ActionName":"CONFIGURE",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"CONFIGURE All Components",
    							"ServiceName":"TEZ"
    						},
    						{
    							"ActionName":"START",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"START All Components",
    							"ServiceName":"TEZ"
    						},
    						{
    							"ActionName":"STOP",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"STOP All Components",
    							"ServiceName":"TEZ"
    						},
    						{
    							"ActionName":"RESTART",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"RESTART All Components",
    							"ServiceName":"TEZ"
    						},
    						{
    							"ActionName":"RESTART",
    							"ComponentName":"TezInit",
    							"DisplayName":"RESTART Tez Client",
    							"ServiceName":"TEZ"
    						},
    						{
    							"ActionName":"RESTART",
    							"ComponentName":"Tomcat",
    							"DisplayName":"RESTART Tomcat",
    							"ServiceName":"TEZ"
    						}
    					]
    				},
    				"ServiceDisplayName":"Tez",
    				"ServiceName":"TEZ",
    				"ServiceStatus":"INSTALLED",
    				"ServiceVersion":"0.9.1",
    				"StoppedNum":0
    			},
    			{
    				"HealthStatus":"",
    				"InstallStatus":false,
    				"NeedRestartInfo":"",
    				"NotStartInfo":"",
    				"ServiceActionList":{
    					"ServiceAction":[]
    				},
    				"ServiceDisplayName":"Phoenix",
    				"ServiceName":"PHOENIX",
    				"ServiceStatus":"",
    				"ServiceVersion":"4.10.0"
    			},
    			{
    				"AbnormalNum":0,
    				"ClientType":false,
    				"HealthStatus":"",
    				"InstallStatus":true,
    				"NeedRestartInfo":"",
    				"NeedRestartNum":0,
    				"NotStartInfo":"",
    				"ServiceActionList":{
    					"ServiceAction":[
    						{
    							"ActionName":"CONFIGURE",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"CONFIGURE All Components",
    							"ServiceName":"PRESTO"
    						},
    						{
    							"ActionName":"START",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"START All Components",
    							"ServiceName":"PRESTO"
    						},
    						{
    							"ActionName":"STOP",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"STOP All Components",
    							"ServiceName":"PRESTO"
    						},
    						{
    							"ActionName":"RESTART",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"RESTART All Components",
    							"ServiceName":"PRESTO"
    						},
    						{
    							"ActionName":"RESTART",
    							"ComponentName":"PrestoMaster",
    							"DisplayName":"RESTART PrestoMaster",
    							"ServiceName":"PRESTO"
    						},
    						{
    							"ActionName":"RESTART",
    							"ComponentName":"PrestoWorker",
    							"DisplayName":"RESTART PrestoWorker",
    							"ServiceName":"PRESTO"
    						}
    					]
    				},
    				"ServiceDisplayName":"Presto",
    				"ServiceName":"PRESTO",
    				"ServiceStatus":"INSTALLED",
    				"ServiceVersion":"0.208",
    				"StoppedNum":0
    			},
    			{
    				"AbnormalNum":0,
    				"ClientType":false,
    				"HealthStatus":"",
    				"InstallStatus":true,
    				"NeedRestartInfo":"",
    				"NeedRestartNum":0,
    				"NotStartInfo":"",
    				"ServiceActionList":{
    					"ServiceAction":[{
    						"ActionName":"CONFIGURE",
    						"ComponentName":"ALL COMPONENTS",
    						"DisplayName":"CONFIGURE All Components",
    						"ServiceName":"SQOOP"
    					}]
    				},
    				"ServiceDisplayName":"Sqoop",
    				"ServiceName":"SQOOP",
    				"ServiceStatus":"INSTALLED",
    				"ServiceVersion":"1.4.7",
    				"StoppedNum":0
    			},
    			{
    				"AbnormalNum":0,
    				"ClientType":false,
    				"HealthStatus":"",
    				"InstallStatus":true,
    				"NeedRestartInfo":"",
    				"NeedRestartNum":0,
    				"NotStartInfo":"",
    				"ServiceActionList":{
    					"ServiceAction":[{
    						"ActionName":"CONFIGURE",
    						"ComponentName":"ALL COMPONENTS",
    						"DisplayName":"CONFIGURE All Components",
    						"ServiceName":"PIG"
    					}]
    				},
    				"ServiceDisplayName":"Pig",
    				"ServiceName":"PIG",
    				"ServiceStatus":"INSTALLED",
    				"ServiceVersion":"0.14.0",
    				"StoppedNum":0
    			},
    			{
    				"HealthStatus":"",
    				"InstallStatus":false,
    				"NeedRestartInfo":"",
    				"NotStartInfo":"",
    				"ServiceActionList":{
    					"ServiceAction":[]
    				},
    				"ServiceDisplayName":"Storm",
    				"ServiceName":"STORM",
    				"ServiceStatus":"",
    				"ServiceVersion":"1.1.2"
    			},
    			{
    				"HealthStatus":"",
    				"InstallStatus":false,
    				"NeedRestartInfo":"",
    				"NotStartInfo":"",
    				"ServiceActionList":{
    					"ServiceAction":[]
    				},
    				"ServiceDisplayName":"Impala",
    				"ServiceName":"IMPALA",
    				"ServiceStatus":"",
    				"ServiceVersion":"2.10.0"
    			},
    			{
    				"AbnormalNum":0,
    				"ClientType":false,
    				"HealthStatus":"",
    				"InstallStatus":true,
    				"NeedRestartInfo":"",
    				"NeedRestartNum":0,
    				"NotStartInfo":"",
    				"ServiceActionList":{
    					"ServiceAction":[
    						{
    							"ActionName":"CONFIGURE",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"CONFIGURE All Components",
    							"ServiceName":"HAPROXY"
    						},
    						{
    							"ActionName":"START",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"START All Components",
    							"ServiceName":"HAPROXY"
    						},
    						{
    							"ActionName":"STOP",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"STOP All Components",
    							"ServiceName":"HAPROXY"
    						},
    						{
    							"ActionName":"RESTART",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"RESTART All Components",
    							"ServiceName":"HAPROXY"
    						},
    						{
    							"ActionName":"RESTART",
    							"ComponentName":"HAProxy",
    							"DisplayName":"RESTART HAProxy",
    							"ServiceName":"HAPROXY"
    						}
    					]
    				},
    				"ServiceDisplayName":"HAProxy",
    				"ServiceName":"HAPROXY",
    				"ServiceStatus":"INSTALLED",
    				"ServiceVersion":"1.5.18",
    				"StoppedNum":0
    			},
    			{
    				"HealthStatus":"",
    				"InstallStatus":false,
    				"NeedRestartInfo":"",
    				"NotStartInfo":"",
    				"ServiceActionList":{
    					"ServiceAction":[]
    				},
    				"ServiceDisplayName":"Kafka",
    				"ServiceName":"KAFKA",
    				"ServiceStatus":"",
    				"ServiceVersion":"2.11-1.1.0-1.0.0"
    			},
    			{
    				"HealthStatus":"",
    				"InstallStatus":false,
    				"NeedRestartInfo":"",
    				"NotStartInfo":"",
    				"ServiceActionList":{
    					"ServiceAction":[]
    				},
    				"ServiceDisplayName":"Kafka-Manager",
    				"ServiceName":"KAFKA-MANAGER",
    				"ServiceStatus":"",
    				"ServiceVersion":"1.3.3.16-1.1.0"
    			},
    			{
    				"HealthStatus":"",
    				"InstallStatus":false,
    				"NeedRestartInfo":"",
    				"NotStartInfo":"",
    				"ServiceActionList":{
    					"ServiceAction":[]
    				},
    				"ServiceDisplayName":"TensorFlow",
    				"ServiceName":"TENSORFLOW",
    				"ServiceStatus":"",
    				"ServiceVersion":"1.8.0"
    			},
    			{
    				"HealthStatus":"",
    				"InstallStatus":false,
    				"NeedRestartInfo":"",
    				"NotStartInfo":"",
    				"ServiceActionList":{
    					"ServiceAction":[]
    				},
    				"ServiceDisplayName":"TensorFlow-On-YARN",
    				"ServiceName":"TENSORFLOW-ON-YARN",
    				"ServiceStatus":"",
    				"ServiceVersion":"1.0.0"
    			},
    			{
    				"HealthStatus":"",
    				"InstallStatus":false,
    				"NeedRestartInfo":"",
    				"NotStartInfo":"",
    				"ServiceActionList":{
    					"ServiceAction":[]
    				},
    				"ServiceDisplayName":"HAS",
    				"ServiceName":"HAS",
    				"ServiceStatus":"",
    				"ServiceVersion":"1.1.1"
    			},
    			{
    				"AbnormalNum":0,
    				"ClientType":false,
    				"HealthStatus":"",
    				"InstallStatus":true,
    				"NeedRestartInfo":"",
    				"NeedRestartNum":0,
    				"NotStartInfo":"",
    				"ServiceActionList":{
    					"ServiceAction":[
    						{
    							"ActionName":"CONFIGURE",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"CONFIGURE All Components",
    							"ServiceName":"APACHEDS"
    						},
    						{
    							"ActionName":"START",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"START All Components",
    							"ServiceName":"APACHEDS"
    						},
    						{
    							"ActionName":"STOP",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"STOP All Components",
    							"ServiceName":"APACHEDS"
    						},
    						{
    							"ActionName":"RESTART",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"RESTART All Components",
    							"ServiceName":"APACHEDS"
    						},
    						{
    							"ActionName":"RESTART",
    							"ComponentName":"ApacheDS",
    							"DisplayName":"RESTART ApacheDS",
    							"ServiceName":"APACHEDS"
    						}
    					]
    				},
    				"ServiceDisplayName":"ApacheDS",
    				"ServiceName":"APACHEDS",
    				"ServiceStatus":"INSTALLED",
    				"ServiceVersion":"2.0.0",
    				"StoppedNum":0
    			},
    			{
    				"AbnormalNum":0,
    				"ClientType":false,
    				"HealthStatus":"",
    				"InstallStatus":true,
    				"NeedRestartInfo":"",
    				"NeedRestartNum":0,
    				"NotStartInfo":"",
    				"ServiceActionList":{
    					"ServiceAction":[
    						{
    							"ActionName":"CONFIGURE",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"CONFIGURE All Components",
    							"ServiceName":"KNOX"
    						},
    						{
    							"ActionName":"START",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"START All Components",
    							"ServiceName":"KNOX"
    						},
    						{
    							"ActionName":"STOP",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"STOP All Components",
    							"ServiceName":"KNOX"
    						},
    						{
    							"ActionName":"RESTART",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"RESTART All Components",
    							"ServiceName":"KNOX"
    						},
    						{
    							"ActionName":"RESTART",
    							"ComponentName":"KNOX",
    							"DisplayName":"RESTART Knox",
    							"ServiceName":"KNOX"
    						}
    					]
    				},
    				"ServiceDisplayName":"Knox",
    				"ServiceName":"KNOX",
    				"ServiceStatus":"INSTALLED",
    				"ServiceVersion":"0.13.0",
    				"StoppedNum":0
    			},
    			{
    				"HealthStatus":"",
    				"InstallStatus":false,
    				"NeedRestartInfo":"",
    				"NotStartInfo":"",
    				"ServiceActionList":{
    					"ServiceAction":[]
    				},
    				"ServiceDisplayName":"Flink",
    				"ServiceName":"FLINK",
    				"ServiceStatus":"",
    				"ServiceVersion":"1.4.0"
    			},
    			{
    				"HealthStatus":"",
    				"InstallStatus":false,
    				"NeedRestartInfo":"",
    				"NotStartInfo":"",
    				"ServiceActionList":{
    					"ServiceAction":[]
    				},
    				"ServiceDisplayName":"RANGER",
    				"ServiceName":"RANGER",
    				"ServiceStatus":"",
    				"ServiceVersion":"1.0.0"
    			},
    			{
    				"HealthStatus":"",
    				"InstallStatus":false,
    				"NeedRestartInfo":"",
    				"NotStartInfo":"",
    				"ServiceActionList":{
    					"ServiceAction":[]
    				},
    				"ServiceDisplayName":"Druid",
    				"ServiceName":"DRUID",
    				"ServiceStatus":"",
    				"ServiceVersion":"0.12.3-1.0.0"
    			},
    			{
    				"HealthStatus":"",
    				"InstallStatus":false,
    				"NeedRestartInfo":"",
    				"NotStartInfo":"",
    				"ServiceActionList":{
    					"ServiceAction":[]
    				},
    				"ServiceDisplayName":"Superset",
    				"ServiceName":"SUPERSET",
    				"ServiceStatus":"",
    				"ServiceVersion":"0.26.3"
    			},
    			{
    				"HealthStatus":"",
    				"InstallStatus":false,
    				"NeedRestartInfo":"",
    				"NotStartInfo":"",
    				"ServiceActionList":{
    					"ServiceAction":[]
    				},
    				"ServiceDisplayName":"analytics-zoo",
    				"ServiceName":"ANALYTICS-ZOO",
    				"ServiceStatus":"",
    				"ServiceVersion":"0.2.0"
    			},
    			{
    				"HealthStatus":"",
    				"InstallStatus":false,
    				"NeedRestartInfo":"",
    				"NotStartInfo":"",
    				"ServiceActionList":{
    					"ServiceAction":[]
    				},
    				"ServiceDisplayName":"Jupyter",
    				"ServiceName":"JUPYTER",
    				"ServiceStatus":"",
    				"ServiceVersion":"4.4.0"
    			},
    			{
    				"HealthStatus":"",
    				"InstallStatus":false,
    				"NeedRestartInfo":"",
    				"NotStartInfo":"",
    				"ServiceActionList":{
    					"ServiceAction":[]
    				},
    				"ServiceDisplayName":"LIVY",
    				"ServiceName":"LIVY",
    				"ServiceStatus":"",
    				"ServiceVersion":"0.5.0"
    			},
    			{
    				"AbnormalNum":0,
    				"ClientType":false,
    				"HealthStatus":"",
    				"InstallStatus":true,
    				"NeedRestartInfo":"",
    				"NeedRestartNum":0,
    				"NotStartInfo":"",
    				"ServiceActionList":{
    					"ServiceAction":[
    						{
    							"ActionName":"CONFIGURE",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"CONFIGURE All Components",
    							"ServiceName":"ILOGTAIL"
    						},
    						{
    							"ActionName":"START",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"START All Components",
    							"ServiceName":"ILOGTAIL"
    						},
    						{
    							"ActionName":"STOP",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"STOP All Components",
    							"ServiceName":"ILOGTAIL"
    						},
    						{
    							"ActionName":"RESTART",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"RESTART All Components",
    							"ServiceName":"ILOGTAIL"
    						},
    						{
    							"ActionName":"RESTART",
    							"ComponentName":"Ilogtaild",
    							"DisplayName":"RESTART ilogtaild",
    							"ServiceName":"ILOGTAIL"
    						}
    					]
    				},
    				"ServiceDisplayName":"ILOGTAIL",
    				"ServiceName":"ILOGTAIL",
    				"ServiceStatus":"INSTALLED",
    				"ServiceVersion":"1.0.0",
    				"StoppedNum":0
    			},
    			{
    				"AbnormalNum":0,
    				"ClientType":false,
    				"HealthStatus":"",
    				"InstallStatus":true,
    				"NeedRestartInfo":"",
    				"NeedRestartNum":0,
    				"NotStartInfo":"",
    				"ServiceActionList":{
    					"ServiceAction":[
    						{
    							"ActionName":"CONFIGURE",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"CONFIGURE All Components",
    							"ServiceName":"EMRFLOW"
    						},
    						{
    							"ActionName":"START",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"START All Components",
    							"ServiceName":"EMRFLOW"
    						},
    						{
    							"ActionName":"STOP",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"STOP All Components",
    							"ServiceName":"EMRFLOW"
    						},
    						{
    							"ActionName":"RESTART",
    							"ComponentName":"ALL COMPONENTS",
    							"DisplayName":"RESTART All Components",
    							"ServiceName":"EMRFLOW"
    						},
    						{
    							"ActionName":"RESTART",
    							"ComponentName":"FlowAgentDaemon",
    							"DisplayName":"RESTART Flow Agent Daemon",
    							"ServiceName":"EMRFLOW"
    						},
    						{
    							"ActionName":"RESTART",
    							"ComponentName":"MetaCommand",
    							"DisplayName":"RESTART Emr Meta Command",
    							"ServiceName":"EMRFLOW"
    						},
    						{
    							"ActionName":"CUSTOM_COMMAND",
    							"Command":"do_ops",
    							"ComponentName":"MetaCommand",
    							"DisplayName":"metacli ops",
    							"ServiceName":"EMRFLOW"
    						}
    					]
    				},
    				"ServiceDisplayName":"EmrFlow",
    				"ServiceName":"EMRFLOW",
    				"ServiceStatus":"INSTALLED",
    				"ServiceVersion":"1.2.1.0.5",
    				"StoppedNum":0
    			}
    		]
    	},
    	"RequestId":"3F11150E-0F33-41DC-AE2B-C3675C9743BA"
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

