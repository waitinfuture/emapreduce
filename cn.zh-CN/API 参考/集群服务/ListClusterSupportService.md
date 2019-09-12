# ListClusterSupportService {#doc_api_Emr_ListClusterSupportService .reference}

调用ListClusterSupportService接口，查看集群支持的服务列表。

## 调试 {#api_explorer .section}

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=ListClusterSupportService&type=RPC&version=2016-04-08)

## 请求参数 {#parameters .section}

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ListClusterSupportService|系统规定参数。取值：ListClusterSupportService。

 |
|ClusterId|String|是|C-0EF9B0EC8564\*\*\*\*|集群ID。

 |
|RegionId|String|是|cn-hangzhou|地域ID。

 |
|AccessKeyId|String|否|LTA\*\*\*\*u7gTm\*\*\*\*|阿里云AccessKey ID，用于标识访问者身份。

 |

## 返回数据 {#resultMapping .section}

|名称|类型|示例值|描述|
|--|--|---|--|
|PageNumber|String|1|分布查询的页数。

 |
|RequestId|String|EA5FB7F2-BDDA-49C8-B9ED-9C4D3F5D27E8|请求的ID。

 |
|SupportServiceList| | |支持的服务列表信息。

 |
|SupportService| | |支持的服务列表信息。

 |
|ServiceDisplayName|String|Hive|服务展示名称。

 |
|ServiceEcmVersion|String|3.1.1.3.0|保留字段。

 |
|ServiceName|String|HIVE|服务名称。

 |
|ServiceVersion|String|3.1.1|服务版本。

 |
|TotalCount|String|30|查询总数。

 |

## 示例 {#demo .section}

请求示例

``` {#request_demo}

http(s)://[Endpoint]/?Action=ListClusterSupportService
&ClusterId=C-0EF9B0EC8564****
&RegionId=cn-hangzhou
&<公共请求参数>

```

正常返回示例

`XML` 格式

