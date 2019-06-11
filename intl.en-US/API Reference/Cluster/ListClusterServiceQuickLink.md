# ListClusterServiceQuickLink {#concept_dbp_2kw_dgb .concept}

You can call this operation to query the quick links of services that run on a cluster.

## Request parameters {#section_pdw_lv4_dgb .section}

|Parameter|Type|Required|Example|Description |
|---------|----|--------|-------|------------|
|AccessKeyId|String|Yes|LTAI8ljWyu7y\*\*\*\*|The AccessKey ID.|
|ClusterId|String|Yes|C-4DBD656F20CEAE0B|The ID of the cluster.|
|ServiceName|String|No|SPARK|The name of the service.|
|RegionId|String|Yes|cn-hangzhou|The region ID.|

## Response parameters {#section_vdw_lv4_dgb .section}

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|BF4FBAC6-B03E-4BFB-B6DB-EB53C34F2E22|The ID of the request.|
|QuickLinkList| | |The list of quick links.|
|ServiceName|String|HUE|The name of the service.|
|ServiceDisplayName|String|Hue|The display name of the service.|
|QuickLinkAddress|String|knox.C-4DBD656F20CEAE0B.cn-hangzhou.emr.aliyuncs.com:8888|The address of the quick link.|
|Protocol|String|https|The protocol.|
|Port|String|8888|The port number.|
|Type|String|KNOX|The authentication type.|

## Examples {#section_ydw_lv4_dgb .section}

-   Sample requests

    ```
    /? ClusterId=C-4DBD656F20CEAE0B
    &RegionId=cn-hangzhou 
    &AccessKeyId=LTAI8ljWyu7y****
    &ServiceName=SPARK 
    &<Common request parameters>
    ```

-   Successful response examples

    JSON format

    ```
    {
    	"quickLinkList":[
    		{
    			"port":"8443",
    			"protocol":"https",
    			"quickLinkAddress":"knox.C-4DBD656F20CEAE0B.cn-hangzhou.emr.aliyuncs.com:8443/gateway/cluster-topo/hdfs/",
    			"serviceDisplayName":"HDFS",
    			"serviceName":"HDFS",
    			"type":"KNOX"
    		},
    		{
    			"port":"8443",
    			"protocol":"https",
    			"quickLinkAddress":"knox.C-4DBD656F20CEAE0B.cn-hangzhou.emr.aliyuncs.com:8443/gateway/cluster-topo/yarn/",
    			"serviceDisplayName":"YARN",
    			"serviceName":"YARN",
    			"type":"KNOX"
    		},
    		{
    			"port":"8443",
    			"protocol":"https",
    			"quickLinkAddress":"knox.C-4DBD656F20CEAE0B.cn-hangzhou.emr.aliyuncs.com:8443/gateway/cluster-topo/sparkhistory/",
    			"serviceDisplayName":"Spark",
    			"serviceName":"SPARK",
    			"type":"KNOX"
    		},
    		{
    			"port":"8888",
    			"protocol":"http",
    			"quickLinkAddress":"knox.C-4DBD656F20CEAE0B.cn-hangzhou.emr.aliyuncs.com:8888",
    			"serviceDisplayName":"Hue",
    			"serviceName":"HUE",
    			"type":"DEFAULT"
    		},
    		{
    			"port":"8080",
    			"protocol":"http",
    			"quickLinkAddress":"knox.C-4DBD656F20CEAE0B.cn-hangzhou.emr.aliyuncs.com:8080",
    			"serviceDisplayName":"Zeppelin",
    			"serviceName":"ZEPPELIN",
    			"type":"DEFAULT"
    		},
    		{
    			"port":"8443",
    			"protocol":"https",
    			"quickLinkAddress":"knox.C-4DBD656F20CEAE0B.cn-hangzhou.emr.aliyuncs.com:8443/gateway/cluster-topo/ganglia/",
    			"serviceDisplayName":"Ganglia",
    			"serviceName":"GANGLIA",
    			"type":"KNOX"
    		}
    	],
    	"requestId":"162E738D-8EF8-4C26-AE55-42DA6857A043"
    }
    ```

-   Error response examples

    JSON format

    ```
    {
    	"quickLinkList":[],
    	"requestId":"6385E55F-0193-4E16-8341-90E0E87B0ABC"
    }
    ```


## Error codes {#section_a2w_lv4_dgb .section}

