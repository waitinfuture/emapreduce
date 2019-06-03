# ListResourcePool {#concept_ow4_s1s_2gb .concept}

You can call this operation to view the resource pools.

## Request parameters {#section_obj_yxl_2gb .section}

|Name|Type|Required|Example|Description|
|----|----|--------|-------|-----------|
|ClusterId|String|Yes|C-EBD62A703A430E23|The ID of the cluster.|
|RegionId|String|Yes|cn-hangzhou|The region location ID.|
|PoolType|String|No|CAPACITY\_SCHEDULER|The type of the resource pool. Valid values: CAPACITY\_SCHEDULER and FAIR\_SCHEDULER.|
|AccessKeyId|String|No|xxx|The AccessKey ID.|

## Response parameters {#section_vbj_yxl_2gb .section}

|Name|Type|Example|Description|
|----|----|-------|-----------|
|RequestId|String|A544317F-4A60-4532-AC96-191B9D80420A|The ID of the request.|
|PageNumber|Integer|1|The current page number.|
|PageSize|Integer|100|The number of records shown on each page.|
|Total|Integer|10|The total number of records.|
|PoolInfoList| | |The list of resource pool information.|
|QueueList| | |The list of resource pool queues.|
|EcmResourcePoolConfigList| | |The list of resource pool configurations.|
|Id|Long|2926|The ID of the configuration item.|
|ConfigKey|String|minimum-user-limit-percent|The key of the configuration item.|
|ConfigValue|String|0|The value of the configuration item.|
|ConfigType|String|RESOURCE\_QUEUE\_CONFIG|The type of the configuration item.|
|Category|String|QUEUE\_RESOURCE\_LIMIT|The category of the configuration item.|
|Status|String|NORMAL|The status of the configuration item.|
|Note|String|test|The description of the configuration item.|
|EcmResourceQueue| | |The resource queue.|
|Id|Long|2928|The ID of the resource queue.|
|Name|String|DEFAULT2|The name of the resource queue.|
|QualifiedName|String|default|QualifiedName|
|QueueType|String|null|The type of the resource queue.|
|ParentQueueId|Long|116|The ID of the parent resource queue.|
|Leaf|Boolean|false|Indicates whether the queue node is a leaf node.|
|Status|String|NORMAL|The status of the queue.|
|UserId|String|15283423567645362|The account ID of the user.|
|ResourcePoolId|Long|116|The ID of the resource pool.|
|EcmResourcePoolConfigList| | |The list of resource pool configurations.|
|Id|Long|2926|The ID of the configuration item.|
|ConfigKey|String|minimum-user-limit-percent|The key of the configuration item.|
|ConfigValue|String|0|The value of the configuration item.|
|ConfigType|String|RESOURCE\_QUEUE\_CONFIG|The type of the configuration type.|
|Category|String|QUEUE\_RESOURCE\_LIMIT|The category of the configuration item.|
|Status|String|NORMAL|The status of the configuration item.|
|Note|String|test|The description of the configuration item.|
|EcmResourcePool| | |The resource pool.|
|Id|Long|116|The ID of the resource pool.|
|Name|String|DEFAULT|The name of the resource pool.|
|PoolType|String|CAPACITY\_SCHEDULER|The type of the resource pool.|
|Active|Boolean|true|Indicates whether the resource pool is activated.|
|Note|String|test|The description of the resource pool.|
|UserId|String|15283423567645362|The account ID of the user.|

## Examples {#section_dcj_yxl_2gb .section}

-   Sample requests

    ``` {#codeblock_mjj_li3_u47}
    /? ClusterId=C-EBD62A703A430E23
    &RegionId=cn-hangzhou 
    &AccessKeyId=xxx
    &PoolType=CAPACITY_SCHEDULER
    &<Common request parameters>
    ```

