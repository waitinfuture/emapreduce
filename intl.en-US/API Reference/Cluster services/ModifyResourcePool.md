# ModifyResourcePool {#concept_vvk_ynt_2gb .concept}

You can call this operation to modify a resource pool.

## Request parameters {#section_obj_yxl_2gb .section}

|Name|Type|Required|Example|Description|
|----|----|--------|-------|-----------|
|ClusterId|String|Yes|C-EBD62A703A430E23|The ID of the cluster.|
|Id|String|Yes|116|The ID of the resource pool.|
|RegionId|String|Yes|cn-hangzhou|RegionID|
|Active|Boolean|No|true|Indicates whether the resource pool is activated.|
|Config.N.Category|String|No|DEFAULT\_SETTINGS|The category of the configuration. Valid values: DEFAULT\_SETTINGS,ACCESS\_CONTROL\_SETTINGS,QUEUE\_RESOURCE\_LIMIT,QUEUE\_SCHEDULING\_POLICY,QUEUE\_PREEMPTION,QUEUE\_SUBMISSION\_ACCESS\_CONTROL, and QUEUE\_ADMINISTRATION\_ACCESS\_CONTROL.|
|Config.N.ConfigKey|String|No|capacity|The key of the configuration item.|
|Config.N.ConfigValue|String|No|100|The value of the configuration item/|
|Config.N.Id|String|No|10|The ID of the configuration item.|
|Config.N.Note|String|No|capacity weight|The description of the configuration item.|
|Name|String|No|custompool|The name of the resource pool.|
|Yarnsiteconfig|String|No|Yarnsiteconfig|/|
|AccessKeyId|String|No|xxx|The AccessKey ID.|

## Response parameters {#section_vbj_yxl_2gb .section}

|Name|Type|Example|Description|
|----|----|-------|-----------|
|RequestId|String|A544317F-4A60-4532-AC96-191B9D80420A|The ID of the request.|

## Examples {#section_dcj_yxl_2gb .section}

-   Sample requests

    ``` {#codeblock_iof_py4_3st}
    /? ClusterId=C-0E995C0EE7E5ECB3
    &Id=116
    &RegionId=cn-hangzhou 
    &AccessKeyId=xxx
    &Active=true
    &Name=custompool
    &Yarnsiteconfig=Yarnsiteconfig
    &Config. 1. Category=DEFAULT_SETTINGS
    &Config. 1. ConfigKey=capacity
    &Config. 1. ConfigValue=100
    &Config. 1. Id=10
    &Config. 1.Note=capacity weight
    &<Common request parameters>
    ```

-   Successful response examples

    JSON format

    ``` {#codeblock_4ap_p7n_4ao}
    {
        "code":"200",
        "requestId":"A544317F-4A60-4532-AC96-191B9D80420A",
        "successResponse":true
    }
    ```

-   Error response examples

    JSON format

    ``` {#codeblock_uz4_ium_9ye}
    {
        "code":"AuthRealNameNotPass",
        "message":"User real name authenticate failed!",
        "requestId":"A544317F-4A60-4532-AC96-191B9D80420AF",
        "successResponse":false
    }
    ```


## Error codes {#section_fcj_yxl_2gb .section}

