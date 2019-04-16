# ModifyResourceQueue {#concept_inn_nc5_2gb .concept}

调用 ModifyResourceQueue 接口修改资源队列

## 请求参数 {#section_obj_yxl_2gb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|ClusterId|String|是|C-EBD62A703A430E23|集群 ID|
|Id|String|是|116|资源池队列 ID|
|RegionId|String|是|cn-hangzhou|区域 ID|
|ResourcePoolId|Long|是|115|资源池 ID|
|AccessKeyId|String|是|LTAI8ljWyu7y\*\*\*\*|对应的阿里云 AccessKey ID 信息|
|Config.N.Category|String|否|QUEUE\_RESOURCE\_LIMIT|参数类别|
|Config.N.ConfigKey|String|否|capacity|参数 Key|
|Config.N.ConfigValue|String|否|100|参数值|
|Config.N.Id|Long|否|200|参数 ID|
|Config.N.Note|String|否|容量权重|参数描述|
|Leaf|Boolean|否|false|是否叶子节点|
|Name|String|否|queue 2|队列名称|
|ParentQueueId|Long|否|115|父资源队列 ID|
|QualifiedName|String|否|test|无需填写|

## 返回参数 {#section_vbj_yxl_2gb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|A544317F-4A60-4532-AC96-191B9D80420A|请求 ID|

## 示例 {#section_dcj_yxl_2gb .section}

-   请求示例

    ```
    /?Action=ModifyResourceQueue
    &ClusterId=C-0E995C0EE7E5ECB3
    &Id=116
    &RegionId=cn-hangzhou
    &ResourcePoolId=115
    &AccessKeyId=LTAI8ljWyu7y****
    &Leaf=false
    &Name=pool2
    &ParentQueueId=115
    &QualifiedName=test
    &Config.1.Category=QUEUE_RESOURCE_LIMIT
    &Config.1.ConfigKey=capacity
    &Config.1.ConfigValue=100
    &Config.1.Id=200
    &Config.1.1ote=容量权重
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
    	"code":"AuthRealNameNotPass",
    	"message":"User real name authenticate failed!",
    	"requestId":"A544317F-4A60-4532-AC96-191B9D80420AF",
    	"successResponse":false
    }
    ```


## 错误码 {#section_fcj_yxl_2gb .section}

[查看本产品错误码](https://error-center.alibabacloud.com/status/product/Emr)

