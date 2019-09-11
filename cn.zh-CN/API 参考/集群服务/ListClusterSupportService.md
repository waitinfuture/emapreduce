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
	  <data>
		    <pageNumber>3.22.0.1.0-cl</pageNumber>
		    <requestId>EA5FB7F2-BDDA-49C8-B9ED-9C4D3F5D27E8</requestId>
		    <supportServiceList>
			      <serviceDisplayName>BigBoot</serviceDisplayName>
			      <serviceEcmVersion>1.0.0.1.2</serviceEcmVersion>
			      <serviceName>BIGBOOT</serviceName>
			      <serviceVersion>1.0.0</serviceVersion>
		    </supportServiceList>
		    <supportServiceList>
			      <serviceDisplayName>SmartData</serviceDisplayName>
			      <serviceEcmVersion>1.0.0.1.5</serviceEcmVersion>
			      <serviceName>SMARTDATA</serviceName>
			      <serviceVersion>1.0.0</serviceVersion>
		    </supportServiceList>
		    <supportServiceList>
			      <serviceDisplayName>Ganglia</serviceDisplayName>
			      <serviceEcmVersion>3.7.2.9.7</serviceEcmVersion>
			      <serviceName>GANGLIA</serviceName>
			      <serviceVersion>3.7.2</serviceVersion>
		    </supportServiceList>
		    <supportServiceList>
			      <serviceDisplayName>Impala</serviceDisplayName>
			      <serviceEcmVersion>2.12.2.0.1.4</serviceEcmVersion>
			      <serviceName>IMPALA</serviceName>
			      <serviceVersion>2.12.2-1.0.0</serviceVersion>
		    </supportServiceList>
		    <supportServiceList>
			      <serviceDisplayName>Presto</serviceDisplayName>
			      <serviceEcmVersion>0.213.0.1.5</serviceEcmVersion>
			      <serviceName>PRESTO</serviceName>
			      <serviceVersion>0.213</serviceVersion>
		    </supportServiceList>
		    <supportServiceList>
			      <serviceDisplayName>Oozie</serviceDisplayName>
			      <serviceEcmVersion>5.1.0.0.2</serviceEcmVersion>
			      <serviceName>OOZIE</serviceName>
			      <serviceVersion>5.1.0</serviceVersion>
		    </supportServiceList>
		    <supportServiceList>
			      <serviceDisplayName>ZooKeeper</serviceDisplayName>
			      <serviceEcmVersion>3.4.13.2.1</serviceEcmVersion>
			      <serviceName>ZOOKEEPER</serviceName>
			      <serviceVersion>3.4.13</serviceVersion>
		    </supportServiceList>
		    <supportServiceList>
			      <serviceDisplayName>HDFS</serviceDisplayName>
			      <serviceEcmVersion>2.8.5.2.0-cl</serviceEcmVersion>
			      <serviceName>HDFS</serviceName>
			      <serviceVersion>2.8.5-1.3.2</serviceVersion>
		    </supportServiceList>
		    <supportServiceList>
			      <serviceDisplayName>YARN</serviceDisplayName>
			      <serviceEcmVersion>2.8.5.2.0-cl</serviceEcmVersion>
			      <serviceName>YARN</serviceName>
			      <serviceVersion>2.8.5-1.3.2</serviceVersion>
		    </supportServiceList>
		    <supportServiceList>
			      <serviceDisplayName>Hive</serviceDisplayName>
			      <serviceEcmVersion>3.1.1.3.0</serviceEcmVersion>
			      <serviceName>HIVE</serviceName>
			      <serviceVersion>3.1.1-1.1.6</serviceVersion>
		    </supportServiceList>
		    <supportServiceList>
			      <serviceDisplayName>HBase</serviceDisplayName>
			      <serviceEcmVersion>1.4.9.1.2</serviceEcmVersion>
			      <serviceName>HBASE</serviceName>
			      <serviceVersion>1.4.9-1.0.0</serviceVersion>
		    </supportServiceList>
		    <supportServiceList>
			      <serviceDisplayName>Spark</serviceDisplayName>
			      <serviceEcmVersion>2.4.3.1.1</serviceEcmVersion>
			      <serviceName>SPARK</serviceName>
			      <serviceVersion>2.4.3-1.1.0</serviceVersion>
		    </supportServiceList>
		    <supportServiceList>
			      <serviceDisplayName>Pig</serviceDisplayName>
			      <serviceEcmVersion>0.14.0.1.1</serviceEcmVersion>
			      <serviceName>PIG</serviceName>
			      <serviceVersion>0.14.0</serviceVersion>
		    </supportServiceList>
		    <supportServiceList>
			      <serviceDisplayName>Phoenix</serviceDisplayName>
			      <serviceEcmVersion>4.14.1.1.1</serviceEcmVersion>
			      <serviceName>PHOENIX</serviceName>
			      <serviceVersion>4.14.1</serviceVersion>
		    </supportServiceList>
		    <supportServiceList>
			      <serviceDisplayName>Sqoop</serviceDisplayName>
			      <serviceEcmVersion>1.4.7.1.2</serviceEcmVersion>
			      <serviceName>SQOOP</serviceName>
			      <serviceVersion>1.4.7-1.0.0</serviceVersion>
		    </supportServiceList>
		    <supportServiceList>
			      <serviceDisplayName>Tez</serviceDisplayName>
			      <serviceEcmVersion>0.9.1.5.2</serviceEcmVersion>
			      <serviceName>TEZ</serviceName>
			      <serviceVersion>0.9.1-1.1.0</serviceVersion>
		    </supportServiceList>
		    <supportServiceList>
			      <serviceDisplayName>Hue</serviceDisplayName>
			      <serviceEcmVersion>4.4.0.1.0</serviceEcmVersion>
			      <serviceName>HUE</serviceName>
			      <serviceVersion>4.4.0</serviceVersion>
		    </supportServiceList>
		    <supportServiceList>
			      <serviceDisplayName>Zeppelin</serviceDisplayName>
			      <serviceEcmVersion>0.8.1.1.2</serviceEcmVersion>
			      <serviceName>ZEPPELIN</serviceName>
			      <serviceVersion>0.8.1-1.0.0</serviceVersion>
		    </supportServiceList>
		    <supportServiceList>
			      <serviceDisplayName>Storm</serviceDisplayName>
			      <serviceEcmVersion>1.2.2.1.1</serviceEcmVersion>
			      <serviceName>STORM</serviceName>
			      <serviceVersion>1.2.2</serviceVersion>
		    </supportServiceList>
		    <supportServiceList>
			      <serviceDisplayName>ApacheDS</serviceDisplayName>
			      <serviceEcmVersion>2.0.0.1.1</serviceEcmVersion>
			      <serviceName>APACHEDS</serviceName>
			      <serviceVersion>2.0.0</serviceVersion>
		    </supportServiceList>
		    <supportServiceList>
			      <serviceDisplayName>Knox</serviceDisplayName>
			      <serviceEcmVersion>1.1.0.1.6</serviceEcmVersion>
			      <serviceName>KNOX</serviceName>
			      <serviceVersion>1.1.0-1.0.2</serviceVersion>
		    </supportServiceList>
		    <supportServiceList>
			      <serviceDisplayName>Flink</serviceDisplayName>
			      <serviceEcmVersion>1.7.2.2.0</serviceEcmVersion>
			      <serviceName>FLINK</serviceName>
			      <serviceVersion>1.7.2-2.8.5-1.0.0</serviceVersion>
		    </supportServiceList>
		    <supportServiceList>
			      <serviceDisplayName>RANGER</serviceDisplayName>
			      <serviceEcmVersion>1.2.0.2.2</serviceEcmVersion>
			      <serviceName>RANGER</serviceName>
			      <serviceVersion>1.2.0-1.0.0</serviceVersion>
		    </supportServiceList>
		    <supportServiceList>
			      <serviceDisplayName>Superset</serviceDisplayName>
			      <serviceEcmVersion>0.28.1.2.0</serviceEcmVersion>
			      <serviceName>SUPERSET</serviceName>
			      <serviceVersion>0.28.1</serviceVersion>
		    </supportServiceList>
		    <supportServiceList>
			      <serviceDisplayName>analytics-zoo</serviceDisplayName>
			      <serviceEcmVersion>0.5.0.1.0</serviceEcmVersion>
			      <serviceName>ANALYTICS-ZOO</serviceName>
			      <serviceVersion>0.5.0</serviceVersion>
		    </supportServiceList>
		    <supportServiceList>
			      <serviceDisplayName>LIVY</serviceDisplayName>
			      <serviceEcmVersion>0.6.0.1.0</serviceEcmVersion>
			      <serviceName>LIVY</serviceName>
			      <serviceVersion>0.6.0-1.0.0</serviceVersion>
		    </supportServiceList>
		    <supportServiceList>
			      <serviceDisplayName>Flume</serviceDisplayName>
			      <serviceEcmVersion>1.8.0.4.1</serviceEcmVersion>
			      <serviceName>FLUME</serviceName>
			      <serviceVersion>1.8.0-1.3.1</serviceVersion>
		    </supportServiceList>
		    <totalCount>EMR</totalCount>
	  </data>
	  <success>true</success>
