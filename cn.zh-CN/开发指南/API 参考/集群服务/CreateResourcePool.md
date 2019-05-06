# CreateResourcePool {#concept_zrl_yx2_2gb .concept}

调用 CreateResourcePool 接口创建 YARN 资源池

## 请求参数 {#section_p5v_ffb_kfb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Active|Boolean|是|true|是否激活|
|ClusterId|String|是|C-0E995C0EE7E5ECB3|集群 ID|
|Name|String|是|default|资源池名称|
|PoolType|String|是|CAPACITY\_SCHEDULER|资源池类型，枚举值：CAPACITY\_SCHEDULER，FAIR\_SCHEDULER|
|RegionId|String|是|cn-hangzhou|区域 ID|
|AccessKeyId|String|是|LTAI8ljWyu7y\*\*\*\*|AccessKeyId|
|Config.N.Category|String|否|DEFAULT\_SETTINGS|配置类别，合法值DEFAULT\_SETTINGS，ACCESS\_CONTROL\_SETTINGS，QUEUE\_RESOURCE\_LIMIT，QUEUE\_SCHEDULING\_POLICY，QUEUE\_PREEMPTION,QUEUE\_SUBMISSION\_ACCESS\_CONTROL，QUEUE\_ADMINISTRATION\_ACCESS\_CONTROL|
|Config.N.ConfigKey|String|否|capacity|配置参数 Key|
|Config.N.ConfigValue|String|否|100|配置参数值|
|Config.N.Note|String|否|容量权重|参数备注|
|Config.N.TargetId|String|否|test|该字段废弃，不为空即可|
|Config.N.configType|String|否|RESOURCE\_POOL\_CONFIG|配置类型，此处填写 RESOURCE\_POOL\_CONFIG|
|Note|String|否|默认资源池|备注信息|
|YarnSiteConfig|String|否|configList|Yarn site 配置参数|

## 返回参数 {#section_vdw_lv4_dgb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|A544317F-4A60-4532-AC96-191B9D80420AF|请求 ID|

## 示例 {#section_ydw_lv4_dgb .section}

-   请求示例

    ```
    /?Active=true
    &ClusterId=C-0E995C0EE7E5ECB3
    &Name=default
    &PoolType=CAPACITY_SCHEDULER
    &RegionId=cn-hangzhou
    &AccessKeyId=LTAI8ljWyu7y****
    &Note=默认资源池
    &YarnSiteConfig=
    &Config.1.Category=DEFAULT_SETTINGS
    &Config.1.ConfigKey=capacity
    &Config.1.configType=RESOURCE_POOL_CONFIG
    &Config.1.ConfigValue=100
    &Config.1.1ote=容量权重
    &Config.1.TargetId=
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


## 错误码 {#section_a2w_lv4_dgb .section}

[查看本产品错误码](https://error-center.alibabacloud.com/status/product/Emr)