``` {#xml_return_success_demo}
<ListClusterSupportServiceResponse>
    <SupportServiceList>
        <SupportService>
            <ServiceEcmVersion>1.0.0.1.2</ServiceEcmVersion>
            <ServiceName>BIGBOOT</ServiceName>
            <ServiceVersion>1.0.0</ServiceVersion>
            <ServiceDisplayName>Bigboot</ServiceDisplayName>
        </SupportService>
        <SupportService>
            <ServiceEcmVersion>1.0.0.1.5</ServiceEcmVersion>
            <ServiceName>SMARTDATA</ServiceName>
            <ServiceVersion>1.0.0</ServiceVersion>
            <ServiceDisplayName>SmartData</ServiceDisplayName>
        </SupportService>
        <SupportService>
            <ServiceEcmVersion>3.7.2.9.7</ServiceEcmVersion>
            <ServiceName>GANGLIA</ServiceName>
            <ServiceVersion>3.7.2</ServiceVersion>
            <ServiceDisplayName>Ganglia</ServiceDisplayName>
        </SupportService>
        <SupportService>
            <ServiceEcmVersion>2.12.2.0.1.4</ServiceEcmVersion>
            <ServiceName>IMPALA</ServiceName>
            <ServiceVersion>2.12.2-1.0.0</ServiceVersion>
            <ServiceDisplayName>Impala</ServiceDisplayName>
        </SupportService>
        <SupportService>
            <ServiceEcmVersion>0.213.0.1.3</ServiceEcmVersion>
            <ServiceName>PRESTO</ServiceName>
            <ServiceVersion>0.213</ServiceVersion>
            <ServiceDisplayName>Presto</ServiceDisplayName>
        </SupportService>
        <SupportService>
            <ServiceEcmVersion>5.1.0.0.2</ServiceEcmVersion>
            <ServiceName>OOZIE</ServiceName>
            <ServiceVersion>5.1.0</ServiceVersion>
            <ServiceDisplayName>Oozie</ServiceDisplayName>
        </SupportService>
        <SupportService>
            <ServiceEcmVersion>3.4.13.2.1</ServiceEcmVersion>
            <ServiceName>ZOOKEEPER</ServiceName>
            <ServiceVersion>3.4.13</ServiceVersion>
            <ServiceDisplayName>ZooKeeper</ServiceDisplayName>
        </SupportService>
        <SupportService>
            <ServiceEcmVersion>2.8.5.1.5</ServiceEcmVersion>
            <ServiceName>HDFS</ServiceName>
            <ServiceVersion>2.8.5-1.3.0</ServiceVersion>
            <ServiceDisplayName>HDFS</ServiceDisplayName>
        </SupportService>
        <SupportService>
            <ServiceEcmVersion>2.8.5.1.5</ServiceEcmVersion>
            <ServiceName>YARN</ServiceName>
            <ServiceVersion>2.8.5-1.3.0</ServiceVersion>
            <ServiceDisplayName>YARN</ServiceDisplayName>
        </SupportService>
        <SupportService>
            <ServiceEcmVersion>3.1.1.2.0</ServiceEcmVersion>
            <ServiceName>HIVE</ServiceName>
            <ServiceVersion>3.1.1-1.1.5</ServiceVersion>
            <ServiceDisplayName>Hive</ServiceDisplayName>
        </SupportService>
        <SupportService>
            <ServiceEcmVersion>1.4.9.1.2</ServiceEcmVersion>
            <ServiceName>HBASE</ServiceName>
            <ServiceVersion>1.4.9-1.0.0</ServiceVersion>
            <ServiceDisplayName>HBase</ServiceDisplayName>
        </SupportService>
        <SupportService>
            <ServiceEcmVersion>2.4.2.0.1</ServiceEcmVersion>
            <ServiceName>SPARK</ServiceName>
            <ServiceVersion>2.4.2-1.0.0</ServiceVersion>
            <ServiceDisplayName>Spark</ServiceDisplayName>
        </SupportService>
        <SupportService>
            <ServiceEcmVersion>0.14.0.1.1</ServiceEcmVersion>
            <ServiceName>PIG</ServiceName>
            <ServiceVersion>0.14.0</ServiceVersion>
            <ServiceDisplayName>Pig</ServiceDisplayName>
        </SupportService>
        <SupportService>
            <ServiceEcmVersion>4.14.1.1.1</ServiceEcmVersion>
            <ServiceName>PHOENIX</ServiceName>
            <ServiceVersion>4.14.1</ServiceVersion>
            <ServiceDisplayName>Phoenix</ServiceDisplayName>
        </SupportService>
        <SupportService>
            <ServiceEcmVersion>1.4.7.1.2</ServiceEcmVersion>
            <ServiceName>SQOOP</ServiceName>
            <ServiceVersion>1.4.7-1.0.0</ServiceVersion>
            <ServiceDisplayName>Sqoop</ServiceDisplayName>
        </SupportService>
        <SupportService>
            <ServiceEcmVersion>0.9.1.5.2</ServiceEcmVersion>
            <ServiceName>TEZ</ServiceName>
            <ServiceVersion>0.9.1-1.1.0</ServiceVersion>
            <ServiceDisplayName>Tez</ServiceDisplayName>
        </SupportService>
        <SupportService>
            <ServiceEcmVersion>4.1.0.1.7</ServiceEcmVersion>
            <ServiceName>HUE</ServiceName>
            <ServiceVersion>4.1.0</ServiceVersion>
            <ServiceDisplayName>Hue</ServiceDisplayName>
        </SupportService>
        <SupportService>
            <ServiceEcmVersion>0.8.1.1.1</ServiceEcmVersion>
            <ServiceName>ZEPPELIN</ServiceName>
            <ServiceVersion>0.8.1</ServiceVersion>
            <ServiceDisplayName>Zeppelin</ServiceDisplayName>
        </SupportService>
        <SupportService>
            <ServiceEcmVersion>1.2.2.1.1</ServiceEcmVersion>
            <ServiceName>STORM</ServiceName>
            <ServiceVersion>1.2.2</ServiceVersion>
            <ServiceDisplayName>Storm</ServiceDisplayName>
        </SupportService>
        <SupportService>
            <ServiceEcmVersion>2.0.0.1.1</ServiceEcmVersion>
            <ServiceName>APACHEDS</ServiceName>
            <ServiceVersion>2.0.0</ServiceVersion>
            <ServiceDisplayName>ApacheDS</ServiceDisplayName>
        </SupportService>
        <SupportService>
            <ServiceEcmVersion>1.1.0.1.6</ServiceEcmVersion>
            <ServiceName>KNOX</ServiceName>
            <ServiceVersion>1.1.0-1.0.2</ServiceVersion>
            <ServiceDisplayName>Knox</ServiceDisplayName>
        </SupportService>
        <SupportService>
            <ServiceEcmVersion>1.7.2.1.1</ServiceEcmVersion>
            <ServiceName>FLINK</ServiceName>
            <ServiceVersion>1.7.2-2.8.5-1.0.0</ServiceVersion>
            <ServiceDisplayName>Flink</ServiceDisplayName>
        </SupportService>
        <SupportService>
            <ServiceEcmVersion>1.2.0.2.2</ServiceEcmVersion>
            <ServiceName>RANGER</ServiceName>
            <ServiceVersion>1.2.0-1.0.0</ServiceVersion>
            <ServiceDisplayName>RANGER</ServiceDisplayName>
        </SupportService>
        <SupportService>
            <ServiceEcmVersion>0.28.1.2.0</ServiceEcmVersion>
            <ServiceName>SUPERSET</ServiceName>
            <ServiceVersion>0.28.1</ServiceVersion>
            <ServiceDisplayName>Superset</ServiceDisplayName>
        </SupportService>
        <SupportService>
            <ServiceEcmVersion>0.6.0.1.0</ServiceEcmVersion>
            <ServiceName>LIVY</ServiceName>
            <ServiceVersion>0.6.0-1.0.0</ServiceVersion>
            <ServiceDisplayName>LIVY</ServiceDisplayName>
        </SupportService>
        <SupportService>
            <ServiceEcmVersion>1.8.0.4.0</ServiceEcmVersion>
            <ServiceName>FLUME</ServiceName>
            <ServiceVersion>1.8.0-1.3.0</ServiceVersion>
            <ServiceDisplayName>FLUME</ServiceDisplayName>
        </SupportService>
    </SupportServiceList>
    <RequestId>B4920367-A2FE-44FF-868C-D029E20E40CF</RequestId>
</ListClusterSupportServiceResponse>
```

