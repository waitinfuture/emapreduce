# ModifyResourcePoolSchedulerType {#concept_hjh_515_2gb .concept}

调用 ModifyResourcePoolSchedulerType 接口修改资源池调度类型

## 请求参数 {#section_obj_yxl_2gb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|ClusterId|String|是|C-EBD62A703A430E23|集群 ID|
|RegionId|String|是|cn-hangzhou|区域 ID|
|SchedulerType|String|是|CAPACITY\_SCHEDULER|资源调度类型|
|AccessKeyId|String|是|LTAI8ljWyu7y\*\*\*\*|对应的阿里云 AccessKey ID 信息|

## 返回参数 {#section_vbj_yxl_2gb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|A544317F-4A60-4532-AC96-191B9D80420A|请求 ID|

## 示例 {#section_dcj_yxl_2gb .section}

-   请求示例

    ```
    /?Action=ModifyResourcePoolSchedulerType
    &ClusterId=C-EBD62A703A430E23
    &RegionId=cn-hangzhou
    &SchedulerType=CAPACITY_SCHEDULER
    &AccessKeyId=LTAI8ljWyu7y****
    &<公共请求参数>
    ```

-   正常返回示例

    JSON 格式

    ```
    {
    	"code":"200",
    	"requestId":"A544317F-4A60-4532-AC96-191B9D80420A",
    	"successResponse":true
    }
    ```

-   异常返回示例

    JSON 格式

    ```
    {
    	"code":"User.OtherUserResource.NotAllow",
    	"message":"It is not allowed to operate other user's resource",
    	"requestId":"A544317F-4A60-4532-AC96-191B9D80420AF",
    	"successResponse":false
    }
    ```


## 错误码 {#section_fcj_yxl_2gb .section}

[查看本产品错误码](https://error-center.alibabacloud.com/status/product/Emr)

