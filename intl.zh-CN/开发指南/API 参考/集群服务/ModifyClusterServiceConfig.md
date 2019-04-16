# ModifyClusterServiceConfig {#concept_ods_wds_2gb .concept}

调用 ModifyClusterServiceConfig 接口修改集群指定服务的配置信息

## 请求参数 {#section_obj_yxl_2gb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|ClusterId|String|是|C-EBD62A703A430E23|集群 ID|
|RegionId|String|是|cn-hangzhou|区域 ID|
|ServiceName|String|是|TEZ|服务名称|
|Comment|String|否|modify tez config|本次修改的备注信息|
|ConfigParams|String|否|\{"tez-site":\{"tez.am.resource.memory.mb":"640"\}\}|本次修改的具体内容，JSON 格式的字符串|
|ConfigType|String|否|""|配置项类型|
|CustomConfigParams|String|否|\{"tez-site":\{"key1":\{"Value":"value1"\}\}\}|自定义配置项的修改内容|
|GroupId|String|否|G-xxx|主机组 ID|
|HostInstanceId|String|否|i-xxx|主机实例 ID，一般是 ecsID|
|AccessKeyId|String|否|LTAI8ljWyu7y\*\*\*\*|对应的阿里云 AccessKey ID 信息|

## 返回参数 {#section_vbj_yxl_2gb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|D9A09DDE-6BE7-473B-9E4B-B3CB762FD4B|请求 ID|

## 示例 {#section_dcj_yxl_2gb .section}

-   请求示例

    ```
    /?ClusterId=C-xxx
    &RegionId=cn-hangzhou
    &ServiceName=TEZ
    &AccessKeyId=LTAI8ljWyu7y****
    &Comment=modify tez config
    &ConfigParams={"tez-site":{"tez.am.resource.memory.mb":"640"}}
    &ConfigType=""
    &CustomConfigParams={"tez-site":{"key1":{"Value":"value1"}}}
    &GroupId=G-xxx
    &HostInstanceId=i-xxx
    &<公共请求参数>
    ```

-   正常返回示例

    JSON 格式

    ```
    {
    	"RequestId":"D9A09DDE-6BE7-473B-9E4B-B3CB762FD4B3"
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