`JSON` 格式

``` {#json_return_success_demo}
{
	"requestId":"D110BB6A-DEC7-4856-8399-C08974588598",
	"supportServiceList":[
		{
			"serviceEcmVersion":"1.0.0.1.2",
			"serviceVersion":"1.0.0",
			"serviceDisplayName":"Bigboot",
			"serviceName":"BIGBOOT"
		},
		{
			"serviceEcmVersion":"1.0.0.1.5",
			"serviceVersion":"1.0.0",
			"serviceDisplayName":"SmartData",
			"serviceName":"SMARTDATA"
		},
		{
			"serviceEcmVersion":"3.7.2.9.7",
			"serviceVersion":"3.7.2",
			"serviceDisplayName":"Ganglia",
			"serviceName":"GANGLIA"
		},
		{
			"serviceEcmVersion":"2.12.2.0.1.4",
			"serviceVersion":"2.12.2-1.0.0",
			"serviceDisplayName":"Impala",
			"serviceName":"IMPALA"
		},
		{
			"serviceEcmVersion":"0.213.0.1.3",
			"serviceVersion":"0.213",
			"serviceDisplayName":"Presto",
			"serviceName":"PRESTO"
		},
		{
			"serviceEcmVersion":"5.1.0.0.2",
			"serviceVersion":"5.1.0",
			"serviceDisplayName":"Oozie",
			"serviceName":"OOZIE"
		},
		{
			"serviceEcmVersion":"3.4.13.2.1",
			"serviceVersion":"3.4.13",
			"serviceDisplayName":"ZooKeeper",
			"serviceName":"ZOOKEEPER"
		},
		{
			"serviceEcmVersion":"2.8.5.1.5",
			"serviceVersion":"2.8.5-1.3.0",
			"serviceDisplayName":"HDFS",
			"serviceName":"HDFS"
		},
		{
			"serviceEcmVersion":"2.8.5.1.5",
			"serviceVersion":"2.8.5-1.3.0",
			"serviceDisplayName":"YARN",
			"serviceName":"YARN"
		},
		{
			"serviceEcmVersion":"3.1.1.2.0",
			"serviceVersion":"3.1.1-1.1.5",
			"serviceDisplayName":"Hive",
			"serviceName":"HIVE"
		},
		{
			"serviceEcmVersion":"1.4.9.1.2",
			"serviceVersion":"1.4.9-1.0.0",
			"serviceDisplayName":"HBase",
			"serviceName":"HBASE"
		},
		{
			"serviceEcmVersion":"2.4.2.0.1",
			"serviceVersion":"2.4.2-1.0.0",
			"serviceDisplayName":"Spark",
			"serviceName":"SPARK"
		},
		{
			"serviceEcmVersion":"0.14.0.1.1",
			"serviceVersion":"0.14.0",
			"serviceDisplayName":"Pig",
			"serviceName":"PIG"
		},
		{
			"serviceEcmVersion":"4.14.1.1.1",
			"serviceVersion":"4.14.1",
			"serviceDisplayName":"Phoenix",
			"serviceName":"PHOENIX"
		},
		{
			"serviceEcmVersion":"1.4.7.1.2",
			"serviceVersion":"1.4.7-1.0.0",
			"serviceDisplayName":"Sqoop",
			"serviceName":"SQOOP"
		},
		{
			"serviceEcmVersion":"0.9.1.5.2",
			"serviceVersion":"0.9.1-1.1.0",
			"serviceDisplayName":"Tez",
			"serviceName":"TEZ"
		},
		{
			"serviceEcmVersion":"4.1.0.1.7",
			"serviceVersion":"4.1.0",
			"serviceDisplayName":"Hue",
			"serviceName":"HUE"
		},
		{
			"serviceEcmVersion":"0.8.1.1.1",
			"serviceVersion":"0.8.1",
			"serviceDisplayName":"Zeppelin",
			"serviceName":"ZEPPELIN"
		},
		{
			"serviceEcmVersion":"1.2.2.1.1",
			"serviceVersion":"1.2.2",
			"serviceDisplayName":"Storm",
			"serviceName":"STORM"
		},
		{
			"serviceEcmVersion":"2.0.0.1.1",
			"serviceVersion":"2.0.0",
			"serviceDisplayName":"ApacheDS",
			"serviceName":"APACHEDS"
		},
		{
			"serviceEcmVersion":"1.1.0.1.6",
			"serviceVersion":"1.1.0-1.0.2",
			"serviceDisplayName":"Knox",
			"serviceName":"KNOX"
		},
		{
			"serviceEcmVersion":"1.7.2.1.1",
			"serviceVersion":"1.7.2-2.8.5-1.0.0",
			"serviceDisplayName":"Flink",
			"serviceName":"FLINK"
		},
		{
			"serviceEcmVersion":"1.2.0.2.2",
			"serviceVersion":"1.2.0-1.0.0",
			"serviceDisplayName":"RANGER",
			"serviceName":"RANGER"
		},
		{
			"serviceEcmVersion":"0.28.1.2.0",
			"serviceVersion":"0.28.1",
			"serviceDisplayName":"Superset",
			"serviceName":"SUPERSET"
		},
		{
			"serviceEcmVersion":"0.6.0.1.0",
			"serviceVersion":"0.6.0-1.0.0",
			"serviceDisplayName":"LIVY",
			"serviceName":"LIVY"
		},
		{
			"serviceEcmVersion":"1.8.0.4.0",
			"serviceVersion":"1.8.0-1.3.0",
			"serviceDisplayName":"FLUME",
			"serviceName":"FLUME"
		}
	]
}
```

## 错误码 { .section}

访问[错误中心](https://error-center.alibabacloud.com/status/product/Emr)查看更多错误码。