</ListClusterSupportServiceResponse>
```

`JSON` 格式

``` {#json_return_success_demo}
{
	"data":{
		"totalCount":"EMR",
		"requestId":"EA5FB7F2-BDDA-49C8-B9ED-9C4D3F5D27E8",
		"pageNumber":"3.22.0.1.0-cl",
		"supportServiceList":[
			{
				"serviceEcmVersion":"1.0.0.1.2",
				"serviceVersion":"1.0.0",
				"serviceDisplayName":"BigBoot",
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
				"serviceEcmVersion":"0.213.0.1.5",
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
				"serviceEcmVersion":"2.8.5.2.0-cl",
				"serviceVersion":"2.8.5-1.3.2",
				"serviceDisplayName":"HDFS",
				"serviceName":"HDFS"
			},
			{
				"serviceEcmVersion":"2.8.5.2.0-cl",
				"serviceVersion":"2.8.5-1.3.2",
				"serviceDisplayName":"YARN",
				"serviceName":"YARN"
			},
			{
				"serviceEcmVersion":"3.1.1.3.0",
				"serviceVersion":"3.1.1-1.1.6",
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
				"serviceEcmVersion":"2.4.3.1.1",
				"serviceVersion":"2.4.3-1.1.0",
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
				"serviceEcmVersion":"4.4.0.1.0",
				"serviceVersion":"4.4.0",
				"serviceDisplayName":"Hue",
				"serviceName":"HUE"
			},
			{
				"serviceEcmVersion":"0.8.1.1.2",
				"serviceVersion":"0.8.1-1.0.0",
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
				"serviceEcmVersion":"1.7.2.2.0",
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
				"serviceEcmVersion":"0.5.0.1.0",
				"serviceVersion":"0.5.0",
				"serviceDisplayName":"analytics-zoo",
				"serviceName":"ANALYTICS-ZOO"
			},
			{
				"serviceEcmVersion":"0.6.0.1.0",
				"serviceVersion":"0.6.0-1.0.0",
				"serviceDisplayName":"LIVY",
				"serviceName":"LIVY"
			},
			{
				"serviceEcmVersion":"1.8.0.4.1",
				"serviceVersion":"1.8.0-1.3.1",
				"serviceDisplayName":"Flume",
				"serviceName":"FLUME"
			}
		]
	},
	"success":true
}
```

## 错误码 { .section}

访问[错误中心](https://error-center.aliyun.com/status/product/Emr)查看更多错误码。

