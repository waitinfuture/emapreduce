# ReleaseCluster {#concept_ip5_dfb_kfb .concept}

You can call this operation to release a cluster.

## Request parameters {#section_p5v_ffb_kfb .section}

|Parameter|Type|Required|Example|Description |
|---------|----|--------|-------|------------|
|Id|String|Yes|C-D7958B72E59BAB88|The ID of the cluster.|
|RegionId|String|Yes|cn-hangzhou|The region ID.|
|AccessKeyId|String|Yes|LTAI8ljWyu7y\*\*\*\*|The AccessKey ID.|
|ForceRelease|Boolean|No|true|When you release a cluster and if you have enabled the logging feature, the system needs a few minutes to upload the logs to OSS. You can select Force Release to release the cluster without uploading logs.|

## Response parameters {#section_vdw_lv4_dgb .section}

|Parameter|Type|Example|Description |
|---------|----|-------|------------|
|RequestId|String|BF4FBAC6-B03E-4BFB-B6DB-EB53C34F2E22|The ID of the request.|

## Examples {#section_ydw_lv4_dgb .section}

-   Sample requests

    ```
    /? Id=C-D7958B72E59BAB88
    &RegionId=cn-hangzhou 
    &AccessKeyId=LTAI8ljWyu7y**** 
    &ForceRelease=true
    &<Common request parameters>
    ```

-   Successful response examples

    JSON format

    ```
    {
    	"RequestId":"1BD8FE28-82AC-4433-848A-37B4928430C4"
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


## Error codes {#section_a2w_lv4_dgb .section}

