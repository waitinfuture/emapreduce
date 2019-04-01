# ModifyResourcePool {#concept_vvk_ynt_2gb .concept}

更新资源池参数及示例

## 请求参数 {#section_obj_yxl_2gb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|ClusterId|String|是|C-EBD62A703A430E23|集群 ID|
|Id|String|是|116|资源池 ID|
|RegionId|String|是|cn-hangzhou|区域 ID|
|Active|Boolean|否|true|是否激活|
|Config.N.Category|String|否|DEFAULT\_SETTINGS|配置类别，合法值DEFAULT\_SETTINGS，ACCESS\_CONTROL\_SETTINGS,QUEUE\_RESOURCE\_LIMIT，QUEUE\_SCHEDULING\_POLICY，QUEUE\_PREEMPTION，QUEUE\_SUBMISSION\_ACCESS\_CONTROL，QUEUE\_ADMINISTRATION\_ACCESS\_CONTROL|
|Config.N.ConfigKey|String|否|capacity|配置参数 Key|
|Config.N.ConfigValue|String|否|100|配置参数值|
|Config.N.Id|String|否|10|配置参数 ID|
|Config.N.Note|String|否|容量权重|配置参数描述|
|Name|String|否|custompool|资源池名称|
|Yarnsiteconfig|String|否|Yarnsiteconfig|yarn配置列表|
|AccessKeyId|String|否|LTAI8ljWyu7y\*\*\*\*|对应的阿里云AccessKey ID信息|

## 返回参数 {#section_vbj_yxl_2gb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|A544317F-4A60-4532-AC96-191B9D80420A|请求 ID|

## 示例 {#section_dcj_yxl_2gb .section}

-   请求示例

    ```
    /?ClusterId=C-0E995C0EE7E5ECB3
    &Id=116
    &RegionId=cn-hangzhou
    &AccessKeyId=LTAI8ljWyu7y****
    &Active=true
    &Name=custompool
    &Yarnsiteconfig=Yarnsiteconfig
    &Config.1.Category=DEFAULT_SETTINGS
    &Config.1.ConfigKey=capacity
    &Config.1.ConfigValue=100
    &Config.1.Id=10
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

