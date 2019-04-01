# CreateClusterWithTemplate {#concept_lbs_2jv_cgb .concept}

通过集群模版创建集群参数以及示例。

## 请求参数 {#section_vky_vjv_cgb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|TemplateBizId|String|是|CT-35498C56B3F12002|模版 ID|
|AccessKeyId|String|否|LTAI8ljWyu7y\*\*\*\*|AccessKeyId|
|UniqueTag|String|否|60a632f0-5909-430d-b65c-1b0f248e4947|用户自定义的业务标识，此字段的值对集群模版创建无影响|

## 返回参数 {#section_ynb_yjv_cgb1 .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|BF4FBAC6-B03E-4BFB-B6DB-EB53C34F2E22|请求 ID|
|ClusterId|String|C-D7958B72E59BAB88|集群 ID|
|EmrOrderId|String|0|EMR 服务订单 Id|
|MasterOrderId|String|0|Master 节点 Ecs 订单Id|
|CoreOrderId|String|0|Core节点 Ecs 订单Id|

## 示例 {#section_yr3_jz1_kfb .section}

-   请求示例

    ```
    /?TemplateBizId=CT-35498C56B3F12002
    &AccessKeyId=LTAI8ljWyu7y****
    &UniqueTag=60a632f0-5909-430d-b65c-1b0f248e4947
    &<公共请求参数>
    ```

-   正常返回示例

    JSON 格式

    ```
    {
    	"ClusterId":"C-956FA90598865F02",
    	"RequestId":"BF4FBAC6-B03E-4BFB-B6DB-EB53C34F2E22"
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


## 错误码 {#section_img_my5_cgb .section}

[查看本产品错误码](https://error-center.alibabacloud.com/status/product/Emr)

