# DeleteClusterTemplate {#concept_slg_pr4_dgb .concept}

You can call this operation to delete a cluster template.

## Request parameters {#section_tt3_xr4_dgb .section}

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|BizId|String|Yes|CT-35498C56B3F12002|The ID of the cluster template.|
|RegionId|String|Yes|cn-hangzhou|The region ID.|
|AccessKeyId|String|Yes|LTAI8ljWyu7y\*\*\*\*|The AccessKey ID.|

## Response parameters {#section_ynb_yjv_cgb1 .section}

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|F2BF8586-045D-4104-B00C-44A4AA619C05|The request ID.|

## Examples {#section_yr3_jz1_kfb .section}

-   Sample requests

    ```
    /? BizId=CT-35498C56B3F12002
    &RegionId=cn-hangzhou
    &AccessKeyId=LTAI8ljWyu7y**** 
    &<Common request parameters>
    ```

-   Successful response examples

    JSON format

    ```
    {
    	"requestId":"F2BF8586-045D-4104-B00C-44A4AA619C05"
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

