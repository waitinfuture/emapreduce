# DescribeClusterServiceConfigTag {#concept_tcy_1wk_2gb .concept}

调用 DescribeClusterServiceConfigTag 接口查询集群指定服务的配置标签列表

## 请求参数 {#section_p5v_ffb_kfb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|ClusterId|String|是|C-EBD62A703A430E23|集群 ID|
|RegionId|String|是|cn-hangzhou|区域 ID|
|ConfigTag|String|否|FILE\_NAME|标签名|
|ServiceName|String|否|TEZ|服务名称|
|AccessKeyId|String|是|LTAI8ljWyu7y\*\*\*\*|阿里云AccessKey ID信息|

## 返回参数 {#section_vdw_lv4_dgb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|EBB4D49C-4064-4818-B3AE-4C6BE5FC8264|请求 ID|
|ConfigTagList| | |标签列表|
|Tag|String|FILE\_NAME|标签名|
|TagDesc|String|所属文件|标签描述|
|ValueList| | |标签值列表|
|Value|String|tez-site|标签的 Value|
|ValueDesc|String|tez-site|标签描述|

## 示例 {#section_ydw_lv4_dgb .section}

-   请求示例

    ```
    /?ClusterId=C-F32FB31D82954C64
    &RegionId=cn-hangzhou
    &ConfigTag=FILE_NAME
    &ServiceName=TEZ
    &AccessKeyId=LTAI8ljWyu7y****
    &<公共请求参数>
    ```

-   正常返回示例

    JSON 格式

    ```
    {
    	"ConfigTagList":{
    		"ConfigTag":[
    			{
    				"Tag":"FILE_NAME",
    				"TagDesc":"所属文件",
    				"ValueList":{
    					"Value":[{
    						"Value":"tez-site",
    						"ValueDesc":"tez-site"
    					}]
    				}
    			},
    			{
    				"Tag":"PROPERTY_TYPE",
    				"TagDesc":"配置类型",
    				"ValueList":{
    					"Value":[
    						{
    							"Value":"BASIC",
    							"ValueDesc":"基础配置"
    						},
    						{
    							"Value":"ADVANCED",
    							"ValueDesc":"高级配置"
    						},
    						{
    							"Value":"INFORMATION",
    							"ValueDesc":"只读配置"
    						},
    						{
    							"Value":"DATA_PATH",
    							"ValueDesc":"数据路径"
    						},
    						{
    							"Value":"LOG_PATH",
    							"ValueDesc":"日志路径"
    						},
    						{
    							"Value":"LOG",
    							"ValueDesc":"日志相关"
    						},
    						{
    							"Value":"JVM",
    							"ValueDesc":"JVM相关"
    						},
    						{
    							"Value":"DATA",
    							"ValueDesc":"数据相关"
    						},
    						{
    							"Value":"DATABASE",
    							"ValueDesc":"数据库相关"
    						},
    						{
    							"Value":"PERFORMANCE",
    							"ValueDesc":"性能相关"
    						},
    						{
    							"Value":"TIME",
    							"ValueDesc":"时间相关"
    						},
    						{
    							"Value":"CODEC",
    							"ValueDesc":"编解码相关"
    						},
    						{
    							"Value":"OSS",
    							"ValueDesc":"OSS相关"
    						},
    						{
    							"Value":"PORT",
    							"ValueDesc":"地址端口"
    						},
    						{
    							"Value":"MEMORY",
    							"ValueDesc":"内存配置"
    						},
    						{
    							"Value":"DISK",
    							"ValueDesc":"磁盘相关"
    						},
    						{
    							"Value":"NETWORK",
    							"ValueDesc":"网络相关"
    						},
    						{
    							"Value":"PATH",
    							"ValueDesc":"文件路径"
    						},
    						{
    							"Value":"URI",
    							"ValueDesc":"URL或URI"
    						}
    					]
    				}
    			}
    		]
    	},
    	"RequestId":"B887B2A3-E770-4340-A556-D9FAF41ABF47"
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


## 错误码 {#section_a2w_lv4_dgb .section}

[查看本产品错误码](https://error-center.alibabacloud.com/status/product/Emr)

