# ListClusterInstalledService {#doc_api_Emr_ListClusterInstalledService .reference}

调用ListClusterInstalledService接口，查询集群当前已安装的服务列表信息。

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
|ClusterInstalledService| | |集群已安装的服务列表信息。

 |
|ServiceActionList| | |服务支持的操作信息列表。

 |
|ServiceAction| | |服务支持的操作信息列表。

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
    <ClusterInstalledServiceList>
        <ClusterInstalledService>
            <abnormalNum>0</abnormalNum>
            <ServiceActionList>
                <ServiceAction>
                    <ActionName>CONFIGURE</ActionName>
                    <ServiceName>HDFS</ServiceName>
                    <DisplayName>CONFIGURE All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>START</ActionName>
                    <ServiceName>HDFS</ServiceName>
                    <DisplayName>START All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>STOP</ActionName>
                    <ServiceName>HDFS</ServiceName>
                    <DisplayName>STOP All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>RESTART</ActionName>
                    <ServiceName>HDFS</ServiceName>
                    <DisplayName>RESTART All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>RESTART</ActionName>
                    <ServiceName>HDFS</ServiceName>
                    <DisplayName>RESTART DataNode</DisplayName>
                    <ComponentName>DataNode</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>RESTART</ActionName>
                    <ServiceName>HDFS</ServiceName>
                    <DisplayName>RESTART HttpFS</DisplayName>
                    <ComponentName>HttpFS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>RESTART</ActionName>
                    <ServiceName>HDFS</ServiceName>
                    <DisplayName>RESTART JournalNode</DisplayName>
                    <ComponentName>JournalNode</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>RESTART</ActionName>
                    <ServiceName>HDFS</ServiceName>
                    <DisplayName>RESTART KMS</DisplayName>
                    <ComponentName>KMS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>RESTART</ActionName>
                    <ServiceName>HDFS</ServiceName>
                    <DisplayName>RESTART NameNode</DisplayName>
                    <ComponentName>NameNode</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>RESTART</ActionName>
                    <ServiceName>HDFS</ServiceName>
                    <DisplayName>RESTART ZKFC</DisplayName>
                    <ComponentName>ZKFC</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <Command>rebalance</Command>
                    <ActionName>CUSTOM_COMMAND</ActionName>
                    <ServiceName>HDFS</ServiceName>
                    <DisplayName>rebalance</DisplayName>
                    <ComponentName>NameNode</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <Command>stop_rebalance</Command>
                    <ActionName>CUSTOM_COMMAND</ActionName>
                    <ServiceName>HDFS</ServiceName>
                    <DisplayName>stop_rebalance</DisplayName>
                    <ComponentName>NameNode</ComponentName>
                </ServiceAction>
            </ServiceActionList>
            <ServiceName>HDFS</ServiceName>
            <notStartedNum>2</notStartedNum>
            <onlyClient>false</onlyClient>
            <ServiceVersion>2.8.5</ServiceVersion>
            <ServiceDisplayName>HDFS</ServiceDisplayName>
            <needRestartNum>0</needRestartNum>
        </ClusterInstalledService>
        <ClusterInstalledService>
            <abnormalNum>0</abnormalNum>
            <ServiceActionList>
                <ServiceAction>
                    <ActionName>CONFIGURE</ActionName>
                    <ServiceName>YARN</ServiceName>
                    <DisplayName>CONFIGURE All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>START</ActionName>
                    <ServiceName>YARN</ServiceName>
                    <DisplayName>START All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>STOP</ActionName>
                    <ServiceName>YARN</ServiceName>
                    <DisplayName>STOP All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>RESTART</ActionName>
                    <ServiceName>YARN</ServiceName>
                    <DisplayName>RESTART All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>RESTART</ActionName>
                    <ServiceName>YARN</ServiceName>
                    <DisplayName>RESTART JobHistory</DisplayName>
                    <ComponentName>JobHistory</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>RESTART</ActionName>
                    <ServiceName>YARN</ServiceName>
                    <DisplayName>RESTART NodeManager</DisplayName>
                    <ComponentName>NodeManager</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>RESTART</ActionName>
                    <ServiceName>YARN</ServiceName>
                    <DisplayName>RESTART ResourceManager</DisplayName>
                    <ComponentName>ResourceManager</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>RESTART</ActionName>
                    <ServiceName>YARN</ServiceName>
                    <DisplayName>RESTART App Timeline Server</DisplayName>
                    <ComponentName>TimeLineServer</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>RESTART</ActionName>
                    <ServiceName>YARN</ServiceName>
                    <DisplayName>RESTART WebAppProxyServer</DisplayName>
                    <ComponentName>WebAppProxyServer</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <Command>refreshQueues</Command>
                    <ActionName>CUSTOM_COMMAND</ActionName>
                    <ServiceName>YARN</ServiceName>
                    <DisplayName>refreshQueues</DisplayName>
                    <ComponentName>ResourceManager</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <Command>enableCGroups</Command>
                    <ActionName>CUSTOM_COMMAND</ActionName>
                    <ServiceName>YARN</ServiceName>
                    <DisplayName>enableCGroups</DisplayName>
                    <ComponentName>NodeManager</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <Command>disableCGroups</Command>
                    <ActionName>CUSTOM_COMMAND</ActionName>
                    <ServiceName>YARN</ServiceName>
                    <DisplayName>disableCGroups</DisplayName>
                    <ComponentName>NodeManager</ComponentName>
                </ServiceAction>
            </ServiceActionList>
            <ServiceName>YARN</ServiceName>
            <notStartedNum>0</notStartedNum>
            <onlyClient>false</onlyClient>
            <ServiceVersion>2.8.5</ServiceVersion>
            <ServiceDisplayName>YARN</ServiceDisplayName>
            <needRestartNum>0</needRestartNum>
        </ClusterInstalledService>
        <ClusterInstalledService>
            <abnormalNum>0</abnormalNum>
            <ServiceActionList>
                <ServiceAction>
                    <ActionName>CONFIGURE</ActionName>
                    <ServiceName>HIVE</ServiceName>
                    <DisplayName>CONFIGURE All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>START</ActionName>
                    <ServiceName>HIVE</ServiceName>
                    <DisplayName>START All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>STOP</ActionName>
                    <ServiceName>HIVE</ServiceName>
                    <DisplayName>STOP All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>RESTART</ActionName>
                    <ServiceName>HIVE</ServiceName>
                    <DisplayName>RESTART All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>RESTART</ActionName>
                    <ServiceName>HIVE</ServiceName>
                    <DisplayName>RESTART Hive MetaStore</DisplayName>
                    <ComponentName>HiveMetaStore</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>RESTART</ActionName>
                    <ServiceName>HIVE</ServiceName>
                    <DisplayName>RESTART HiveServer2</DisplayName>
                    <ComponentName>HiveServer</ComponentName>
                </ServiceAction>
            </ServiceActionList>
            <ServiceName>HIVE</ServiceName>
            <notStartedNum>0</notStartedNum>
            <onlyClient>false</onlyClient>
            <ServiceVersion>3.1.1</ServiceVersion>
            <ServiceDisplayName>Hive</ServiceDisplayName>
            <needRestartNum>0</needRestartNum>
        </ClusterInstalledService>
        <ClusterInstalledService>
            <abnormalNum>0</abnormalNum>
            <ServiceActionList>
                <ServiceAction>
                    <ActionName>CONFIGURE</ActionName>
                    <ServiceName>GANGLIA</ServiceName>
                    <DisplayName>CONFIGURE All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>START</ActionName>
                    <ServiceName>GANGLIA</ServiceName>
                    <DisplayName>START All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>STOP</ActionName>
                    <ServiceName>GANGLIA</ServiceName>
                    <DisplayName>STOP All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>RESTART</ActionName>
                    <ServiceName>GANGLIA</ServiceName>
                    <DisplayName>RESTART All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>RESTART</ActionName>
                    <ServiceName>GANGLIA</ServiceName>
                    <DisplayName>RESTART Gmetad</DisplayName>
                    <ComponentName>GMETAD</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>RESTART</ActionName>
                    <ServiceName>GANGLIA</ServiceName>
                    <DisplayName>RESTART Gmond</DisplayName>
                    <ComponentName>GMOND</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>RESTART</ActionName>
                    <ServiceName>GANGLIA</ServiceName>
                    <DisplayName>RESTART Httpd</DisplayName>
                    <ComponentName>HTTPD</ComponentName>
                </ServiceAction>
            </ServiceActionList>
            <ServiceName>GANGLIA</ServiceName>
            <notStartedNum>0</notStartedNum>
            <onlyClient>false</onlyClient>
            <ServiceVersion>3.7.2</ServiceVersion>
            <ServiceDisplayName>Ganglia</ServiceDisplayName>
            <needRestartNum>0</needRestartNum>
        </ClusterInstalledService>
        <ClusterInstalledService>
            <abnormalNum>0</abnormalNum>
            <ServiceActionList>
                <ServiceAction>
                    <ActionName>CONFIGURE</ActionName>
                    <ServiceName>ZOOKEEPER</ServiceName>
                    <DisplayName>CONFIGURE All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>START</ActionName>
                    <ServiceName>ZOOKEEPER</ServiceName>
                    <DisplayName>START All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>STOP</ActionName>
                    <ServiceName>ZOOKEEPER</ServiceName>
                    <DisplayName>STOP All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>RESTART</ActionName>
                    <ServiceName>ZOOKEEPER</ServiceName>
                    <DisplayName>RESTART All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>RESTART</ActionName>
                    <ServiceName>ZOOKEEPER</ServiceName>
                    <DisplayName>RESTART ZooKeeper</DisplayName>
                    <ComponentName>ZOOKEEPER</ComponentName>
                </ServiceAction>
            </ServiceActionList>
            <ServiceName>ZOOKEEPER</ServiceName>
            <notStartedNum>0</notStartedNum>
            <onlyClient>false</onlyClient>
            <ServiceVersion>3.4.13</ServiceVersion>
            <ServiceDisplayName>ZooKeeper</ServiceDisplayName>
            <needRestartNum>0</needRestartNum>
        </ClusterInstalledService>
        <ClusterInstalledService>
            <abnormalNum>0</abnormalNum>
            <ServiceActionList>
                <ServiceAction>
                    <ActionName>CONFIGURE</ActionName>
                    <ServiceName>SPARK</ServiceName>
                    <DisplayName>CONFIGURE All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>START</ActionName>
                    <ServiceName>SPARK</ServiceName>
                    <DisplayName>START All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>STOP</ActionName>
                    <ServiceName>SPARK</ServiceName>
                    <DisplayName>STOP All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>RESTART</ActionName>
                    <ServiceName>SPARK</ServiceName>
                    <DisplayName>RESTART All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>RESTART</ActionName>
                    <ServiceName>SPARK</ServiceName>
                    <DisplayName>RESTART SparkHistory</DisplayName>
                    <ComponentName>SparkHistory</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>RESTART</ActionName>
                    <ServiceName>SPARK</ServiceName>
                    <DisplayName>RESTART ThriftServer</DisplayName>
                    <ComponentName>SparkThriftServer</ComponentName>
                </ServiceAction>
            </ServiceActionList>
            <ServiceName>SPARK</ServiceName>
            <notStartedNum>0</notStartedNum>
            <onlyClient>false</onlyClient>
            <ServiceVersion>2.4.2</ServiceVersion>
            <ServiceDisplayName>Spark</ServiceDisplayName>
            <needRestartNum>0</needRestartNum>
        </ClusterInstalledService>
        <ClusterInstalledService>
            <abnormalNum>0</abnormalNum>
            <ServiceActionList>
                <ServiceAction>
                    <ActionName>CONFIGURE</ActionName>
                    <ServiceName>HUE</ServiceName>
                    <DisplayName>CONFIGURE All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>START</ActionName>
                    <ServiceName>HUE</ServiceName>
                    <DisplayName>START All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>STOP</ActionName>
                    <ServiceName>HUE</ServiceName>
                    <DisplayName>STOP All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>RESTART</ActionName>
                    <ServiceName>HUE</ServiceName>
                    <DisplayName>RESTART All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>RESTART</ActionName>
                    <ServiceName>HUE</ServiceName>
                    <DisplayName>RESTART Hue</DisplayName>
                    <ComponentName>Hue</ComponentName>
                </ServiceAction>
            </ServiceActionList>
            <ServiceName>HUE</ServiceName>
            <notStartedNum>0</notStartedNum>
            <onlyClient>false</onlyClient>
            <ServiceVersion>4.1.0</ServiceVersion>
            <ServiceDisplayName>Hue</ServiceDisplayName>
            <needRestartNum>0</needRestartNum>
        </ClusterInstalledService>
        <ClusterInstalledService>
            <abnormalNum>0</abnormalNum>
            <ServiceActionList>
                <ServiceAction>
                    <ActionName>CONFIGURE</ActionName>
                    <ServiceName>ZEPPELIN</ServiceName>
                    <DisplayName>CONFIGURE All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>START</ActionName>
                    <ServiceName>ZEPPELIN</ServiceName>
                    <DisplayName>START All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>STOP</ActionName>
                    <ServiceName>ZEPPELIN</ServiceName>
                    <DisplayName>STOP All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>RESTART</ActionName>
                    <ServiceName>ZEPPELIN</ServiceName>
                    <DisplayName>RESTART All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>RESTART</ActionName>
                    <ServiceName>ZEPPELIN</ServiceName>
                    <DisplayName>RESTART Zeppelin</DisplayName>
                    <ComponentName>Zeppelin</ComponentName>
                </ServiceAction>
            </ServiceActionList>
            <ServiceName>ZEPPELIN</ServiceName>
            <notStartedNum>0</notStartedNum>
            <onlyClient>false</onlyClient>
            <ServiceVersion>0.8.1</ServiceVersion>
            <ServiceDisplayName>Zeppelin</ServiceDisplayName>
            <needRestartNum>0</needRestartNum>
        </ClusterInstalledService>
        <ClusterInstalledService>
            <abnormalNum>0</abnormalNum>
            <ServiceActionList>
                <ServiceAction>
                    <ActionName>CONFIGURE</ActionName>
                    <ServiceName>TEZ</ServiceName>
                    <DisplayName>CONFIGURE All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>START</ActionName>
                    <ServiceName>TEZ</ServiceName>
                    <DisplayName>START All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>STOP</ActionName>
                    <ServiceName>TEZ</ServiceName>
                    <DisplayName>STOP All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>RESTART</ActionName>
                    <ServiceName>TEZ</ServiceName>
                    <DisplayName>RESTART All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>RESTART</ActionName>
                    <ServiceName>TEZ</ServiceName>
                    <DisplayName>RESTART Tez Client</DisplayName>
                    <ComponentName>TezInit</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>RESTART</ActionName>
                    <ServiceName>TEZ</ServiceName>
                    <DisplayName>RESTART Tomcat</DisplayName>
                    <ComponentName>Tomcat</ComponentName>
                </ServiceAction>
            </ServiceActionList>
            <ServiceName>TEZ</ServiceName>
            <notStartedNum>0</notStartedNum>
            <onlyClient>false</onlyClient>
            <ServiceVersion>0.9.1</ServiceVersion>
            <ServiceDisplayName>Tez</ServiceDisplayName>
            <needRestartNum>0</needRestartNum>
        </ClusterInstalledService>
        <ClusterInstalledService>
            <abnormalNum>0</abnormalNum>
            <ServiceActionList>
                <ServiceAction>
                    <ActionName>CONFIGURE</ActionName>
                    <ServiceName>SQOOP</ServiceName>
                    <DisplayName>CONFIGURE All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
            </ServiceActionList>
            <ServiceName>SQOOP</ServiceName>
            <notStartedNum>0</notStartedNum>
            <onlyClient>false</onlyClient>
            <ServiceVersion>1.4.7</ServiceVersion>
            <ServiceDisplayName>Sqoop</ServiceDisplayName>
            <needRestartNum>0</needRestartNum>
        </ClusterInstalledService>
        <ClusterInstalledService>
            <abnormalNum>0</abnormalNum>
            <ServiceActionList>
                <ServiceAction>
                    <ActionName>CONFIGURE</ActionName>
                    <ServiceName>PIG</ServiceName>
                    <DisplayName>CONFIGURE All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
            </ServiceActionList>
            <ServiceName>PIG</ServiceName>
            <notStartedNum>0</notStartedNum>
            <onlyClient>false</onlyClient>
            <ServiceVersion>0.14.0</ServiceVersion>
            <ServiceDisplayName>Pig</ServiceDisplayName>
            <needRestartNum>0</needRestartNum>
        </ClusterInstalledService>
        <ClusterInstalledService>
            <abnormalNum>0</abnormalNum>
            <ServiceActionList>
                <ServiceAction>
                    <ActionName>CONFIGURE</ActionName>
                    <ServiceName>KNOX</ServiceName>
                    <DisplayName>CONFIGURE All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>START</ActionName>
                    <ServiceName>KNOX</ServiceName>
                    <DisplayName>START All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>STOP</ActionName>
                    <ServiceName>KNOX</ServiceName>
                    <DisplayName>STOP All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>RESTART</ActionName>
                    <ServiceName>KNOX</ServiceName>
                    <DisplayName>RESTART All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>RESTART</ActionName>
                    <ServiceName>KNOX</ServiceName>
                    <DisplayName>RESTART Knox</DisplayName>
                    <ComponentName>KNOX</ComponentName>
                </ServiceAction>
            </ServiceActionList>
            <ServiceName>KNOX</ServiceName>
            <notStartedNum>0</notStartedNum>
            <onlyClient>false</onlyClient>
            <ServiceVersion>1.1.0</ServiceVersion>
            <ServiceDisplayName>Knox</ServiceDisplayName>
            <needRestartNum>0</needRestartNum>
        </ClusterInstalledService>
        <ClusterInstalledService>
            <abnormalNum>0</abnormalNum>
            <ServiceActionList>
                <ServiceAction>
                    <ActionName>CONFIGURE</ActionName>
                    <ServiceName>APACHEDS</ServiceName>
                    <DisplayName>CONFIGURE All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>START</ActionName>
                    <ServiceName>APACHEDS</ServiceName>
                    <DisplayName>START All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>STOP</ActionName>
                    <ServiceName>APACHEDS</ServiceName>
                    <DisplayName>STOP All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>RESTART</ActionName>
                    <ServiceName>APACHEDS</ServiceName>
                    <DisplayName>RESTART All Components</DisplayName>
                    <ComponentName>ALL COMPONENTS</ComponentName>
                </ServiceAction>
                <ServiceAction>
                    <ActionName>RESTART</ActionName>
                    <ServiceName>APACHEDS</ServiceName>
                    <DisplayName>RESTART ApacheDS</DisplayName>
                    <ComponentName>ApacheDS</ComponentName>
                </ServiceAction>
            </ServiceActionList>
            <ServiceName>APACHEDS</ServiceName>
            <notStartedNum>0</notStartedNum>
            <onlyClient>false</onlyClient>
            <ServiceVersion>2.0.0</ServiceVersion>
            <ServiceDisplayName>ApacheDS</ServiceDisplayName>
            <needRestartNum>0</needRestartNum>
        </ClusterInstalledService>
    </ClusterInstalledServiceList>
    <RequestId>04851356-DBCC-424F-8599-F683119C36BF</RequestId>
