# ListClusterHostComponent {#concept_lbm_xxl_2gb .concept}

You can call this operation to view the component lists of all hosts in a specified cluster.

## Request parameters {#section_obj_yxl_2gb .section}

|Name|Type|Required|Example|Description|
|----|----|--------|-------|-----------|
|ClusterId|String|Yes|C-EBD62A703A430E23|The ID of the cluster.|
|RegionId|String|Yes|cn-hangzhou|The region location ID.|
|ComponentName|String|No|TezInit|The name of the component.|
|ComponentStatus|String|No|STARTED|The status of the component.|
|HostInstanceId|String|No|i-xxx|The instance ID.|
|HostName|String|No|emr-worker-1|The name of the host.|
|HostRole|String|No|CORE|The role of the host.|
|PageNumber|Integer|No|1|The page number to query.|
|PageSize|Integer|No|100|The number of records to show on each page.|
|ServiceName|String|No|Tez.|The name of the service.|
|AccessKeyId|String|No|xxx|The AccessKey ID.|

## Response parameters {#section_vbj_yxl_2gb .section}

|Name|Type|Example|Description|
|----|----|-------|-----------|
|RequestId|String|DF202AC2-5D5D-4288-B608-B7B1595B5C7C|The ID of the request.|
|PageNumber|Integer|100|The current page number.|
|PageSize|Integer|1|The number of records that are shown on each page.|
|Total|Integer|4|The total number of records.|
|ComponentList| | |The list of components.|
|ServiceName|String|Tez.|The name of the service.|
|ServiceDisplayName|String|Tez.|The displayed name of the service.|
|ComponentName|String|TezInit|The name of the component.|
|ComponentDisplayName|String|Tez Client|The displayed name of the component.|
|Status|String|STARTED|The status of the component.|
|NeedRestart|Boolean|true|Indicates whether the component needs to be restarted.|
|HostId|String|111|The ID of the host.|
|ServerStatus|String|active|The service status of the component. Valid values: active and standBY \(for some components on a high-availability cluster\).|
|HostName|String|emr-worker-1|The name of the host.|
|PublicIp|String|48.20.119.10|The public IP address of the host.|
|PrivateIp|String|192.168.1.1|The private IP address of the host.|
|Role|String|CORE|The role of the host.|
|InstanceType|String|ecs.sn1.xlarge|The instance type.|
|Cpu|Integer|4|The number of vCPUs.|
|Memory|Integer|8|The memory size of the host.|
|HostInstanceId|String|i-xxx|The instance ID of the host.|
|SerialNumber|String|x11-05e5-4d36-b773-8ae4106babd4|The serial number of the host.|

## Examples {#section_dcj_yxl_2gb .section}

-   Sample requests

    ```
    /? ClusterId=C-F32FB31D82954C64
    &RegionId=cn-hangzhou 
    &AccessKeyId=111
    &ComponentName=TezInit
    &ComponentStatus=STARTED
    &HostInstanceId=i-xxx 
    &HostName=emr-worker-1 
    &HostRole=CORE
    &PageNumber=1 
    &PageSize=100 
    &ServiceName=TEZ 
    &<Common request parameters>
    ```

-   Successful response examples

    JSON format

    ```
    {
    	"ComponentList":{
    		"Component":[
    			{
    				"ComponentDisplayName":"Tez Client",
    				"ComponentName":"TezInit",
    				"HostId":"124903",
    				"HostInstanceId":"i-xxx1",
    				"HostName":"emr-header-1",
    				"InstanceType":"ecs.n4.xlarge",
    				"NeedRestart":false,
    				"PrivateIp":"192.168.143.231",
    				"PublicIp":"47.97.106.27",
    				"Role":"MASTER",
    				"SerialNumber":"x11-05e5-4d36-b773-8ae4106babd4",
    				"ServiceDisplayName":"Tez",
    				"ServiceName":"TEZ",
    				"Status":"STARTED"
    			},
    			{
    				"ComponentDisplayName":"Tomcat",
    				"ComponentName":"Tomcat",
    				"HostId":"124903",
    				"HostInstanceId":"i-xxx2",
    				"HostName":"emr-header-1",
    				"InstanceType":"ecs.n4.xlarge",
    				"NeedRestart":false,
    				"PrivateIp":"192.168.143.231",
    				"PublicIp":"47.97.106.27",
    				"Role":"MASTER",
    				"SerialNumber":"1xx1-05e5-x-b773-xxx",
    				"ServiceDisplayName":"Tez",
    				"ServiceName":"TEZ",
    				"Status":"STARTED"
    			},
    			{
    				"ComponentDisplayName":"Tez Client",
    				"ComponentName":"TezInit",
    				"HostId":"124902",
    				"HostInstanceId":"i-xxx3",
    				"HostName":"emr-worker-1",
    				"InstanceType":"ecs.n4.xlarge",
    				"NeedRestart":false,
    				"PrivateIp":"192.168.143.232",
    				"PublicIp":"",
    				"Role":"CORE",
    				"SerialNumber":"asd1x1-af39-1c-32b27d83d4c3",
    				"ServiceDisplayName":"Tez",
    				"ServiceName":"TEZ",
    				"Status":"STARTED"
    			},
    			{
    				"ComponentDisplayName":"Tez Client",
    				"ComponentName":"TezInit",
    				"HostId":"124901",
    				"HostInstanceId":"i-xxx4",
    				"HostName":"emr-worker-2",
    				"InstanceType":"ecs.n4.xlarge",
    				"NeedRestart":false,
    				"PrivateIp":"192.168.143.233",
    				"PublicIp":"",
    				"Role":"CORE",
    				"SerialNumber":"1cc-2762-4d84-a430-xxx",
    				"ServiceDisplayName":"Tez",
    				"ServiceName":"TEZ",
    				"Status":"STARTED"
    			}
    		]
    	},
    	"PageNumber":1,
    	"PageSize":10,
    	"RequestId":"DF202AC2-5D5D-4288-B608-B7B1595B5C7C",
    	"Total":4
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


## Error codes {#section_fcj_yxl_2gb .section}

