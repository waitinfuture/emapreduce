# ListFlowProject {#concept_nhs_rvk_ggb .concept}

You can call this operation to view a list of projects.

## Request parameters {#section_obj_yxl_2gb .section}

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|NodeInstanceId|String|Yes|FE4BD156E939CF1F|The ID of the node instance.|
|Name|String|No|my\_project|The name of the project.|
|PageNumber|Integer|No|1|The page number to request.|
|PageSize|Integer|No|20|The number of records to show on each page.|
|ProjectId|String|No|FP-7A1018ADE9179EE1|Project ID|

## Response parameters {#section_vbj_yxl_2gb .section}

|Field|Type|Example|Description|
|-----|----|-------|-----------|
|RequestId|String|83B256D4-4E95-454B-AD08-799DF31D5556|The ID of the request.|
|PageNumber|Integer|1|The page number to request.|
|PageSize|Integer|20|The number of records to show on each page.|
|Total|Integer|1|The total number of records.|
|Projects| | |The list of projects.|
|Id|String|7A1018ADE9179EE1|The ID of the project.|
|GmtCreate|Long|1540796236000|The creation time.|
|GmtModified|Long|1540796236000|The modification time.|
|UserId|String|123456|The ID of the Alibaba Cloud account.|
|Name|String|my\_project|The name of the project.|
|Description|String|my project description|The description of the project.|

## Examples {#section_dcj_yxl_2gb .section}

-   Sample requests

    ```
    
    /? Action=ListFlowProject
    &RegionId=cn-hangzhou
    &Name=my_project
    &PageNumber=1
    &PageSize=20
    &ProjectId=FP-7A1018ADE9179EE1 
    &<Common request parameters>
    ```

-   Successful response examples

    JSON format

    ```
    {
    	"PageNumber":1,
    	"PageSize":1,
    	"projects": [
    		"Project":[{
    			"Description":"gh-alert_enhance_11",
    			"GmtCreate":1542957424000,
    			"GmtModified":1542964674000,
    			"Id":"FP-17AB3389E1AD9A34",
    			"Name":"gh-alert_enhance",
    			"UserId":"1250460021754461"
    		}]
    	},
    	"RequestId":"3177EF07-34F5-48D0-9BFD-3C33E70EB260",
    	"Total":34
    }
    ```

-   Error response examples

    JSON format

    ```
    {
    	"code":"FLOW_API_FAILED",
    	"message":"Invalid Param[pageSize should less than 250]",
    	"requestId":"5E866FC1-7BBD-4965-AC6B-4689A121FC88",
    	"successResponse":false
    }
    ```


## Error codes {#section_fcj_yxl_2gb .section}

[View error codes.](https://error-center.alibabacloud.com/status/product/Emr)