</ListClusterInstalledServiceResponse>
```

`JSON` 格式

``` {#json_return_success_demo}
{
	"requestId":"168B04C3-F037-43B8-837E-792FF56F73DC",
	"clusterInstalledServiceList":[
		{
			"abnormalNum":0,
			"serviceActionList":[
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"CONFIGURE",
					"displayName":"CONFIGURE All Components",
					"serviceName":"HDFS"
				},
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"START",
					"displayName":"START All Components",
					"serviceName":"HDFS"
				},
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"STOP",
					"displayName":"STOP All Components",
					"serviceName":"HDFS"
				},
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"RESTART",
					"displayName":"RESTART All Components",
					"serviceName":"HDFS"
				},
				{
					"componentName":"DataNode",
					"actionName":"RESTART",
					"displayName":"RESTART DataNode",
					"serviceName":"HDFS"
				},
				{
					"componentName":"HttpFS",
					"actionName":"RESTART",
					"displayName":"RESTART HttpFS",
					"serviceName":"HDFS"
				},
				{
					"componentName":"JournalNode",
					"actionName":"RESTART",
					"displayName":"RESTART JournalNode",
					"serviceName":"HDFS"
				},
				{
					"componentName":"KMS",
					"actionName":"RESTART",
					"displayName":"RESTART KMS",
					"serviceName":"HDFS"
				},
				{
					"componentName":"NameNode",
					"actionName":"RESTART",
					"displayName":"RESTART NameNode",
					"serviceName":"HDFS"
				},
				{
					"componentName":"ZKFC",
					"actionName":"RESTART",
					"displayName":"RESTART ZKFC",
					"serviceName":"HDFS"
				},
				{
					"componentName":"NameNode",
					"command":"rebalance",
					"actionName":"CUSTOM_COMMAND",
					"displayName":"rebalance",
					"serviceName":"HDFS"
				},
				{
					"componentName":"NameNode",
					"command":"stop_rebalance",
					"actionName":"CUSTOM_COMMAND",
					"displayName":"stop_rebalance",
					"serviceName":"HDFS"
				}
			],
			"notStartedNum":2,
			"serviceVersion":"2.8.5",
			"onlyClient":false,
			"serviceDisplayName":"HDFS",
			"needRestartNum":0,
			"serviceName":"HDFS"
		},
		{
			"abnormalNum":0,
			"serviceActionList":[
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"CONFIGURE",
					"displayName":"CONFIGURE All Components",
					"serviceName":"YARN"
				},
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"START",
					"displayName":"START All Components",
					"serviceName":"YARN"
				},
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"STOP",
					"displayName":"STOP All Components",
					"serviceName":"YARN"
				},
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"RESTART",
					"displayName":"RESTART All Components",
					"serviceName":"YARN"
				},
				{
					"componentName":"JobHistory",
					"actionName":"RESTART",
					"displayName":"RESTART JobHistory",
					"serviceName":"YARN"
				},
				{
					"componentName":"NodeManager",
					"actionName":"RESTART",
					"displayName":"RESTART NodeManager",
					"serviceName":"YARN"
				},
				{
					"componentName":"ResourceManager",
					"actionName":"RESTART",
					"displayName":"RESTART ResourceManager",
					"serviceName":"YARN"
				},
				{
					"componentName":"TimeLineServer",
					"actionName":"RESTART",
					"displayName":"RESTART App Timeline Server",
					"serviceName":"YARN"
				},
				{
					"componentName":"WebAppProxyServer",
					"actionName":"RESTART",
					"displayName":"RESTART WebAppProxyServer",
					"serviceName":"YARN"
				},
				{
					"componentName":"ResourceManager",
					"command":"refreshQueues",
					"actionName":"CUSTOM_COMMAND",
					"displayName":"refreshQueues",
					"serviceName":"YARN"
				},
				{
					"componentName":"NodeManager",
					"command":"enableCGroups",
					"actionName":"CUSTOM_COMMAND",
					"displayName":"enableCGroups",
					"serviceName":"YARN"
				},
				{
					"componentName":"NodeManager",
					"command":"disableCGroups",
					"actionName":"CUSTOM_COMMAND",
					"displayName":"disableCGroups",
					"serviceName":"YARN"
				}
			],
			"notStartedNum":0,
			"serviceVersion":"2.8.5",
			"onlyClient":false,
			"serviceDisplayName":"YARN",
			"needRestartNum":0,
			"serviceName":"YARN"
		},
		{
			"abnormalNum":0,
			"serviceActionList":[
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"CONFIGURE",
					"displayName":"CONFIGURE All Components",
					"serviceName":"HIVE"
				},
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"START",
					"displayName":"START All Components",
					"serviceName":"HIVE"
				},
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"STOP",
					"displayName":"STOP All Components",
					"serviceName":"HIVE"
				},
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"RESTART",
					"displayName":"RESTART All Components",
					"serviceName":"HIVE"
				},
				{
					"componentName":"HiveMetaStore",
					"actionName":"RESTART",
					"displayName":"RESTART Hive MetaStore",
					"serviceName":"HIVE"
				},
				{
					"componentName":"HiveServer",
					"actionName":"RESTART",
					"displayName":"RESTART HiveServer2",
					"serviceName":"HIVE"
				}
			],
			"notStartedNum":0,
			"serviceVersion":"3.1.1",
			"onlyClient":false,
			"serviceDisplayName":"Hive",
			"needRestartNum":0,
			"serviceName":"HIVE"
		},
		{
			"abnormalNum":0,
			"serviceActionList":[
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"CONFIGURE",
					"displayName":"CONFIGURE All Components",
					"serviceName":"GANGLIA"
				},
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"START",
					"displayName":"START All Components",
					"serviceName":"GANGLIA"
				},
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"STOP",
					"displayName":"STOP All Components",
					"serviceName":"GANGLIA"
				},
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"RESTART",
					"displayName":"RESTART All Components",
					"serviceName":"GANGLIA"
				},
				{
					"componentName":"GMETAD",
					"actionName":"RESTART",
					"displayName":"RESTART Gmetad",
					"serviceName":"GANGLIA"
				},
				{
					"componentName":"GMOND",
					"actionName":"RESTART",
					"displayName":"RESTART Gmond",
					"serviceName":"GANGLIA"
				},
				{
					"componentName":"HTTPD",
					"actionName":"RESTART",
					"displayName":"RESTART Httpd",
					"serviceName":"GANGLIA"
				}
			],
			"notStartedNum":0,
			"serviceVersion":"3.7.2",
			"onlyClient":false,
			"serviceDisplayName":"Ganglia",
			"needRestartNum":0,
			"serviceName":"GANGLIA"
		},
		{
			"abnormalNum":0,
			"serviceActionList":[
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"CONFIGURE",
					"displayName":"CONFIGURE All Components",
					"serviceName":"ZOOKEEPER"
				},
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"START",
					"displayName":"START All Components",
					"serviceName":"ZOOKEEPER"
				},
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"STOP",
					"displayName":"STOP All Components",
					"serviceName":"ZOOKEEPER"
				},
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"RESTART",
					"displayName":"RESTART All Components",
					"serviceName":"ZOOKEEPER"
				},
				{
					"componentName":"ZOOKEEPER",
					"actionName":"RESTART",
					"displayName":"RESTART ZooKeeper",
					"serviceName":"ZOOKEEPER"
				}
			],
			"notStartedNum":0,
			"serviceVersion":"3.4.13",
			"onlyClient":false,
			"serviceDisplayName":"ZooKeeper",
			"needRestartNum":0,
			"serviceName":"ZOOKEEPER"
		},
		{
			"abnormalNum":0,
			"serviceActionList":[
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"CONFIGURE",
					"displayName":"CONFIGURE All Components",
					"serviceName":"SPARK"
				},
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"START",
					"displayName":"START All Components",
					"serviceName":"SPARK"
				},
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"STOP",
					"displayName":"STOP All Components",
					"serviceName":"SPARK"
				},
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"RESTART",
					"displayName":"RESTART All Components",
					"serviceName":"SPARK"
				},
				{
					"componentName":"SparkHistory",
					"actionName":"RESTART",
					"displayName":"RESTART SparkHistory",
					"serviceName":"SPARK"
				},
				{
					"componentName":"SparkThriftServer",
					"actionName":"RESTART",
					"displayName":"RESTART ThriftServer",
					"serviceName":"SPARK"
				}
			],
			"notStartedNum":0,
			"serviceVersion":"2.4.2",
			"onlyClient":false,
			"serviceDisplayName":"Spark",
			"needRestartNum":0,
			"serviceName":"SPARK"
		},
		{
			"abnormalNum":0,
			"serviceActionList":[
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"CONFIGURE",
					"displayName":"CONFIGURE All Components",
					"serviceName":"HUE"
				},
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"START",
					"displayName":"START All Components",
					"serviceName":"HUE"
				},
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"STOP",
					"displayName":"STOP All Components",
					"serviceName":"HUE"
				},
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"RESTART",
					"displayName":"RESTART All Components",
					"serviceName":"HUE"
				},
				{
					"componentName":"Hue",
					"actionName":"RESTART",
					"displayName":"RESTART Hue",
					"serviceName":"HUE"
				}
			],
			"notStartedNum":0,
			"serviceVersion":"4.1.0",
			"onlyClient":false,
			"serviceDisplayName":"Hue",
			"needRestartNum":0,
			"serviceName":"HUE"
		},
		{
			"abnormalNum":0,
			"serviceActionList":[
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"CONFIGURE",
					"displayName":"CONFIGURE All Components",
					"serviceName":"ZEPPELIN"
				},
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"START",
					"displayName":"START All Components",
					"serviceName":"ZEPPELIN"
				},
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"STOP",
					"displayName":"STOP All Components",
					"serviceName":"ZEPPELIN"
				},
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"RESTART",
					"displayName":"RESTART All Components",
					"serviceName":"ZEPPELIN"
				},
				{
					"componentName":"Zeppelin",
					"actionName":"RESTART",
					"displayName":"RESTART Zeppelin",
					"serviceName":"ZEPPELIN"
				}
			],
			"notStartedNum":0,
			"serviceVersion":"0.8.1",
			"onlyClient":false,
			"serviceDisplayName":"Zeppelin",
			"needRestartNum":0,
			"serviceName":"ZEPPELIN"
		},
		{
			"abnormalNum":0,
			"serviceActionList":[
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"CONFIGURE",
					"displayName":"CONFIGURE All Components",
					"serviceName":"TEZ"
				},
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"START",
					"displayName":"START All Components",
					"serviceName":"TEZ"
				},
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"STOP",
					"displayName":"STOP All Components",
					"serviceName":"TEZ"
				},
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"RESTART",
					"displayName":"RESTART All Components",
					"serviceName":"TEZ"
				},
				{
					"componentName":"TezInit",
					"actionName":"RESTART",
					"displayName":"RESTART Tez Client",
					"serviceName":"TEZ"
				},
				{
					"componentName":"Tomcat",
					"actionName":"RESTART",
					"displayName":"RESTART Tomcat",
					"serviceName":"TEZ"
				}
			],
			"notStartedNum":0,
			"serviceVersion":"0.9.1",
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
					"actionName":"CONFIGURE",
					"displayName":"CONFIGURE All Components",
					"serviceName":"SQOOP"
				}
			],
			"notStartedNum":0,
			"serviceVersion":"1.4.7",
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
					"actionName":"CONFIGURE",
					"displayName":"CONFIGURE All Components",
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
			"abnormalNum":0,
			"serviceActionList":[
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"CONFIGURE",
					"displayName":"CONFIGURE All Components",
					"serviceName":"KNOX"
				},
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"START",
					"displayName":"START All Components",
					"serviceName":"KNOX"
				},
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"STOP",
					"displayName":"STOP All Components",
					"serviceName":"KNOX"
				},
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"RESTART",
					"displayName":"RESTART All Components",
					"serviceName":"KNOX"
				},
				{
					"componentName":"KNOX",
					"actionName":"RESTART",
					"displayName":"RESTART Knox",
					"serviceName":"KNOX"
				}
			],
			"notStartedNum":0,
			"serviceVersion":"1.1.0",
			"onlyClient":false,
			"serviceDisplayName":"Knox",
			"needRestartNum":0,
			"serviceName":"KNOX"
		},
		{
			"abnormalNum":0,
			"serviceActionList":[
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"CONFIGURE",
					"displayName":"CONFIGURE All Components",
					"serviceName":"APACHEDS"
				},
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"START",
					"displayName":"START All Components",
					"serviceName":"APACHEDS"
				},
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"STOP",
					"displayName":"STOP All Components",
					"serviceName":"APACHEDS"
				},
				{
					"componentName":"ALL COMPONENTS",
					"actionName":"RESTART",
					"displayName":"RESTART All Components",
					"serviceName":"APACHEDS"
				},
				{
					"componentName":"ApacheDS",
					"actionName":"RESTART",
					"displayName":"RESTART ApacheDS",
					"serviceName":"APACHEDS"
				}
			],
			"notStartedNum":0,
			"serviceVersion":"2.0.0",
			"onlyClient":false,
			"serviceDisplayName":"ApacheDS",
			"needRestartNum":0,
			"serviceName":"APACHEDS"
		}
	]
}
```

## 错误码 { .section}

访问[错误中心](https://error-center.alibabacloud.com/status/product/Emr)查看更多错误码。

