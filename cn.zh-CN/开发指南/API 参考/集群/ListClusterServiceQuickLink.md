# ListClusterServiceQuickLink {#concept_dbp_2kw_dgb .concept}

调用 ListClusterServiceQuickLink 查询集群快捷链接列表

## 请求参数 {#section_pdw_lv4_dgb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|AccessKeyId|String|是|LTAI8ljWyu7y\*\*\*\*|AccessKeyId|
|ClusterId|String|是|C-4DBD656F20CEAE0B|集群 ID|
|ServiceName|String|否|SPARK|服务名称|
|RegionId|String|是|cn-hangzhou|区域 ID|

## 返回参数 {#section_vdw_lv4_dgb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|BF4FBAC6-B03E-4BFB-B6DB-EB53C34F2E22|请求 ID|
|QuickLinkList| | |快捷链接列表|
|ServiceName|String|HUE|服务名|
|ServiceDisplayName|String|Hue|服务显示名|
|QuickLinkAddress|String|knox.C-4DBD656F20CEAE0B.cn-hangzhou.emr.aliyuncs.com:8888|快捷链接地址|
|Protocol|String|https|链接协议|
|Port|String|8888|端口|
|Type|String|KNOX|类型|

## 示例 {#section_ydw_lv4_dgb .section}

-   请求示例

    ```
    /?ClusterId=C-4DBD656F20CEAE0B
    &RegionId=cn-hangzhou
    &AccessKeyId=LTAI8ljWyu7y****
    &ServiceName=SPARK
    &<公共请求参数>
    ```

-   正常返回示例

    JSON 格式

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

-   异常返回示例

    JSON 格式

    ```
    {
    	"quickLinkList":[],
    	"requestId":"6385E55F-0193-4E16-8341-90E0E87B0ABC"
    }
    ```


## 错误码 {#section_a2w_lv4_dgb .section}

[查看本产品错误码](https://error-center.alibabacloud.com/status/product/Emr)

