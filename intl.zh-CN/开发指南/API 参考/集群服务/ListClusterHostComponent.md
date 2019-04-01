# ListClusterHostComponent {#concept_lbm_xxl_2gb .concept}

获取集群各个主机上安装的组件列表

## 请求参数 {#section_obj_yxl_2gb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|ClusterId|String|是|C-EBD62A703A430E23|集群 ID|
|RegionId|String|是|cn-hangzhou|集群对应的区域 ID|
|ComponentName|String|否|TezInit|待查询的组件名称|
|ComponentStatus|String|否|STARTED|待查询的组件状态|
|HostInstanceId|String|否|i-xxx|主机实例 ID，一般是 ecsId|
|HostName|String|否|emr-worker-1|主机名|
|HostRole|String|否|CORE|主机角色|
|PageNumber|Integer|否|1|分页查询的查询页码|
|PageSize|Integer|否|100|分页查询的每页记录数|
|ServiceName|String|否|TEZ|服务名称|
|AccessKeyId|String|否|LTAI8ljWyu7y\*\*\*\*|阿里云 AccessKey ID 信息|

## 返回参数 {#section_vbj_yxl_2gb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|DF202AC2-5D5D-4288-B608-B7B1595B5C7C|请求 ID|
|PageNumber|Integer|100|当前的页号|
|PageSize|Integer|1|每页的记录数|
|Total|Integer|4|记录总数|
|ComponentList| | |组件列表|
|ServiceName|String|TEZ|服务名称|
|ServiceDisplayName|String|TEZ|服务的显示名称|
|ComponentName|String|TezInit|组件名|
|ComponentDisplayName|String|Tez Client|组件的显示名称|
|Status|String|STARTED|组件状态|
|NeedRestart|Boolean|true|组件是否需要重启|
|HostId|String|111|主机 ID|
|ServerStatus|String|active|组件服务状态，比如高可用集群某些组件的 active 和standBy 状态|
|HostName|String|emr-worker-1|主机名|
|PublicIp|String|48.20.119.10|主机的公网 IP|
|PrivateIp|String|192.168.1.1|主机内网 IP|
|Role|String|CORE|主机角色|
|InstanceType|String|ecs.sn1.xlarge|主机的规格类型|
|Cpu|Integer|4|主机的cpu核心数|
|Memory|Integer|8|主机内存大小|
|HostInstanceId|String|i-xxx|主机实例 ID，一般是 ecsID|
|SerialNumber|String|x11-05e5-4d36-b773-8ae4106babd4|主机的 SerialNumber 信息|

## 示例 {#section_dcj_yxl_2gb .section}

-   请求示例

    ```
    /?ClusterId=C-F32FB31D82954C64
    &RegionId=cn-hangzhou
    &AccessKeyId=LTAI8ljWyu7y****
    &ComponentName=TezInit
    &ComponentStatus=STARTED
    &HostInstanceId=i-xxx
    &HostName=emr-worker-1
    &HostRole=CORE
    &PageNumber=1
    &PageSize=100
    &ServiceName=TEZ
    &<公共请求参数>
    ```

-   正常返回示例

    JSON 格式

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

