# DeleteResourceQueue {#concept_df1_5df_2gb .concept}

调用 DeleteResourceQueue 接口删除资源队列

## 请求参数 {#section_p5v_ffb_kfb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|ClusterId|String|是|C-0E995C0EE7E5ECB3|集群 ID|
|RegionId|String|是|cn-hangzhou|区域 ID|
|ResourceQueueId|Long|是|248|资源队列 ID|
|AccessKeyId|String|是|LTAI8ljWyu7y\*\*\*\*|AccessKeyId|

## 返回参数 {#section_vdw_lv4_dgb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|A544317F-4A60-4532-AC96-191B9D80420AF|请求 ID|

## 示例 {#section_ydw_lv4_dgb .section}

-   请求示例

    ```
    /?Action=DeleteResourceQueue
    &ClusterId=C-EBD62A703A430E23
    &RegionId=cn-hangzhou
    &ResourceQueueId=248
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
    	"code":"AuthRealNameNotPass",
    	"message":"User real name authenticate failed!",
    	"requestId":"A544317F-4A60-4532-AC96-191B9D80420AF",
    	"successResponse":false
    }
    ```


## 错误码 {#section_a2w_lv4_dgb .section}

[查看本产品错误码](https://error-center.alibabacloud.com/status/product/Emr)

