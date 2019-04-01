# DeleteResourcePool {#concept_obj_1cf_2gb .concept}

删除指定资源池参数以及示例

## 请求参数 {#section_p5v_ffb_kfb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|ClusterId|String|是|C-0E995C0EE7E5ECB3|集群 ID|
|RegionId|String|是|cn-hangzhou|区域 ID|
|ResourcePoolId|Long|是|115|资源池 ID|
|AccessKeyId|String|否|LTAI8ljWyu7y\*\*\*\*|AccessKeyId|

## 返回参数 {#section_vdw_lv4_dgb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|A544317F-4A60-4532-AC96-191B9D80420AF|请求 ID|

## 示例 {#section_ydw_lv4_dgb .section}

-   请求示例

    ```
    /?Action=DeleteResourcePool
    &ClusterId=C-EBD62A703A430E23
    &RegionId=cn-hangzhou
    &ResourcePoolId=115
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
    	"code":"4009",
    	"message":"The User Account maybe is out of service!",
    	"requestId":"7DF74E3B-5BEA-4365-8E67-4FD95095924F",
    	"successResponse":false
    }
    ```


## 错误码 {#section_a2w_lv4_dgb .section}

[查看本产品错误码](https://error-center.alibabacloud.com/status/product/Emr)

