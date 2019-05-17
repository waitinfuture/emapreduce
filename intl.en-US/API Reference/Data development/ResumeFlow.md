# ResumeFlow {#concept_cf3_rml_ggb .concept}

You can call this operation to resume a workflow.

## Request parameters {#section_obj_yxl_2gb .section}

|Name|Type|Required|Example|Description|
|----|----|--------|-------|-----------|
|FlowInstanceId|String|Yes|FI-9DDAAA3ADA5F6FA2|The ID of the workflow instance.|
|ProjectId|String|Yes|FP-3535FE0BE5224A47|The ID of the project.|
|RegionId|String|Yes|cn-hangzhou|The ID of the region.|

## Response parameters {#section_vbj_yxl_2gb .section}

|Name|Type|Example|Description|
|----|----|-------|-----------|
|RequestId|String|B46F8A2A-B46B-415C-8A9C-B01B99B775A2|The ID of the request.|
|Data|Boolean|true|Indicates whether the workflow is resumed.|

## Examples {#section_dcj_yxl_2gb .section}

-   Sample requests

    ```
    
    /? Action=ResumeFlow
    &FlowInstanceId=FI-9DDAAA3ADA5F6FA2
    &ProjectId=FP-3535FE0BE5224A47 
    &RegionId=cn-hangzhou
    &<Common request parameters>>
    ```

-   Successful response examples

    JSON format

    ```
    {
    	"Data":true,
    	"RequestId":"B46F8A2A-B46B-415C-8A9C-B01B99B775A2"
    }
    ```

-   Error response examples

    JSON format

    ```
    {
    	"code":"FLOW_API_FAILED",
    	"message":"project not exist",
    	"requestId":"11BAFBD8-8509-4177-A26D-407505E73713",
    	"successResponse": true
    }
    ```


## Error codes {#section_fcj_yxl_2gb .section}

[View error codes.](https://error-center.alibabacloud.com/status/product/Emr)

