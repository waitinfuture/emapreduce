# ListClusterInstalledService {#doc_api_Emr_ListClusterInstalledService .reference}

调用ListClusterInstalledService接口查询集群当前已安装的服务列表信息。

## 调试 {#api_explorer .section}

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=ListClusterInstalledService&type=RPC&version=2016-04-08)

## 请求参数 {#parameters .section}

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ListClusterInstalledService|系统规定参数。取值：ListClusterInstalledService。

 |
|ClusterId|String|是|C-0EF9B0EC8564\*\*\*\*|集群ID。

 |
|RegionId|String|是|cn-hangzhou|区域ID。

 |
|AccessKeyId|String|否|LTA\*\*\*\*u7gTm\*\*\*\*|阿里云AccessKey ID，标识访问者身份。

 |
|PageNumber|Integer|否|1|分页查询的页码。

 |
|PageSize|Integer|否|30|分页查询的每页数据量。

 |

## 返回数据 {#resultMapping .section}

|名称|类型|示例值|描述|
|--|--|---|--|
|ClusterInstalledServiceList| | |集群已安装的服务列表信息。

 |
|ServiceActionList| | |服务支持的操作信息列表。

 |
|ActionName|String|CUSTOM\_COMMAND|操作名称，枚举值：**CONFIGURE**、**START**、**STOP**、**RESTART**和**CUSTOM\_COMMAND**。

 |
|Command|String|rebalance|**CUSTOM\_COMMAND**的**commandName**。

 |
|ComponentName|String|NameNode|组件名称。

 |
|DisplayName|String|Rebalance|操作的展示名称。

 |
|ServiceName|String|HDFS|服务名称。

 |
|ServiceDisplayName|String|Ganglia|服务的展示名称。

 |
|ServiceEcmVersion|String|3.7.2.2.7|保留字段。

 |
|ServiceName|String|HDFS|服务名称。

 |
|ServiceVersion|String|3.7.2|服务版本。

 |
|abnormalNum|Integer|0|异常的组件数量。

 |
|comment|String|test|备注信息，可空。

 |
|needRestartNum|Integer|0|待重启的组件数量。

 |
|notStartedNum|Integer|0|已停止组件数量。

 |
|onlyClient|Boolean|false|是否仅有**Client**类型组件。

 |
|serviceStatus|String|INSTALLING|服务状态。

 |
|RequestId|String|88A94B4A-898D-448B-BC0B-6F0FE2CC64CA|接口调用的唯一ID标识。

 |

## 示例 {#demo .section}

请求示例

``` {#request_demo}

http(s)://[Endpoint]/?Action=ListClusterInstalledService
&ClusterId=C-0EF9B0EC8564****
&RegionId=cn-hangzhou
&<公共请求参数>

```

正常返回示例

`XML` 格式

