# CreateClusterWithTemplate {#concept_lbs_2jv_cgb .concept}

You can call this operation to create a cluster by using a cluster template.

## Request parameters {#section_vky_vjv_cgb .section}

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|TemplateBizId|String|Yes|CT-35498C56B3F12002|The ID of the template.|
|AccessKeyId|String|Yes|LTAI8ljWyu7y\*\*\*\*|The AccessKey ID.|
|UniqueTag|String|No|60a632f0-5909-430d-b65c-1b0f248e4947|The user-defined unique tag.|

## Response parameters {#section_ynb_yjv_cgb1 .section}

|Type|Example|Description|
|----|-------|-----------|
|String|BF4FBAC6-B03E-4BFB-B6DB-EB53C34F2E22|The ID of the request.|
|String|C-D7958B72E59BAB88|The ID of the cluster.|

## Examples {#section_yr3_jz1_kfb .section}

-   Sample requests

    ```
    /? TemplateBizId=CT-35498C56B3F12002
    &AccessKeyId=LTAI8ljWyu7y****
    &UniqueTag=60a632f0-5909-430d-b65c-1b0f248e4947
    &<Common request parameters>
    ```

-   Successful response examples

    JSON format

    ```
    {
    	"ClusterId":"C-956FA90598865F02",
    	"RequestId":"BF4FBAC6-B03E-4BFB-B6DB-EB53C34F2E22"
    }
    ```

-   Error response examples

    JSON format

    ```
    {
    	"code":"RAM.Permission.NotAllow",
    	"message":"It is not allow to execute this operation,please use RAM to authorize!",
    	"requestId":"9AEDC439-1F63-491D-B8C6-9737C372BF3A",
    	"successResponse":false
    }
    ```


## Error codes {#section_img_my5_cgb .section}

