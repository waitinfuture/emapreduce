# DescribeClusterResourcePoolSchedulerType {#concept_qpj_lmf_2gb .concept}

\[DO NOT TRANSLATE\]

## Request parameters {#section_p5v_ffb_kfb .section}

|Name|Type|Required|Example|Description|
|----|----|--------|-------|-----------|
|ClusterId|String|Yes|C-EBD62A703A430E23|The ID of the cluster.|
|RegionId|String|Yes|cn-hangzhou|The region location ID.|
|AccessKeyId|String|No|xxx|The|

## Response parameters {#section_vdw_lv4_dgb .section}

|Name|Type|Example|Description|
|----|----|-------|-----------|
|RequestId|String|51517046-1327-45CF-9C70-AEA03A5BE02|The ID of the request.|
|CurrentSchedulerType|String|CAPACITY\_SCHEDULER, FAIR\_SCHEDULER|The scheduler type.|
|SupportSchedulerType|String|CAPACITY\_SCHEDULER|The supported scheduler types.|

## Examples {#section_ydw_lv4_dgb .section}

-   Sample requests

    ```
    /? Action=DescribeClusterResourcePoolSchedulerType
    &ClusterId=C-EBD62A703A430E23
    &RegionId=cn-hangzhou 
    &AccessKeyId=xxx
    &<Common request parameters>
    ```

-   Successful response examples

    JSON format

    ```
    {
    	"code":"200",
    	"data":{
    		"CurrentSchedulerType":"CAPACITY_SCHEDULER",
    		"RequestId":"51517046-1327-45CF-9C70-AEA03A5BE025",
    		"SupportSchedulerType":"CAPACITY_SCHEDULER,FAIR_SCHEDULER"
    	},
    	"requestId":"51517046-1327-45CF-9C70-AEA03A5BE025",
    	"successResponse":true
    }
    ```

-   Error response examples

    JSON format

    ```
    {
    	"code":"User.OtherUserResource.NotAllow",
    	"message":"It is not allowed to operate other user's resource",
    	"requestId":"A544317F-4A60-4532-AC96-191B9D80420AF",
    	"successResponse":false
    }
    ```


## Error codes {#section_a2w_lv4_dgb .section}