``` {#xml_return_success_demo}
<ListClusterInstalledServiceResponse>
	  <code>200</code>
	  <data>
		    <clusterInstalledServiceList>
			      <abnormalNum>10</abnormalNum>
			      <needRestartNum>0</needRestartNum>
			      <notStartedNum>12</notStartedNum>
			      <onlyClient>false</onlyClient>
			      <serviceActionList>
				        <actionName>CONFIGURE</actionName>
				        <bizActionName>CONFIGURE</bizActionName>
				        <componentName>ALL COMPONENTS</componentName>
				        <displayName>Configure All Components</displayName>
				        <serviceName>HDFS</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>START</actionName>
				        <bizActionName>START</bizActionName>
				        <componentName>ALL COMPONENTS</componentName>
				        <displayName>Start All Components</displayName>
				        <serviceName>HDFS</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>STOP</actionName>
				        <bizActionName>STOP</bizActionName>
				        <componentName>ALL COMPONENTS</componentName>
				        <displayName>Stop All Components</displayName>
				        <serviceName>HDFS</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>RESTART</actionName>
				        <bizActionName>RESTART</bizActionName>
				        <componentName>ALL COMPONENTS</componentName>
				        <displayName>Restart All Components</displayName>
				        <serviceName>HDFS</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>RESTART</actionName>
				        <bizActionName>RESTART</bizActionName>
				        <componentName>DataNode</componentName>
				        <displayName>Restart DataNode</displayName>
				        <serviceName>HDFS</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>RESTART</actionName>
				        <bizActionName>RESTART</bizActionName>
				        <componentName>HttpFS</componentName>
				        <displayName>Restart HttpFS</displayName>
				        <serviceName>HDFS</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>RESTART</actionName>
				        <bizActionName>RESTART</bizActionName>
				        <componentName>JournalNode</componentName>
				        <displayName>Restart JournalNode</displayName>
				        <serviceName>HDFS</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>RESTART</actionName>
				        <bizActionName>RESTART</bizActionName>
				        <componentName>KMS</componentName>
				        <displayName>Restart KMS</displayName>
				        <serviceName>HDFS</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>RESTART</actionName>
				        <bizActionName>RESTART</bizActionName>
				        <componentName>NameNode</componentName>
				        <displayName>Restart NameNode</displayName>
				        <serviceName>HDFS</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>RESTART</actionName>
				        <bizActionName>RESTART</bizActionName>
				        <componentName>ZKFC</componentName>
				        <displayName>Restart ZKFC</displayName>
				        <serviceName>HDFS</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>CUSTOM_COMMAND</actionName>
				        <bizActionName>CUSTOM_COMMAND</bizActionName>
				        <command>rebalance</command>
				        <componentName>NameNode</componentName>
				        <displayName>Rebalance</displayName>
				        <serviceName>HDFS</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>CUSTOM_COMMAND</actionName>
				        <bizActionName>CUSTOM_COMMAND</bizActionName>
				        <command>stop_rebalance</command>
				        <componentName>NameNode</componentName>
				        <displayName>Stop Rebalance</displayName>
				        <serviceName>HDFS</serviceName>
			      </serviceActionList>
			      <serviceDisplayName>HDFS</serviceDisplayName>
			      <serviceName>HDFS</serviceName>
			      <serviceVersion>2.8.5</serviceVersion>
		    </clusterInstalledServiceList>
		    <clusterInstalledServiceList>
			      <abnormalNum>9</abnormalNum>
			      <needRestartNum>0</needRestartNum>
			      <notStartedNum>9</notStartedNum>
			      <onlyClient>false</onlyClient>
			      <serviceActionList>
				        <actionName>CONFIGURE</actionName>
				        <bizActionName>CONFIGURE</bizActionName>
				        <componentName>ALL COMPONENTS</componentName>
				        <displayName>Configure All Components</displayName>
				        <serviceName>YARN</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>START</actionName>
				        <bizActionName>START</bizActionName>
				        <componentName>ALL COMPONENTS</componentName>
				        <displayName>Start All Components</displayName>
				        <serviceName>YARN</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>STOP</actionName>
				        <bizActionName>STOP</bizActionName>
				        <componentName>ALL COMPONENTS</componentName>
				        <displayName>Stop All Components</displayName>
				        <serviceName>YARN</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>RESTART</actionName>
				        <bizActionName>RESTART</bizActionName>
				        <componentName>ALL COMPONENTS</componentName>
				        <displayName>Restart All Components</displayName>
				        <serviceName>YARN</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>RESTART</actionName>
				        <bizActionName>RESTART</bizActionName>
				        <componentName>JobHistory</componentName>
				        <displayName>Restart JobHistory</displayName>
				        <serviceName>YARN</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>RESTART</actionName>
				        <bizActionName>RESTART</bizActionName>
				        <componentName>NodeManager</componentName>
				        <displayName>Restart NodeManager</displayName>
				        <serviceName>YARN</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>RESTART</actionName>
				        <bizActionName>RESTART</bizActionName>
				        <componentName>ResourceManager</componentName>
				        <displayName>Restart ResourceManager</displayName>
				        <serviceName>YARN</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>RESTART</actionName>
				        <bizActionName>RESTART</bizActionName>
				        <componentName>TimeLineServer</componentName>
				        <displayName>Restart App Timeline Server</displayName>
				        <serviceName>YARN</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>RESTART</actionName>
				        <bizActionName>RESTART</bizActionName>
				        <componentName>WebAppProxyServer</componentName>
				        <displayName>Restart WebAppProxyServer</displayName>
				        <serviceName>YARN</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>CUSTOM_COMMAND</actionName>
				        <bizActionName>CUSTOM_COMMAND</bizActionName>
				        <command>refreshQueues</command>
				        <componentName>ResourceManager</componentName>
				        <displayName>Refresh Queues</displayName>
				        <serviceName>YARN</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>CUSTOM_COMMAND</actionName>
				        <bizActionName>CUSTOM_COMMAND</bizActionName>
				        <command>enableCGroups</command>
				        <componentName>NodeManager</componentName>
				        <displayName>Enable CGroups</displayName>
				        <serviceName>YARN</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>CUSTOM_COMMAND</actionName>
				        <bizActionName>CUSTOM_COMMAND</bizActionName>
				        <command>disableCGroups</command>
				        <componentName>NodeManager</componentName>
				        <displayName>Disable CGroups</displayName>
				        <serviceName>YARN</serviceName>
			      </serviceActionList>
			      <serviceDisplayName>YARN</serviceDisplayName>
			      <serviceName>YARN</serviceName>
			      <serviceVersion>2.8.5</serviceVersion>
		    </clusterInstalledServiceList>
		    <clusterInstalledServiceList>
			      <abnormalNum>6</abnormalNum>
			      <needRestartNum>0</needRestartNum>
			      <notStartedNum>6</notStartedNum>
			      <onlyClient>false</onlyClient>
			      <serviceActionList>
				        <actionName>CONFIGURE</actionName>
				        <bizActionName>CONFIGURE</bizActionName>
				        <componentName>ALL COMPONENTS</componentName>
				        <displayName>Configure All Components</displayName>
				        <serviceName>HIVE</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>START</actionName>
				        <bizActionName>START</bizActionName>
				        <componentName>ALL COMPONENTS</componentName>
				        <displayName>Start All Components</displayName>
				        <serviceName>HIVE</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>STOP</actionName>
				        <bizActionName>STOP</bizActionName>
				        <componentName>ALL COMPONENTS</componentName>
				        <displayName>Stop All Components</displayName>
				        <serviceName>HIVE</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>RESTART</actionName>
				        <bizActionName>RESTART</bizActionName>
				        <componentName>ALL COMPONENTS</componentName>
				        <displayName>Restart All Components</displayName>
				        <serviceName>HIVE</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>RESTART</actionName>
				        <bizActionName>RESTART</bizActionName>
				        <componentName>HiveMetaStore</componentName>
				        <displayName>Restart Hive MetaStore</displayName>
				        <serviceName>HIVE</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>RESTART</actionName>
				        <bizActionName>RESTART</bizActionName>
				        <componentName>HiveServer</componentName>
				        <displayName>Restart HiveServer2</displayName>
				        <serviceName>HIVE</serviceName>
			      </serviceActionList>
			      <serviceDisplayName>Hive</serviceDisplayName>
			      <serviceName>HIVE</serviceName>
			      <serviceVersion>3.1.1</serviceVersion>
		    </clusterInstalledServiceList>
		    <clusterInstalledServiceList>
			      <abnormalNum>8</abnormalNum>
			      <needRestartNum>0</needRestartNum>
			      <notStartedNum>8</notStartedNum>
			      <onlyClient>false</onlyClient>
			      <serviceActionList>
				        <actionName>CONFIGURE</actionName>
				        <bizActionName>CONFIGURE</bizActionName>
				        <componentName>ALL COMPONENTS</componentName>
				        <displayName>Configure All Components</displayName>
				        <serviceName>GANGLIA</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>START</actionName>
				        <bizActionName>START</bizActionName>
				        <componentName>ALL COMPONENTS</componentName>
				        <displayName>Start All Components</displayName>
				        <serviceName>GANGLIA</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>STOP</actionName>
				        <bizActionName>STOP</bizActionName>
				        <componentName>ALL COMPONENTS</componentName>
				        <displayName>Stop All Components</displayName>
				        <serviceName>GANGLIA</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>RESTART</actionName>
				        <bizActionName>RESTART</bizActionName>
				        <componentName>ALL COMPONENTS</componentName>
				        <displayName>Restart All Components</displayName>
				        <serviceName>GANGLIA</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>RESTART</actionName>
				        <bizActionName>RESTART</bizActionName>
				        <componentName>GMETAD</componentName>
				        <displayName>Restart Gmetad</displayName>
				        <serviceName>GANGLIA</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>RESTART</actionName>
				        <bizActionName>RESTART</bizActionName>
				        <componentName>GMOND</componentName>
				        <displayName>Restart Gmond</displayName>
				        <serviceName>GANGLIA</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>RESTART</actionName>
				        <bizActionName>RESTART</bizActionName>
				        <componentName>HTTPD</componentName>
				        <displayName>Restart Httpd</displayName>
				        <serviceName>GANGLIA</serviceName>
			      </serviceActionList>
			      <serviceDisplayName>Ganglia</serviceDisplayName>
			      <serviceName>GANGLIA</serviceName>
			      <serviceVersion>3.7.2</serviceVersion>
		    </clusterInstalledServiceList>
		    <clusterInstalledServiceList>
			      <abnormalNum>2</abnormalNum>
			      <needRestartNum>0</needRestartNum>
			      <notStartedNum>2</notStartedNum>
			      <onlyClient>false</onlyClient>
			      <serviceActionList>
				        <actionName>CONFIGURE</actionName>
				        <bizActionName>CONFIGURE</bizActionName>
				        <componentName>ALL COMPONENTS</componentName>
				        <displayName>Configure All Components</displayName>
				        <serviceName>SPARK</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>START</actionName>
				        <bizActionName>START</bizActionName>
				        <componentName>ALL COMPONENTS</componentName>
				        <displayName>Start All Components</displayName>
				        <serviceName>SPARK</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>STOP</actionName>
				        <bizActionName>STOP</bizActionName>
				        <componentName>ALL COMPONENTS</componentName>
				        <displayName>Stop All Components</displayName>
				        <serviceName>SPARK</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>RESTART</actionName>
				        <bizActionName>RESTART</bizActionName>
				        <componentName>ALL COMPONENTS</componentName>
				        <displayName>Restart All Components</displayName>
				        <serviceName>SPARK</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>RESTART</actionName>
				        <bizActionName>RESTART</bizActionName>
				        <componentName>SparkHistory</componentName>
				        <displayName>Restart SparkHistory</displayName>
				        <serviceName>SPARK</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>RESTART</actionName>
				        <bizActionName>RESTART</bizActionName>
				        <componentName>SparkThriftServer</componentName>
				        <displayName>Restart ThriftServer</displayName>
				        <serviceName>SPARK</serviceName>
			      </serviceActionList>
			      <serviceDisplayName>Spark</serviceDisplayName>
			      <serviceName>SPARK</serviceName>
			      <serviceVersion>2.4.3</serviceVersion>
		    </clusterInstalledServiceList>
		    <clusterInstalledServiceList>
			      <abnormalNum>3</abnormalNum>
			      <needRestartNum>0</needRestartNum>
			      <notStartedNum>3</notStartedNum>
			      <onlyClient>false</onlyClient>
			      <serviceActionList>
				        <actionName>CONFIGURE</actionName>
				        <bizActionName>CONFIGURE</bizActionName>
				        <componentName>ALL COMPONENTS</componentName>
				        <displayName>Configure All Components</displayName>
				        <serviceName>HUE</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>START</actionName>
				        <bizActionName>START</bizActionName>
				        <componentName>ALL COMPONENTS</componentName>
				        <displayName>Start All Components</displayName>
				        <serviceName>HUE</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>STOP</actionName>
				        <bizActionName>STOP</bizActionName>
				        <componentName>ALL COMPONENTS</componentName>
				        <displayName>Stop All Components</displayName>
				        <serviceName>HUE</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>RESTART</actionName>
				        <bizActionName>RESTART</bizActionName>
				        <componentName>ALL COMPONENTS</componentName>
				        <displayName>Restart All Components</displayName>
				        <serviceName>HUE</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>RESTART</actionName>
				        <bizActionName>RESTART</bizActionName>
				        <componentName>Hue</componentName>
				        <displayName>Restart Hue</displayName>
				        <serviceName>HUE</serviceName>
			      </serviceActionList>
			      <serviceDisplayName>Hue</serviceDisplayName>
			      <serviceName>HUE</serviceName>
			      <serviceVersion>4.4.0</serviceVersion>
		    </clusterInstalledServiceList>
		    <clusterInstalledServiceList>
			      <abnormalNum>1</abnormalNum>
			      <needRestartNum>0</needRestartNum>
			      <notStartedNum>1</notStartedNum>
			      <onlyClient>false</onlyClient>
			      <serviceActionList>
				        <actionName>CONFIGURE</actionName>
				        <bizActionName>CONFIGURE</bizActionName>
				        <componentName>ALL COMPONENTS</componentName>
				        <displayName>Configure All Components</displayName>
				        <serviceName>ZEPPELIN</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>START</actionName>
				        <bizActionName>START</bizActionName>
				        <componentName>ALL COMPONENTS</componentName>
				        <displayName>Start All Components</displayName>
				        <serviceName>ZEPPELIN</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>STOP</actionName>
				        <bizActionName>STOP</bizActionName>
				        <componentName>ALL COMPONENTS</componentName>
				        <displayName>Stop All Components</displayName>
				        <serviceName>ZEPPELIN</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>RESTART</actionName>
				        <bizActionName>RESTART</bizActionName>
				        <componentName>ALL COMPONENTS</componentName>
				        <displayName>Restart All Components</displayName>
				        <serviceName>ZEPPELIN</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>RESTART</actionName>
				        <bizActionName>RESTART</bizActionName>
				        <componentName>Zeppelin</componentName>
				        <displayName>Restart Zeppelin</displayName>
				        <serviceName>ZEPPELIN</serviceName>
			      </serviceActionList>
			      <serviceDisplayName>Zeppelin</serviceDisplayName>
			      <serviceName>ZEPPELIN</serviceName>
			      <serviceVersion>0.8.1</serviceVersion>
		    </clusterInstalledServiceList>
		    <clusterInstalledServiceList>
			      <abnormalNum>7</abnormalNum>
			      <needRestartNum>0</needRestartNum>
			      <notStartedNum>7</notStartedNum>
			      <onlyClient>false</onlyClient>
			      <serviceActionList>
				        <actionName>CONFIGURE</actionName>
				        <bizActionName>CONFIGURE</bizActionName>
				        <componentName>ALL COMPONENTS</componentName>
				        <displayName>Configure All Components</displayName>
				        <serviceName>TEZ</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>START</actionName>
				        <bizActionName>START</bizActionName>
				        <componentName>ALL COMPONENTS</componentName>
				        <displayName>Start All Components</displayName>
				        <serviceName>TEZ</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>STOP</actionName>
				        <bizActionName>STOP</bizActionName>
				        <componentName>ALL COMPONENTS</componentName>
				        <displayName>Stop All Components</displayName>
				        <serviceName>TEZ</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>RESTART</actionName>
				        <bizActionName>RESTART</bizActionName>
				        <componentName>ALL COMPONENTS</componentName>
				        <displayName>Restart All Components</displayName>
				        <serviceName>TEZ</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>RESTART</actionName>
				        <bizActionName>RESTART</bizActionName>
				        <componentName>TezInit</componentName>
				        <displayName>Restart Tez Client</displayName>
				        <serviceName>TEZ</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>RESTART</actionName>
				        <bizActionName>RESTART</bizActionName>
				        <componentName>Tomcat</componentName>
				        <displayName>Restart Tomcat</displayName>
				        <serviceName>TEZ</serviceName>
			      </serviceActionList>
			      <serviceDisplayName>Tez</serviceDisplayName>
			      <serviceName>TEZ</serviceName>
			      <serviceVersion>0.9.1-1.1.0</serviceVersion>
		    </clusterInstalledServiceList>
		    <clusterInstalledServiceList>
			      <abnormalNum>0</abnormalNum>
			      <needRestartNum>0</needRestartNum>
			      <notStartedNum>0</notStartedNum>
			      <onlyClient>false</onlyClient>
			      <serviceActionList>
				        <actionName>CONFIGURE</actionName>
				        <bizActionName>CONFIGURE</bizActionName>
				        <componentName>ALL COMPONENTS</componentName>
				        <displayName>Configure All Components</displayName>
				        <serviceName>SQOOP</serviceName>
			      </serviceActionList>
			      <serviceDisplayName>Sqoop</serviceDisplayName>
			      <serviceName>SQOOP</serviceName>
			      <serviceVersion>1.4.7-1.0.0</serviceVersion>
		    </clusterInstalledServiceList>
		    <clusterInstalledServiceList>
			      <abnormalNum>0</abnormalNum>
			      <needRestartNum>0</needRestartNum>
			      <notStartedNum>0</notStartedNum>
			      <onlyClient>false</onlyClient>
			      <serviceActionList>
				        <actionName>CONFIGURE</actionName>
				        <bizActionName>CONFIGURE</bizActionName>
				        <componentName>ALL COMPONENTS</componentName>
				        <displayName>Configure All Components</displayName>
				        <serviceName>PIG</serviceName>
			      </serviceActionList>
			      <serviceDisplayName>Pig</serviceDisplayName>
			      <serviceName>PIG</serviceName>
			      <serviceVersion>0.14.0</serviceVersion>
		    </clusterInstalledServiceList>
		    <clusterInstalledServiceList>
			      <abnormalNum>3</abnormalNum>
			      <needRestartNum>0</needRestartNum>
			      <notStartedNum>3</notStartedNum>
			      <onlyClient>false</onlyClient>
			      <serviceActionList>
				        <actionName>CONFIGURE</actionName>
				        <bizActionName>CONFIGURE</bizActionName>
				        <componentName>ALL COMPONENTS</componentName>
				        <displayName>Configure All Components</displayName>
				        <serviceName>KNOX</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>START</actionName>
				        <bizActionName>START</bizActionName>
				        <componentName>ALL COMPONENTS</componentName>
				        <displayName>Start All Components</displayName>
				        <serviceName>KNOX</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>STOP</actionName>
				        <bizActionName>STOP</bizActionName>
				        <componentName>ALL COMPONENTS</componentName>
				        <displayName>Stop All Components</displayName>
				        <serviceName>KNOX</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>RESTART</actionName>
				        <bizActionName>RESTART</bizActionName>
				        <componentName>ALL COMPONENTS</componentName>
				        <displayName>Restart All Components</displayName>
				        <serviceName>KNOX</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>RESTART</actionName>
				        <bizActionName>RESTART</bizActionName>
				        <componentName>KNOX</componentName>
				        <displayName>Restart Knox</displayName>
				        <serviceName>KNOX</serviceName>
			      </serviceActionList>
			      <serviceDisplayName>Knox</serviceDisplayName>
			      <serviceName>KNOX</serviceName>
			      <serviceVersion>1.1.0-1.0.2</serviceVersion>
		    </clusterInstalledServiceList>
		    <clusterInstalledServiceList>
			      <abnormalNum>1</abnormalNum>
			      <needRestartNum>0</needRestartNum>
			      <notStartedNum>1</notStartedNum>
			      <onlyClient>false</onlyClient>
			      <serviceActionList>
				        <actionName>CONFIGURE</actionName>
				        <bizActionName>CONFIGURE</bizActionName>
				        <componentName>ALL COMPONENTS</componentName>
				        <displayName>Configure All Components</displayName>
				        <serviceName>APACHEDS</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>START</actionName>
				        <bizActionName>START</bizActionName>
				        <componentName>ALL COMPONENTS</componentName>
				        <displayName>Start All Components</displayName>
				        <serviceName>APACHEDS</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>STOP</actionName>
				        <bizActionName>STOP</bizActionName>
				        <componentName>ALL COMPONENTS</componentName>
				        <displayName>Stop All Components</displayName>
				        <serviceName>APACHEDS</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>RESTART</actionName>
				        <bizActionName>RESTART</bizActionName>
				        <componentName>ALL COMPONENTS</componentName>
				        <displayName>Restart All Components</displayName>
				        <serviceName>APACHEDS</serviceName>
			      </serviceActionList>
			      <serviceActionList>
				        <actionName>RESTART</actionName>
				        <bizActionName>RESTART</bizActionName>
				        <componentName>ApacheDS</componentName>
				        <displayName>Restart ApacheDS</displayName>
				        <serviceName>APACHEDS</serviceName>
			      </serviceActionList>
			      <serviceDisplayName>ApacheDS</serviceDisplayName>
			      <serviceName>APACHEDS</serviceName>
			      <serviceVersion>2.0.0</serviceVersion>
		    </clusterInstalledServiceList>
		    <requestId>36317F42-D28B-4812-8E2E-AD4D37AAB69D</requestId>
	  </data>
	  <success>true</success>
</ListClusterInstalledServiceResponse>
```