-   Successful response examples

    JSON format

    ``` {#codeblock_d8c_bph_0ym}
    {
        "code":"200",
        "data":{
            "PoolInfoList":{
                "PoolInfo":[
                    {
                        "EcmResourcePool":{
                            "Active":true,
                            "Id":116,
                            "Name":"DEFAULT",
                            "Note":"",
                            "PoolType":"CAPACITY_SCHEDULER",
                            "UserId":"1528342317645362",
                            "YarnSiteConfig":""
                        },
                        "EcmResourcePoolConfigList":{
                            "EcmResourcePoolConfig":[]
                        },
                        "QueueList":{
                            "Queue":[{
                                "EcmResourcePoolConfigList":{
                                    "EcmResourcePoolConfig":[
                                        {
                                            "Category":"QUEUE_RESOURCE_LIMIT",
                                            "ConfigKey":"capacity",
                                            "ConfigType":"RESOURCE_QUEUE_CONFIG",
                                            "ConfigValue":"100",
                                            "Id":2925,
                                            "Note":"",
                                            "Status":"NORMAL"
                                        },
                                        {
                                            "Category":"QUEUE_RESOURCE_LIMIT",
                                            "ConfigKey":"minimum-user-limit-percent",
                                            "ConfigType":"RESOURCE_QUEUE_CONFIG",
                                            "ConfigValue":"0",
                                            "Id":2926,
                                            "Note":"",
                                            "Status":"NORMAL"
                                        },
                                        {
                                            "Category":"QUEUE_RESOURCE_LIMIT",
                                            "ConfigKey":"maximum-capacity",
                                            "ConfigType":"RESOURCE_QUEUE_CONFIG",
                                            "ConfigValue":"0",
                                            "Id":2927,
                                            "Note":"",
                                            "Status":"NORMAL"
                                        },
                                        {
                                            "Category":"QUEUE_RESOURCE_LIMIT",
                                            "ConfigKey":"user-limit-factor",
                                            "ConfigType":"RESOURCE_QUEUE_CONFIG",
                                            "ConfigValue":"",
                                            "Id":2928,
                                            "Note":"",
                                            "Status":"NORMAL"
                                        },
                                        {
                                            "Category":"QUEUE_RESOURCE_LIMIT",
                                            "ConfigKey":"maximum-allocation-mb",
                                            "ConfigType":"RESOURCE_QUEUE_CONFIG",
                                            "ConfigValue":"",
                                            "Id":2929,
                                            "Note":"",
                                            "Status":"NORMAL"
                                        },
                                        {
                                            "Category":"QUEUE_RESOURCE_LIMIT",
                                            "ConfigKey":"maximum-allocation-vcores",
                                            "ConfigType":"RESOURCE_QUEUE_CONFIG",
                                            "ConfigValue":"",
                                            "Id":2930,
                                            "Note":"",
                                            "Status":"NORMAL"
                                        },
                                        {
                                            "Category":"QUEUE_RESOURCE_LIMIT",
                                            "ConfigKey":"maximum-applications",
                                            "ConfigType":"RESOURCE_QUEUE_CONFIG",
                                            "ConfigValue":"",
                                            "Id":2931,
                                            "Note":"",
                                            "Status":"NORMAL"
                                        },
                                        {
                                            "Category":"QUEUE_RESOURCE_LIMIT",
                                            "ConfigKey":"maximum-am-resource-percent",
                                            "ConfigType":"RESOURCE_QUEUE_CONFIG",
                                            "ConfigValue":"",
                                            "Id":2932,
                                            "Note":"",
                                            "Status":"NORMAL"
                                        },
                                        {
                                            "Category":"QUEUE_RESOURCE_LIMIT",
                                            "ConfigKey":"state",
                                            "ConfigType":"RESOURCE_QUEUE_CONFIG",
                                            "ConfigValue":"RUNNING",
                                            "Id":2933,
                                            "Note":"",
                                            "Status":"NORMAL"
                                        },
                                        {
                                            "Category":"QUEUE_SUBMISSION_ACCESS_CONTROL",
                                            "ConfigKey":"acl_submit_applications",
                                            "ConfigType":"RESOURCE_QUEUE_CONFIG",
                                            "ConfigValue":"",
                                            "Id":2934,
                                            "Note":"",
                                            "Status":"NORMAL"
                                        },
                                        {
                                            "Category":"QUEUE_ADMINISTRATION_ACCESS_CONTROL",
                                            "ConfigKey":"acl_administer_queue",
                                            "ConfigType":"RESOURCE_QUEUE_CONFIG",
                                            "ConfigValue":"",
                                            "Id":2935,
                                            "Note":"",
                                            "Status":"NORMAL"
                                        }
                                    ]
                                },
                                "EcmResourceQueue":{
                                    "Id":247,
                                    "Leaf":false,
                                    "Name":"default",
                                    "ParentQueueId":0,
                                    "QualifiedName":"default",
                                    "QueueType":"",
                                    "ResourcePoolId":116,
                                    "Status":"NORMAL",
                                    "UserId":"1528342317645362"
                                }
                            }]
                        }
                    },
                    {
                        "EcmResourcePool":{
                            "Active":true,
                            "Id":117,
                            "Name":"pool1",
                            "Note":"",
                            "PoolType":"CAPACITY_SCHEDULER",
                            "UserId":"1528342317645362",
                            "YarnSiteConfig":""
                        },
                        "EcmResourcePoolConfigList":{
                            "EcmResourcePoolConfig":[]
                        },
                        "QueueList":{
                            "Queue":[]
                        }
                    }
                ]
            },
            "RequestId":"213DD361-EF38-4A31-A909-96857AEE42DE"
        },
        "requestId":"213DD361-EF38-4A31-A909-96857AEE42DE",
        "successResponse":true
    }
    ```

-   Error response examples

    JSON format

    ``` {#codeblock_l2u_t5r_fjo}
    {
        "code":"User.OtherUserResource.NotAllow",
        "message":"It is not allowed to operate other user's resource",
        "requestId":"A544317F-4A60-4532-AC96-191B9D80420AF",
        "successResponse":false
    }
    ```


## Error codes {#section_fcj_yxl_2gb .section}

