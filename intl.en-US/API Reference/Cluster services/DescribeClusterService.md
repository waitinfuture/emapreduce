# DescribeClusterService {#concept_uxk_znf_2gb .concept}

You can call this operation to view the description of a cluster service.

## Request parameters {#section_p5v_ffb_kfb .section}

|Name|Type|Required|Example|Description|
|----|----|--------|-------|-----------|
|ClusterId|String|Yes|C-EBD62A703A430E23|The ID of the cluster.|
|RegionId|String|Yes|cn-hangzhou|The region location ID.|
|ServiceName|String|Yes|HDFS|The name of the service. Sample values: HDFS and YARN.|
|AccessKeyId|String|No|xxx|The AccessKey ID.|

## Response parameters {#section_vdw_lv4_dgb .section}

|Name|Type|Example|Description|
|----|----|-------|-----------|
|RequestId|String|EBB4D49C-4064-4818-B3AE-4C6BE5FC8264|The ID of the request.|
|ServiceInfo| | |The information about the service.|
|ServiceName|String|YARN|The name of the service.|
|ServiceVersion|String|2.7.2|The version of the service.|
|ServiceStatus|String|INSTALLED|The status of the service.|
|NeedRestartInfo|String|""|The information about the components to be restarted.|
|NeedRestartNum|Integer|0|The number of components to be restarted.|
|ServiceActionList| | |The list of actions supported by the service.|
|ServiceName|String|YARN|The name of the service.|
|ComponentName|String|NodeManager|The corresponding component name to the action.|
|ActionName|String|CUSTOM\_COMMAND|The name of the action.|
|Command|String|refreshQueues|When the value of ActionName is CUSTOM\_COMMAND, specify the Command parameter. Services support different commands. Currently, YARN supports refreshQueues, enableCGroups, and disableCGroups.|
|DisplayName|String|RESTART NodeManager|The displayed name of the action.|
|ClusterServiceSummaryList| | |The overview of the service, primarily statistics about the components.|
|Key|String|NodeManager|The key, typically the component name.|
|DisplayName|String|NodeManager|The displayed name.|
|Value|String|20/20 Started|The value corresponding to the key.|
|Status|String|OK|The status.|
|Type|String|COMPONENT|The type of the key.|
|AlertInfo|String|""|The alert information corresponding to the key.|
|NeedRestartHostIdList|String|\["HostId1"\]|The IDs of the hosts where the components to be restarted are deployed.|
|NeedRestartComponentNameList|String|\["NodeManager","ResourceManager"\]|The name list of the components to be restarted.|

## Examples {#section_ydw_lv4_dgb .section}

-   Sample requests

    ```
    /? ClusterId=C-F32FB31D82954C64
    &RegionId=cn-hangzhou
    &ServiceName=HDFS
    &AccessKeyId=akId
    &<Common request parameters>
    ```

-   Successful response examples

    JSON format

    ```
    {
    	"code":"200",
    	"data":{
    		"RequestId":"7A23195A-BC03-4D82-BED5-90ED0D36F002",
    		"ServiceInfo":{
    			"ClusterServiceSummaryList":{
    				"ClusterServiceSummary":[
    					{
    						"AlertInfo":"",
    						"DisplayName":"NodeManager",
    						"Key":"NodeManager",
    						"Status":"OK",
    						"Type":"COMPONENT",
    						"Value":"20/20 Started"
    					},
    					{
    						"AlertInfo":"",
    						"DisplayName":"JobHistory",
    						"Key":"JobHistory",
    						"Status":"OK",
    						"Type":"COMPONENT",
    						"Value":"1/1 Started"
    					},
    					{
    						"AlertInfo":"",
    						"DisplayName":"Yarn Client",
    						"Key":"YarnInit",
    						"Status":"OK",
    						"Type":"COMPONENT",
    						"Value":"22/22 Installed"
    					},
    					{
    						"AlertInfo":"",
    						"DisplayName":"App Timeline Server",
    						"Key":"TimeLineServer",
    						"Status":"OK",
    						"Type":"COMPONENT",
    						"Value":"1/1 Started"
    					},
    					{
    						"AlertInfo":"",
    						"DisplayName":"WebAppProxyServer",
    						"Key":"WebAppProxyServer",
    						"Status":"OK",
    						"Type":"COMPONENT",
    						"Value":"1/1 Started"
    					},
    					{
    						"AlertInfo":"",
    						"DisplayName":"ResourceManager",
    						"Key":"ResourceManager",
    						"Status":"OK",
    						"Type":"COMPONENT",
    						"Value":"2/2 Started"
    					}
    				]
    			},
    			"NeedRestartComponentNameList":{
    				"Service":[]
    			},
    			"NeedRestartHostIdList":{
    				"Service":[]
    			},
    			"NeedRestartInfo":"",
    			"NeedRestartNum":0,
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
    						"ComponentName":"NodeManager",
    						"DisplayName":"RESTART NodeManager",
    						"ServiceName":"YARN"
    					},
    					{
    						"ActionName":"RESTART",
    						"ComponentName":"WebAppProxyServer",
    						"DisplayName":"RESTART WebAppProxyServer",
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
    			"ServiceName":"YARN",
    			"ServiceStatus":"INSTALLING",
    			"ServiceVersion":"2.7.2-1.3.1"
    		}
    	},
    	"requestId":"7A23195A-BC03-4D82-BED5-90ED0D36F002",
    	"successResponse":true
    }
    ```

-   Error response examples

    JSON format

    ```
    {
    	"code":"RAM.Permission.NotAllow",
    	"message":"It is not allow to execute this operation,please use RAM to authorize!",
    	"requestId":"9AEDC439-1F63-491D-B8C6-9737C372BF3A",
    	"successResponse":false
    }
    ```


## Error codes {#section_a2w_lv4_dgb .section}

[View error codes.](https://error-center.alibabacloud.com/status/product/Emr)