`JSON` 格式

``` {#json_return_success_demo}
{
	"data":{
		"requestId":"36317F42-D28B-4812-8E2E-AD4D37AAB69D",
		"clusterInstalledServiceList":[
			{
				"abnormalNum":10,
				"serviceActionList":[
					{
						"componentName":"ALL COMPONENTS",
						"bizActionName":"CONFIGURE",
						"actionName":"CONFIGURE",
						"displayName":"Configure All Components",
						"serviceName":"HDFS"
					},
					{
						"componentName":"ALL COMPONENTS",
						"bizActionName":"START",
						"actionName":"START",
						"displayName":"Start All Components",
						"serviceName":"HDFS"
					},
					{
						"componentName":"ALL COMPONENTS",
						"bizActionName":"STOP",
						"actionName":"STOP",
						"displayName":"Stop All Components",
						"serviceName":"HDFS"
					},
					{
						"componentName":"ALL COMPONENTS",
						"bizActionName":"RESTART",
						"actionName":"RESTART",
						"displayName":"Restart All Components",
						"serviceName":"HDFS"
					},
					{
						"componentName":"DataNode",
						"bizActionName":"RESTART",
						"actionName":"RESTART",
						"displayName":"Restart DataNode",
						"serviceName":"HDFS"
					},
					{
						"componentName":"HttpFS",
						"bizActionName":"RESTART",
						"actionName":"RESTART",
						"displayName":"Restart HttpFS",
						"serviceName":"HDFS"
					},
					{
						"componentName":"JournalNode",
						"bizActionName":"RESTART",
						"actionName":"RESTART",
						"displayName":"Restart JournalNode",
						"serviceName":"HDFS"
					},
					{
						"componentName":"KMS",
						"bizActionName":"RESTART",
						"actionName":"RESTART",
						"displayName":"Restart KMS",
						"serviceName":"HDFS"
					},
					{
						"componentName":"NameNode",
						"bizActionName":"RESTART",
						"actionName":"RESTART",
						"displayName":"Restart NameNode",
						"serviceName":"HDFS"
					},
					{
						"componentName":"ZKFC",
						"bizActionName":"RESTART",
						"actionName":"RESTART",
						"displayName":"Restart ZKFC",
						"serviceName":"HDFS"
					},
					{
						"componentName":"NameNode",
						"command":"rebalance",
						"bizActionName":"CUSTOM_COMMAND",
						"actionName":"CUSTOM_COMMAND",
						"displayName":"Rebalance",
						"serviceName":"HDFS"
					},
					{
						"componentName":"NameNode",
						"command":"stop_rebalance",
						"bizActionName":"CUSTOM_COMMAND",
						"actionName":"CUSTOM_COMMAND",
						"displayName":"Stop Rebalance",
						"serviceName":"HDFS"
					}
				],
				"notStartedNum":12,
				"serviceVersion":"2.8.5",
				"onlyClient":false,
				"serviceDisplayName":"HDFS",
				"needRestartNum":0,
				"serviceName":"HDFS"
			},
			{
				"abnormalNum":9,
				"serviceActionList":[
					{
						"componentName":"ALL COMPONENTS",
						"bizActionName":"CONFIGURE",
						"actionName":"CONFIGURE",
						"displayName":"Configure All Components",
						"serviceName":"YARN"
					},
					{
						"componentName":"ALL COMPONENTS",
						"bizActionName":"START",
						"actionName":"START",
						"displayName":"Start All Components",
						"serviceName":"YARN"
					},
					{
						"componentName":"ALL COMPONENTS",
						"bizActionName":"STOP",
						"actionName":"STOP",
						"displayName":"Stop All Components",
						"serviceName":"YARN"
					},
					{
						"componentName":"ALL COMPONENTS",
						"bizActionName":"RESTART",
						"actionName":"RESTART",
						"displayName":"Restart All Components",
						"serviceName":"YARN"
					},
					{
						"componentName":"JobHistory",
						"bizActionName":"RESTART",
						"actionName":"RESTART",
						"displayName":"Restart JobHistory",
						"serviceName":"YARN"
					},
					{
						"componentName":"NodeManager",
						"bizActionName":"RESTART",
						"actionName":"RESTART",
						"displayName":"Restart NodeManager",
						"serviceName":"YARN"
					},
					{
						"componentName":"ResourceManager",
						"bizActionName":"RESTART",
						"actionName":"RESTART",
						"displayName":"Restart ResourceManager",
						"serviceName":"YARN"
					},
					{
						"componentName":"TimeLineServer",
						"bizActionName":"RESTART",
						"actionName":"RESTART",
						"displayName":"Restart App Timeline Server",
						"serviceName":"YARN"
					},
					{
						"componentName":"WebAppProxyServer",
						"bizActionName":"RESTART",
						"actionName":"RESTART",
						"displayName":"Restart WebAppProxyServer",
						"serviceName":"YARN"
					},
					{
						"componentName":"ResourceManager",
						"command":"refreshQueues",
						"bizActionName":"CUSTOM_COMMAND",
						"actionName":"CUSTOM_COMMAND",
						"displayName":"Refresh Queues",
						"serviceName":"YARN"
					},
					{
						"componentName":"NodeManager",
						"command":"enableCGroups",
						"bizActionName":"CUSTOM_COMMAND",
						"actionName":"CUSTOM_COMMAND",
						"displayName":"Enable CGroups",
						"serviceName":"YARN"
					},
					{
						"componentName":"NodeManager",
						"command":"disableCGroups",
						"bizActionName":"CUSTOM_COMMAND",
						"actionName":"CUSTOM_COMMAND",
						"displayName":"Disable CGroups",
						"serviceName":"YARN"
					}
				],
				"notStartedNum":9,
				"serviceVersion":"2.8.5",
				"onlyClient":false,
				"serviceDisplayName":"YARN",
				"needRestartNum":0,
				"serviceName":"YARN"
			},
			{
				"abnormalNum":6,
				"serviceActionList":[
					{
						"componentName":"ALL COMPONENTS",
						"bizActionName":"CONFIGURE",
						"actionName":"CONFIGURE",
						"displayName":"Configure All Components",
						"serviceName":"HIVE"
					},
					{
						"componentName":"ALL COMPONENTS",
						"bizActionName":"START",
						"actionName":"START",
						"displayName":"Start All Components",
						"serviceName":"HIVE"
					},
					{
						"componentName":"ALL COMPONENTS",
						"bizActionName":"STOP",
						"actionName":"STOP",
						"displayName":"Stop All Components",
						"serviceName":"HIVE"
					},
					{
						"componentName":"ALL COMPONENTS",
						"bizActionName":"RESTART",
						"actionName":"RESTART",
						"displayName":"Restart All Components",
						"serviceName":"HIVE"
					},
					{
						"componentName":"HiveMetaStore",
						"bizActionName":"RESTART",
						"actionName":"RESTART",
						"displayName":"Restart Hive MetaStore",
						"serviceName":"HIVE"
					},
					{
						"componentName":"HiveServer",
						"bizActionName":"RESTART",
						"actionName":"RESTART",
						"displayName":"Restart HiveServer2",
						"serviceName":"HIVE"
					}
				],
				"notStartedNum":6,
				"serviceVersion":"3.1.1",
				"onlyClient":false,
				"serviceDisplayName":"Hive",
				"needRestartNum":0,
				"serviceName":"HIVE"
			},
			{
				"abnormalNum":8,
				"serviceActionList":[
					{
						"componentName":"ALL COMPONENTS",
						"bizActionName":"CONFIGURE",
						"actionName":"CONFIGURE",
						"displayName":"Configure All Components",
						"serviceName":"GANGLIA"
					},
					{
						"componentName":"ALL COMPONENTS",
						"bizActionName":"START",
						"actionName":"START",
						"displayName":"Start All Components",
						"serviceName":"GANGLIA"
					},
					{
						"componentName":"ALL COMPONENTS",
						"bizActionName":"STOP",
						"actionName":"STOP",
						"displayName":"Stop All Components",
						"serviceName":"GANGLIA"
					},
					{
						"componentName":"ALL COMPONENTS",
						"bizActionName":"RESTART",
						"actionName":"RESTART",
						"displayName":"Restart All Components",
						"serviceName":"GANGLIA"
					},
					{
						"componentName":"GMETAD",
						"bizActionName":"RESTART",
						"actionName":"RESTART",
						"displayName":"Restart Gmetad",
						"serviceName":"GANGLIA"
					},
					{
						"componentName":"GMOND",
						"bizActionName":"RESTART",
						"actionName":"RESTART",
						"displayName":"Restart Gmond",
						"serviceName":"GANGLIA"
					},
					{
						"componentName":"HTTPD",
						"bizActionName":"RESTART",
						"actionName":"RESTART",
						"displayName":"Restart Httpd",
						"serviceName":"GANGLIA"
					}
				],
				"notStartedNum":8,
				"serviceVersion":"3.7.2",
				"onlyClient":false,
				"serviceDisplayName":"Ganglia",
				"needRestartNum":0,
				"serviceName":"GANGLIA"
			},
			{
				"abnormalNum":2,
				"serviceActionList":[
					{
						"componentName":"ALL COMPONENTS",
						"bizActionName":"CONFIGURE",
						"actionName":"CONFIGURE",
						"displayName":"Configure All Components",
						"serviceName":"SPARK"
					},
					{
						"componentName":"ALL COMPONENTS",
						"bizActionName":"START",
						"actionName":"START",
						"displayName":"Start All Components",
						"serviceName":"SPARK"
					},
					{
						"componentName":"ALL COMPONENTS",
						"bizActionName":"STOP",
						"actionName":"STOP",
						"displayName":"Stop All Components",
						"serviceName":"SPARK"
					},
					{
						"componentName":"ALL COMPONENTS",
						"bizActionName":"RESTART",
						"actionName":"RESTART",
						"displayName":"Restart All Components",
						"serviceName":"SPARK"
					},
					{
						"componentName":"SparkHistory",
						"bizActionName":"RESTART",
						"actionName":"RESTART",
						"displayName":"Restart SparkHistory",
						"serviceName":"SPARK"
					},
					{
						"componentName":"SparkThriftServer",
						"bizActionName":"RESTART",
						"actionName":"RESTART",
						"displayName":"Restart ThriftServer",
						"serviceName":"SPARK"
					}
				],
				"notStartedNum":2,
				"serviceVersion":"2.4.3",
				"onlyClient":false,
				"serviceDisplayName":"Spark",
				"needRestartNum":0,
				"serviceName":"SPARK"
			},
			{
				"abnormalNum":3,
				"serviceActionList":[
					{
						"componentName":"ALL COMPONENTS",
						"bizActionName":"CONFIGURE",
						"actionName":"CONFIGURE",
						"displayName":"Configure All Components",
						"serviceName":"HUE"
					},
					{
						"componentName":"ALL COMPONENTS",
						"bizActionName":"START",
						"actionName":"START",
						"displayName":"Start All Components",
						"serviceName":"HUE"
					},
					{
						"componentName":"ALL COMPONENTS",
						"bizActionName":"STOP",
						"actionName":"STOP",
						"displayName":"Stop All Components",
						"serviceName":"HUE"
					},
					{
						"componentName":"ALL COMPONENTS",
						"bizActionName":"RESTART",
						"actionName":"RESTART",
						"displayName":"Restart All Components",
						"serviceName":"HUE"
					},
					{
						"componentName":"Hue",
						"bizActionName":"RESTART",
						"actionName":"RESTART",
						"displayName":"Restart Hue",
						"serviceName":"HUE"
					}
				],
				"notStartedNum":3,
				"serviceVersion":"4.4.0",
				"onlyClient":false,
				"serviceDisplayName":"Hue",
				"needRestartNum":0,
				"serviceName":"HUE"
			},
			{
				"abnormalNum":1,
				"serviceActionList":[
					{
						"componentName":"ALL COMPONENTS",
						"bizActionName":"CONFIGURE",
						"actionName":"CONFIGURE",
						"displayName":"Configure All Components",
						"serviceName":"ZEPPELIN"
					},
					{
						"componentName":"ALL COMPONENTS",
						"bizActionName":"START",
						"actionName":"START",
						"displayName":"Start All Components",
						"serviceName":"ZEPPELIN"
					},
					{
						"componentName":"ALL COMPONENTS",
						"bizActionName":"STOP",
						"actionName":"STOP",
						"displayName":"Stop All Components",
						"serviceName":"ZEPPELIN"
					},
					{
						"componentName":"ALL COMPONENTS",
						"bizActionName":"RESTART",
						"actionName":"RESTART",
						"displayName":"Restart All Components",
						"serviceName":"ZEPPELIN"
					},
					{
						"componentName":"Zeppelin",
						"bizActionName":"RESTART",
						"actionName":"RESTART",
						"displayName":"Restart Zeppelin",
						"serviceName":"ZEPPELIN"
					}
				],
				"notStartedNum":1,
				"serviceVersion":"0.8.1",
				"onlyClient":false,
				"serviceDisplayName":"Zeppelin",
				"needRestartNum":0,
				"serviceName":"ZEPPELIN"
			},
			{
				"abnormalNum":7,
				"serviceActionList":[
					{
						"componentName":"ALL COMPONENTS",
						"bizActionName":"CONFIGURE",
						"actionName":"CONFIGURE",
						"displayName":"Configure All Components",
						"serviceName":"TEZ"
					},
					{
						"componentName":"ALL COMPONENTS",
						"bizActionName":"START",
						"actionName":"START",
						"displayName":"Start All Components",
						"serviceName":"TEZ"
					},
					{
						"componentName":"ALL COMPONENTS",
						"bizActionName":"STOP",
						"actionName":"STOP",
						"displayName":"Stop All Components",
						"serviceName":"TEZ"
					},
					{
						"componentName":"ALL COMPONENTS",
						"bizActionName":"RESTART",
						"actionName":"RESTART",
						"displayName":"Restart All Components",
						"serviceName":"TEZ"
					},
					{
						"componentName":"TezInit",
						"bizActionName":"RESTART",
						"actionName":"RESTART",
						"displayName":"Restart Tez Client",
						"serviceName":"TEZ"
					},
					{
						"componentName":"Tomcat",
						"bizActionName":"RESTART",
						"actionName":"RESTART",
						"displayName":"Restart Tomcat",
						"serviceName":"TEZ"
					}
				],
				"notStartedNum":7,
				"serviceVersion":"0.9.1-1.1.0",
				"onlyClient":false,
				"serviceDisplayName":"Tez",
				"needRestartNum":0,
				"serviceName":"TEZ"
			},
			{
				"abnormalNum":0,
				"serviceActionList":[
					{
						"componentName":"ALL COMPONENTS",
						"bizActionName":"CONFIGURE",
						"actionName":"CONFIGURE",
						"displayName":"Configure All Components",
						"serviceName":"SQOOP"
					}
				],
				"notStartedNum":0,
				"serviceVersion":"1.4.7-1.0.0",
				"onlyClient":false,
				"serviceDisplayName":"Sqoop",
				"needRestartNum":0,
				"serviceName":"SQOOP"
			},
			{
				"abnormalNum":0,
				"serviceActionList":[
					{
						"componentName":"ALL COMPONENTS",
						"bizActionName":"CONFIGURE",
						"actionName":"CONFIGURE",
						"displayName":"Configure All Components",
						"serviceName":"PIG"
					}
				],
				"notStartedNum":0,
				"serviceVersion":"0.14.0",
				"onlyClient":false,
				"serviceDisplayName":"Pig",
				"needRestartNum":0,
				"serviceName":"PIG"
			},
			{
				"abnormalNum":3,
				"serviceActionList":[
					{
						"componentName":"ALL COMPONENTS",
						"bizActionName":"CONFIGURE",
						"actionName":"CONFIGURE",
						"displayName":"Configure All Components",
						"serviceName":"KNOX"
					},
					{
						"componentName":"ALL COMPONENTS",
						"bizActionName":"START",
						"actionName":"START",
						"displayName":"Start All Components",
						"serviceName":"KNOX"
					},
					{
						"componentName":"ALL COMPONENTS",
						"bizActionName":"STOP",
						"actionName":"STOP",
						"displayName":"Stop All Components",
						"serviceName":"KNOX"
					},
					{
						"componentName":"ALL COMPONENTS",
						"bizActionName":"RESTART",
						"actionName":"RESTART",
						"displayName":"Restart All Components",
						"serviceName":"KNOX"
					},
					{
						"componentName":"KNOX",
						"bizActionName":"RESTART",
						"actionName":"RESTART",
						"displayName":"Restart Knox",
						"serviceName":"KNOX"
					}
				],
				"notStartedNum":3,
				"serviceVersion":"1.1.0-1.0.2",
				"onlyClient":false,
				"serviceDisplayName":"Knox",
				"needRestartNum":0,
				"serviceName":"KNOX"
			},
			{
				"abnormalNum":1,
				"serviceActionList":[
					{
						"componentName":"ALL COMPONENTS",
						"bizActionName":"CONFIGURE",
						"actionName":"CONFIGURE",
						"displayName":"Configure All Components",
						"serviceName":"APACHEDS"
					},
					{
						"componentName":"ALL COMPONENTS",
						"bizActionName":"START",
						"actionName":"START",
						"displayName":"Start All Components",
						"serviceName":"APACHEDS"
					},
					{
						"componentName":"ALL COMPONENTS",
						"bizActionName":"STOP",
						"actionName":"STOP",
						"displayName":"Stop All Components",
						"serviceName":"APACHEDS"
					},
					{
						"componentName":"ALL COMPONENTS",
						"bizActionName":"RESTART",
						"actionName":"RESTART",
						"displayName":"Restart All Components",
						"serviceName":"APACHEDS"
					},
					{
						"componentName":"ApacheDS",
						"bizActionName":"RESTART",
						"actionName":"RESTART",
						"displayName":"Restart ApacheDS",
						"serviceName":"APACHEDS"
					}
				],
				"notStartedNum":1,
				"serviceVersion":"2.0.0",
				"onlyClient":false,
				"serviceDisplayName":"ApacheDS",
				"needRestartNum":0,
				"serviceName":"APACHEDS"
			}
		]
	},
	"code":"200",
	"success":true
}
```

## 错误码 { .section}

访问[错误中心](https://error-center.alibabacloud.com/status/product/Emr)查看更多错误码。

