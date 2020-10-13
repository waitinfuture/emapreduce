# ListClusterInstalledService

You can call this operation to query ListClusterInstalledService that have been installed in a cluster.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=Emr&api=ListClusterInstalledService&type=RPC&version=2016-04-08)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Required|ListClusterInstalledService|The operation that you want to perform. For API requests using the HTTP or HTTPS URL, this parameter is required. Set this parameter to ListClusterInstalledService. |
|ClusterId|String|Required|C-0EF9B0EC8564\*\*\*\*|The ID of the ApsaraDB for PolarDB cluster. |
|RegionId|String|Required|cn-hangzhou|The ID of the region where your project resides. |
|PageNumber|Integer|Optional|1|The page number of the returned page. |
|PageSize|Integer|Optional|30|The number of entries returned on each page. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|ClusterInstalledServiceList|Array of ClusterInstalledService| |The list of services installed in the cluster. |
|ClusterInstalledService| | | |
|ServiceActionList|Array of ServiceAction| |The list of operations supported by the service. |
|ServiceAction| | | |
|ActionName|String|CUSTOM\_COMMAND|Operation name:

-   CONFIGURE
-   START
-   STOP
-   RESTART
-   CUSTOM\_COMMAND |
|Command|String|rebalance|The command of the custom Action. This parameter is required when the Action name is CUSTOM\_COMMAND. |
|ComponentName|String|DataNode|The name of the add-on. |
|DisplayName|String|Start All Components|The displayed name of the operation. |
|ServiceName|String|HDFS|The name of the service. |
|ServiceDisplayName|String|YARN|The displayed name of the service. |
|ServiceEcmVersion|String|3.7.2.2.7|The reserved field. |
|ServiceName|String|YARN|The name of the service. |
|ServiceVersion|String|3.7.2|The version of the service. |
|abnormalNum|Integer|0|The number of abnormal components. |
|comment|String|test|The description of the service. |
|needRestartNum|Integer|0|The number of components to be restarted. |
|notStartedNum|Integer|0|The number of components that have been stopped. |
|onlyClient|Boolean|false|Only **Client** type component:

-   true
-   false |
|serviceStatus|String|INSTALLING|The service status of the topic. |
|RequestId|String|88A94B4A-898D-448B-BC0B-6F0FE2CC64CA|The unique ID of the request. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=ListClusterInstalledService
&ClusterId=C-0EF9B0EC8564****
&RegionId=cn-hangzhou
&<Common request parameters>
```

Sample success responses

`XML` format

```
<RequestId>F14E2754-DC4E-4D9C-BB59-3136FF6EF033</RequestId>
<ClusterInstalledServiceList>
    <ClusterInstalledService>
        <needRestartNum>0</needRestartNum>
        <abnormalNum>0</abnormalNum>
        <ServiceActionList>
            <ServiceAction>
                <ActionName>CONFIGURE</ActionName>
                <ServiceName>HDFS</ServiceName>
                <DisplayName>Configure All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>START</ActionName>
                <ServiceName>HDFS</ServiceName>
                <DisplayName>Start All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>STOP</ActionName>
                <ServiceName>HDFS</ServiceName>
                <DisplayName>Stop All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>HDFS</ServiceName>
                <DisplayName>Restart All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>HDFS</ServiceName>
                <DisplayName>Restart DataNode</DisplayName>
                <ComponentName>DataNode</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>DECOMMISSION</ActionName>
                <ServiceName>HDFS</ServiceName>
                <DisplayName>Decommission DataNode</DisplayName>
                <ComponentName>DataNode</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>HDFS</ServiceName>
                <DisplayName>Restart HttpFS</DisplayName>
                <ComponentName>HttpFS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>HDFS</ServiceName>
                <DisplayName>Restart KMS</DisplayName>
                <ComponentName>KMS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>HDFS</ServiceName>
                <DisplayName>Restart NameNode</DisplayName>
                <ComponentName>NameNode</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>HDFS</ServiceName>
                <DisplayName>Restart SecondaryNameNode</DisplayName>
                <ComponentName>SecondaryNameNode</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>CUSTOM_COMMAND</ActionName>
                <ServiceName>HDFS</ServiceName>
                <Command>rebalance</Command>
                <DisplayName>Rebalance</DisplayName>
                <ComponentName>NameNode</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>CUSTOM_COMMAND</ActionName>
                <ServiceName>HDFS</ServiceName>
                <Command>stop_rebalance</Command>
                <DisplayName>Stop Rebalance</DisplayName>
                <ComponentName>NameNode</ComponentName>
            </ServiceAction>
        </ServiceActionList>
        <ServiceName>HDFS</ServiceName>
        <notStartedNum>0</notStartedNum>
        <ServiceVersion>2.8.5</ServiceVersion>
        <ServiceDisplayName>HDFS</ServiceDisplayName>
        <onlyClient>false</onlyClient>
    </ClusterInstalledService>
    <ClusterInstalledService>
        <needRestartNum>0</needRestartNum>
        <abnormalNum>0</abnormalNum>
        <ServiceActionList>
            <ServiceAction>
                <ActionName>CONFIGURE</ActionName>
                <ServiceName>YARN</ServiceName>
                <DisplayName>Configure All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>START</ActionName>
                <ServiceName>YARN</ServiceName>
                <DisplayName>Start All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>STOP</ActionName>
                <ServiceName>YARN</ServiceName>
                <DisplayName>Stop All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>YARN</ServiceName>
                <DisplayName>Restart All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>YARN</ServiceName>
                <DisplayName>Restart JobHistory</DisplayName>
                <ComponentName>JobHistory</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>YARN</ServiceName>
                <DisplayName>Restart NodeManager</DisplayName>
                <ComponentName>NodeManager</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>DECOMMISSION</ActionName>
                <ServiceName>YARN</ServiceName>
                <DisplayName>Decommission NodeManager</DisplayName>
                <ComponentName>NodeManager</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>YARN</ServiceName>
                <DisplayName>Restart ResourceManager</DisplayName>
                <ComponentName>ResourceManager</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>YARN</ServiceName>
                <DisplayName>Restart App Timeline Server</DisplayName>
                <ComponentName>TimeLineServer</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>YARN</ServiceName>
                <DisplayName>Restart WebAppProxyServer</DisplayName>
                <ComponentName>WebAppProxyServer</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>CUSTOM_COMMAND</ActionName>
                <ServiceName>YARN</ServiceName>
                <Command>refreshQueues</Command>
                <DisplayName>Refresh Queues</DisplayName>
                <ComponentName>ResourceManager</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>CUSTOM_COMMAND</ActionName>
                <ServiceName>YARN</ServiceName>
                <Command>enableCGroups</Command>
                <DisplayName>Enable CGroups</DisplayName>
                <ComponentName>NodeManager</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>CUSTOM_COMMAND</ActionName>
                <ServiceName>YARN</ServiceName>
                <Command>disableCGroups</Command>
                <DisplayName>Disable CGroups</DisplayName>
                <ComponentName>NodeManager</ComponentName>
            </ServiceAction>
        </ServiceActionList>
        <ServiceName>YARN</ServiceName>
        <notStartedNum>0</notStartedNum>
        <ServiceVersion>2.8.5</ServiceVersion>
        <ServiceDisplayName>YARN</ServiceDisplayName>
        <onlyClient>false</onlyClient>
    </ClusterInstalledService>
    <ClusterInstalledService>
        <needRestartNum>0</needRestartNum>
        <abnormalNum>0</abnormalNum>
        <ServiceActionList>
            <ServiceAction>
                <ActionName>CONFIGURE</ActionName>
                <ServiceName>HIVE</ServiceName>
                <DisplayName>Configure All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>START</ActionName>
                <ServiceName>HIVE</ServiceName>
                <DisplayName>Start All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>STOP</ActionName>
                <ServiceName>HIVE</ServiceName>
                <DisplayName>Stop All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>HIVE</ServiceName>
                <DisplayName>Restart All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>HIVE</ServiceName>
                <DisplayName>Restart Hive MetaStore</DisplayName>
                <ComponentName>HiveMetaStore</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>HIVE</ServiceName>
                <DisplayName>Restart HiveServer2</DisplayName>
                <ComponentName>HiveServer</ComponentName>
            </ServiceAction>
        </ServiceActionList>
        <ServiceName>HIVE</ServiceName>
        <notStartedNum>0</notStartedNum>
        <ServiceVersion>2.3.5</ServiceVersion>
        <ServiceDisplayName>Hive</ServiceDisplayName>
        <onlyClient>false</onlyClient>
    </ClusterInstalledService>
    <ClusterInstalledService>
        <needRestartNum>0</needRestartNum>
        <abnormalNum>0</abnormalNum>
        <ServiceActionList>
            <ServiceAction>
                <ActionName>CONFIGURE</ActionName>
                <ServiceName>GANGLIA</ServiceName>
                <DisplayName>Configure All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>START</ActionName>
                <ServiceName>GANGLIA</ServiceName>
                <DisplayName>Start All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>STOP</ActionName>
                <ServiceName>GANGLIA</ServiceName>
                <DisplayName>Stop All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>GANGLIA</ServiceName>
                <DisplayName>Restart All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>GANGLIA</ServiceName>
                <DisplayName>Restart Gmetad</DisplayName>
                <ComponentName>GMETAD</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>GANGLIA</ServiceName>
                <DisplayName>Restart Gmond</DisplayName>
                <ComponentName>GMOND</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>GANGLIA</ServiceName>
                <DisplayName>Restart Httpd</DisplayName>
                <ComponentName>HTTPD</ComponentName>
            </ServiceAction>
        </ServiceActionList>
        <ServiceName>GANGLIA</ServiceName>
        <notStartedNum>0</notStartedNum>
        <ServiceVersion>3.7.2</ServiceVersion>
        <ServiceDisplayName>Ganglia</ServiceDisplayName>
        <onlyClient>false</onlyClient>
    </ClusterInstalledService>
    <ClusterInstalledService>
        <needRestartNum>0</needRestartNum>
        <abnormalNum>3</abnormalNum>
        <ServiceActionList>
            <ServiceAction>
                <ActionName>CONFIGURE</ActionName>
                <ServiceName>ZOOKEEPER</ServiceName>
                <DisplayName>Configure All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>START</ActionName>
                <ServiceName>ZOOKEEPER</ServiceName>
                <DisplayName>Start All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>STOP</ActionName>
                <ServiceName>ZOOKEEPER</ServiceName>
                <DisplayName>Stop All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>ZOOKEEPER</ServiceName>
                <DisplayName>Restart All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>ZOOKEEPER</ServiceName>
                <DisplayName>Restart ZooKeeper</DisplayName>
                <ComponentName>ZOOKEEPER</ComponentName>
            </ServiceAction>
        </ServiceActionList>
        <ServiceName>ZOOKEEPER</ServiceName>
        <notStartedNum>3</notStartedNum>
        <ServiceVersion>3.5.6</ServiceVersion>
        <ServiceDisplayName>ZooKeeper</ServiceDisplayName>
        <onlyClient>false</onlyClient>
    </ClusterInstalledService>
    <ClusterInstalledService>
        <needRestartNum>0</needRestartNum>
        <abnormalNum>1</abnormalNum>
        <ServiceActionList>
            <ServiceAction>
                <ActionName>CONFIGURE</ActionName>
                <ServiceName>SPARK</ServiceName>
                <DisplayName>Configure All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>START</ActionName>
                <ServiceName>SPARK</ServiceName>
                <DisplayName>Start All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>STOP</ActionName>
                <ServiceName>SPARK</ServiceName>
                <DisplayName>Stop All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>SPARK</ServiceName>
                <DisplayName>Restart All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>SPARK</ServiceName>
                <DisplayName>Restart SparkHistory</DisplayName>
                <ComponentName>SparkHistory</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>SPARK</ServiceName>
                <DisplayName>Restart ThriftServer</DisplayName>
                <ComponentName>SparkThriftServer</ComponentName>
            </ServiceAction>
        </ServiceActionList>
        <ServiceName>SPARK</ServiceName>
        <notStartedNum>1</notStartedNum>
        <ServiceVersion>2.4.5</ServiceVersion>
        <ServiceDisplayName>Spark</ServiceDisplayName>
        <onlyClient>false</onlyClient>
    </ClusterInstalledService>
    <ClusterInstalledService>
        <needRestartNum>0</needRestartNum>
        <abnormalNum>4</abnormalNum>
        <ServiceActionList>
            <ServiceAction>
                <ActionName>CONFIGURE</ActionName>
                <ServiceName>HBASE</ServiceName>
                <DisplayName>Configure All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>START</ActionName>
                <ServiceName>HBASE</ServiceName>
                <DisplayName>Start All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>STOP</ActionName>
                <ServiceName>HBASE</ServiceName>
                <DisplayName>Stop All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>HBASE</ServiceName>
                <DisplayName>Restart All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>HBASE</ServiceName>
                <DisplayName>Restart HMaster</DisplayName>
                <ComponentName>HMaster</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>HBASE</ServiceName>
                <DisplayName>Restart HRegionServer</DisplayName>
                <ComponentName>HRegionServer</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>DECOMMISSION</ActionName>
                <ServiceName>HBASE</ServiceName>
                <DisplayName>Decommission HRegionServer</DisplayName>
                <ComponentName>HRegionServer</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>HBASE</ServiceName>
                <DisplayName>Restart ThriftServer</DisplayName>
                <ComponentName>ThriftServer</ComponentName>
            </ServiceAction>
        </ServiceActionList>
        <ServiceName>HBASE</ServiceName>
        <notStartedNum>4</notStartedNum>
        <ServiceVersion>1.4.9</ServiceVersion>
        <ServiceDisplayName>HBase</ServiceDisplayName>
        <onlyClient>false</onlyClient>
    </ClusterInstalledService>
    <ClusterInstalledService>
        <needRestartNum>0</needRestartNum>
        <abnormalNum>0</abnormalNum>
        <ServiceActionList>
            <ServiceAction>
                <ActionName>CONFIGURE</ActionName>
                <ServiceName>HUE</ServiceName>
                <DisplayName>Configure All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>START</ActionName>
                <ServiceName>HUE</ServiceName>
                <DisplayName>Start All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>STOP</ActionName>
                <ServiceName>HUE</ServiceName>
                <DisplayName>Stop All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>HUE</ServiceName>
                <DisplayName>Restart All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>HUE</ServiceName>
                <DisplayName>Restart Hue</DisplayName>
                <ComponentName>Hue</ComponentName>
            </ServiceAction>
        </ServiceActionList>
        <ServiceName>HUE</ServiceName>
        <notStartedNum>0</notStartedNum>
        <ServiceVersion>4.4.0</ServiceVersion>
        <ServiceDisplayName>Hue</ServiceDisplayName>
        <onlyClient>false</onlyClient>
    </ClusterInstalledService>
    <ClusterInstalledService>
        <needRestartNum>0</needRestartNum>
        <abnormalNum>1</abnormalNum>
        <ServiceActionList>
            <ServiceAction>
                <ActionName>CONFIGURE</ActionName>
                <ServiceName>ZEPPELIN</ServiceName>
                <DisplayName>Configure All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>START</ActionName>
                <ServiceName>ZEPPELIN</ServiceName>
                <DisplayName>Start All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>STOP</ActionName>
                <ServiceName>ZEPPELIN</ServiceName>
                <DisplayName>Stop All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>ZEPPELIN</ServiceName>
                <DisplayName>Restart All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>ZEPPELIN</ServiceName>
                <DisplayName>Restart Zeppelin</DisplayName>
                <ComponentName>Zeppelin</ComponentName>
            </ServiceAction>
        </ServiceActionList>
        <ServiceName>ZEPPELIN</ServiceName>
        <notStartedNum>1</notStartedNum>
        <ServiceVersion>0.8.1</ServiceVersion>
        <ServiceDisplayName>Zeppelin</ServiceDisplayName>
        <onlyClient>false</onlyClient>
    </ClusterInstalledService>
    <ClusterInstalledService>
        <needRestartNum>0</needRestartNum>
        <abnormalNum>1</abnormalNum>
        <ServiceActionList>
            <ServiceAction>
                <ActionName>CONFIGURE</ActionName>
                <ServiceName>OOZIE</ServiceName>
                <DisplayName>Configure All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>START</ActionName>
                <ServiceName>OOZIE</ServiceName>
                <DisplayName>Start All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>STOP</ActionName>
                <ServiceName>OOZIE</ServiceName>
                <DisplayName>Stop All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>OOZIE</ServiceName>
                <DisplayName>Restart All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>OOZIE</ServiceName>
                <DisplayName>Restart Oozie</DisplayName>
                <ComponentName>OOZIE</ComponentName>
            </ServiceAction>
        </ServiceActionList>
        <ServiceName>OOZIE</ServiceName>
        <notStartedNum>1</notStartedNum>
        <ServiceVersion>5.1.0</ServiceVersion>
        <ServiceDisplayName>Oozie</ServiceDisplayName>
        <onlyClient>false</onlyClient>
    </ClusterInstalledService>
    <ClusterInstalledService>
        <needRestartNum>0</needRestartNum>
        <abnormalNum>0</abnormalNum>
        <ServiceActionList>
            <ServiceAction>
                <ActionName>CONFIGURE</ActionName>
                <ServiceName>TEZ</ServiceName>
                <DisplayName>Configure All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>START</ActionName>
                <ServiceName>TEZ</ServiceName>
                <DisplayName>Start All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>STOP</ActionName>
                <ServiceName>TEZ</ServiceName>
                <DisplayName>Stop All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>TEZ</ServiceName>
                <DisplayName>Restart All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>TEZ</ServiceName>
                <DisplayName>Restart Tez Client</DisplayName>
                <ComponentName>TezInit</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>TEZ</ServiceName>
                <DisplayName>Restart Tomcat</DisplayName>
                <ComponentName>Tomcat</ComponentName>
            </ServiceAction>
        </ServiceActionList>
        <ServiceName>TEZ</ServiceName>
        <notStartedNum>0</notStartedNum>
        <ServiceVersion>0.9.2</ServiceVersion>
        <ServiceDisplayName>Tez</ServiceDisplayName>
        <onlyClient>false</onlyClient>
    </ClusterInstalledService>
    <ClusterInstalledService>
        <needRestartNum>0</needRestartNum>
        <abnormalNum>0</abnormalNum>
        <ServiceActionList>
            <ServiceAction>
                <ActionName>CONFIGURE</ActionName>
                <ServiceName>PHOENIX</ServiceName>
                <DisplayName>Configure All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
        </ServiceActionList>
        <ServiceName>PHOENIX</ServiceName>
        <notStartedNum>0</notStartedNum>
        <ServiceVersion>4.14.1</ServiceVersion>
        <ServiceDisplayName>Phoenix</ServiceDisplayName>
        <onlyClient>false</onlyClient>
    </ClusterInstalledService>
    <ClusterInstalledService>
        <needRestartNum>0</needRestartNum>
        <abnormalNum>1</abnormalNum>
        <ServiceActionList>
            <ServiceAction>
                <ActionName>CONFIGURE</ActionName>
                <ServiceName>PRESTO</ServiceName>
                <DisplayName>Configure All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>START</ActionName>
                <ServiceName>PRESTO</ServiceName>
                <DisplayName>Start All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>STOP</ActionName>
                <ServiceName>PRESTO</ServiceName>
                <DisplayName>Stop All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>PRESTO</ServiceName>
                <DisplayName>Restart All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>PRESTO</ServiceName>
                <DisplayName>Restart PrestoMaster</DisplayName>
                <ComponentName>PrestoMaster</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>PRESTO</ServiceName>
                <DisplayName>Restart PrestoWorker</DisplayName>
                <ComponentName>PrestoWorker</ComponentName>
            </ServiceAction>
        </ServiceActionList>
        <ServiceName>PRESTO</ServiceName>
        <notStartedNum>1</notStartedNum>
        <ServiceVersion>331</ServiceVersion>
        <ServiceDisplayName>Presto</ServiceDisplayName>
        <onlyClient>false</onlyClient>
    </ClusterInstalledService>
    <ClusterInstalledService>
        <needRestartNum>0</needRestartNum>
        <abnormalNum>0</abnormalNum>
        <ServiceActionList>
            <ServiceAction>
                <ActionName>CONFIGURE</ActionName>
                <ServiceName>SQOOP</ServiceName>
                <DisplayName>Configure All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
        </ServiceActionList>
        <ServiceName>SQOOP</ServiceName>
        <notStartedNum>0</notStartedNum>
        <ServiceVersion>1.4.7</ServiceVersion>
        <ServiceDisplayName>Sqoop</ServiceDisplayName>
        <onlyClient>false</onlyClient>
    </ClusterInstalledService>
    <ClusterInstalledService>
        <needRestartNum>0</needRestartNum>
        <abnormalNum>0</abnormalNum>
        <ServiceActionList>
            <ServiceAction>
                <ActionName>CONFIGURE</ActionName>
                <ServiceName>PIG</ServiceName>
                <DisplayName>Configure All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
        </ServiceActionList>
        <ServiceName>PIG</ServiceName>
        <notStartedNum>0</notStartedNum>
        <ServiceVersion>0.14.0</ServiceVersion>
        <ServiceDisplayName>Pig</ServiceDisplayName>
        <onlyClient>false</onlyClient>
    </ClusterInstalledService>
    <ClusterInstalledService>
        <needRestartNum>0</needRestartNum>
        <abnormalNum>7</abnormalNum>
        <ServiceActionList>
            <ServiceAction>
                <ActionName>CONFIGURE</ActionName>
                <ServiceName>STORM</ServiceName>
                <DisplayName>Configure All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>START</ActionName>
                <ServiceName>STORM</ServiceName>
                <DisplayName>Start All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>STOP</ActionName>
                <ServiceName>STORM</ServiceName>
                <DisplayName>Stop All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>STORM</ServiceName>
                <DisplayName>Restart All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>STORM</ServiceName>
                <DisplayName>Restart Logviewer</DisplayName>
                <ComponentName>Logviewer</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>STORM</ServiceName>
                <DisplayName>Restart Nimbus</DisplayName>
                <ComponentName>Nimbus</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>STORM</ServiceName>
                <DisplayName>Restart Supervisor</DisplayName>
                <ComponentName>Supervisor</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>STORM</ServiceName>
                <DisplayName>Restart UI</DisplayName>
                <ComponentName>UI</ComponentName>
            </ServiceAction>
        </ServiceActionList>
        <ServiceName>STORM</ServiceName>
        <notStartedNum>7</notStartedNum>
        <ServiceVersion>1.2.2</ServiceVersion>
        <ServiceDisplayName>Storm</ServiceDisplayName>
        <onlyClient>false</onlyClient>
    </ClusterInstalledService>
    <ClusterInstalledService>
        <needRestartNum>0</needRestartNum>
        <abnormalNum>5</abnormalNum>
        <ServiceActionList>
            <ServiceAction>
                <ActionName>CONFIGURE</ActionName>
                <ServiceName>KUDU</ServiceName>
                <DisplayName>Configure All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>START</ActionName>
                <ServiceName>KUDU</ServiceName>
                <DisplayName>Start All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>STOP</ActionName>
                <ServiceName>KUDU</ServiceName>
                <DisplayName>Stop All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>KUDU</ServiceName>
                <DisplayName>Restart All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>KUDU</ServiceName>
                <DisplayName>Restart Kudu Master</DisplayName>
                <ComponentName>KuduMaster</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>KUDU</ServiceName>
                <DisplayName>Restart Kudu Tserver</DisplayName>
                <ComponentName>KuduTserver</ComponentName>
            </ServiceAction>
        </ServiceActionList>
        <ServiceName>KUDU</ServiceName>
        <notStartedNum>5</notStartedNum>
        <ServiceVersion>1.10.0</ServiceVersion>
        <ServiceDisplayName>Kudu</ServiceDisplayName>
        <onlyClient>false</onlyClient>
    </ClusterInstalledService>
    <ClusterInstalledService>
        <needRestartNum>0</needRestartNum>
        <abnormalNum>0</abnormalNum>
        <ServiceActionList>
            <ServiceAction>
                <ActionName>CONFIGURE</ActionName>
                <ServiceName>FLUME</ServiceName>
                <DisplayName>Configure All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>START</ActionName>
                <ServiceName>FLUME</ServiceName>
                <DisplayName>Start All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>STOP</ActionName>
                <ServiceName>FLUME</ServiceName>
                <DisplayName>Stop All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>FLUME</ServiceName>
                <DisplayName>Restart All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>FLUME</ServiceName>
                <DisplayName>Restart Flume Agent</DisplayName>
                <ComponentName>FlumeAgent</ComponentName>
            </ServiceAction>
        </ServiceActionList>
        <ServiceName>FLUME</ServiceName>
        <notStartedNum>3</notStartedNum>
        <ServiceVersion>1.9.0</ServiceVersion>
        <ServiceDisplayName>FLUME</ServiceDisplayName>
        <onlyClient>false</onlyClient>
    </ClusterInstalledService>
    <ClusterInstalledService>
        <needRestartNum>0</needRestartNum>
        <abnormalNum>1</abnormalNum>
        <ServiceActionList>
            <ServiceAction>
                <ActionName>CONFIGURE</ActionName>
                <ServiceName>LIVY</ServiceName>
                <DisplayName>Configure All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>START</ActionName>
                <ServiceName>LIVY</ServiceName>
                <DisplayName>Start All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>STOP</ActionName>
                <ServiceName>LIVY</ServiceName>
                <DisplayName>Stop All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>LIVY</ServiceName>
                <DisplayName>Restart All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>LIVY</ServiceName>
                <DisplayName>Restart Livy</DisplayName>
                <ComponentName>Livy</ComponentName>
            </ServiceAction>
        </ServiceActionList>
        <ServiceName>LIVY</ServiceName>
        <notStartedNum>1</notStartedNum>
        <ServiceVersion>0.6.0</ServiceVersion>
        <ServiceDisplayName>LIVY</ServiceDisplayName>
        <onlyClient>false</onlyClient>
    </ClusterInstalledService>
    <ClusterInstalledService>
        <needRestartNum>0</needRestartNum>
        <abnormalNum>1</abnormalNum>
        <ServiceActionList>
            <ServiceAction>
                <ActionName>CONFIGURE</ActionName>
                <ServiceName>SUPERSET</ServiceName>
                <DisplayName>Configure All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>START</ActionName>
                <ServiceName>SUPERSET</ServiceName>
                <DisplayName>Start All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>STOP</ActionName>
                <ServiceName>SUPERSET</ServiceName>
                <DisplayName>Stop All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>SUPERSET</ServiceName>
                <DisplayName>Restart All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>SUPERSET</ServiceName>
                <DisplayName>Restart Superset</DisplayName>
                <ComponentName>Superset</ComponentName>
            </ServiceAction>
        </ServiceActionList>
        <ServiceName>SUPERSET</ServiceName>
        <notStartedNum>1</notStartedNum>
        <ServiceVersion>0.35.2</ServiceVersion>
        <ServiceDisplayName>Superset</ServiceDisplayName>
        <onlyClient>false</onlyClient>
    </ClusterInstalledService>
    <ClusterInstalledService>
        <needRestartNum>0</needRestartNum>
        <abnormalNum>1</abnormalNum>
        <ServiceActionList>
            <ServiceAction>
                <ActionName>CONFIGURE</ActionName>
                <ServiceName>FLINK</ServiceName>
                <DisplayName>Configure All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>START</ActionName>
                <ServiceName>FLINK</ServiceName>
                <DisplayName>Start All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>STOP</ActionName>
                <ServiceName>FLINK</ServiceName>
                <DisplayName>Stop All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>FLINK</ServiceName>
                <DisplayName>Restart All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>FLINK</ServiceName>
                <DisplayName>Restart FlinkHistoryServer</DisplayName>
                <ComponentName>FlinkHistoryServer</ComponentName>
            </ServiceAction>
        </ServiceActionList>
        <ServiceName>FLINK</ServiceName>
        <notStartedNum>1</notStartedNum>
        <ServiceVersion>1.10-vvr-1.0.4</ServiceVersion>
        <ServiceDisplayName>Flink</ServiceDisplayName>
        <onlyClient>false</onlyClient>
    </ClusterInstalledService>
    <ClusterInstalledService>
        <needRestartNum>0</needRestartNum>
        <abnormalNum>4</abnormalNum>
        <ServiceActionList>
            <ServiceAction>
                <ActionName>CONFIGURE</ActionName>
                <ServiceName>IMPALA</ServiceName>
                <DisplayName>Configure All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>START</ActionName>
                <ServiceName>IMPALA</ServiceName>
                <DisplayName>Start All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>STOP</ActionName>
                <ServiceName>IMPALA</ServiceName>
                <DisplayName>Stop All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>IMPALA</ServiceName>
                <DisplayName>Restart All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>IMPALA</ServiceName>
                <DisplayName>Restart Impala Catalog Server</DisplayName>
                <ComponentName>ImpalaCatalogd</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>IMPALA</ServiceName>
                <DisplayName>Restart Impala Daemon Server</DisplayName>
                <ComponentName>Impalad</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>IMPALA</ServiceName>
                <DisplayName>Restart Impala StateStore Server</DisplayName>
                <ComponentName>ImpalaStatestored</ComponentName>
            </ServiceAction>
        </ServiceActionList>
        <ServiceName>IMPALA</ServiceName>
        <notStartedNum>4</notStartedNum>
        <ServiceVersion>2.12.2</ServiceVersion>
        <ServiceDisplayName>Impala</ServiceDisplayName>
        <onlyClient>false</onlyClient>
    </ClusterInstalledService>
    <ClusterInstalledService>
        <needRestartNum>0</needRestartNum>
        <abnormalNum>2</abnormalNum>
        <ServiceActionList>
            <ServiceAction>
                <ActionName>CONFIGURE</ActionName>
                <ServiceName>RANGER</ServiceName>
                <DisplayName>Configure All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>START</ActionName>
                <ServiceName>RANGER</ServiceName>
                <DisplayName>Start All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>STOP</ActionName>
                <ServiceName>RANGER</ServiceName>
                <DisplayName>Stop All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>RANGER</ServiceName>
                <DisplayName>Restart All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>RANGER</ServiceName>
                <DisplayName>Restart RangerAdmin</DisplayName>
                <ComponentName>RangerAdmin</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>RANGER</ServiceName>
                <DisplayName>Restart RangerUserSync</DisplayName>
                <ComponentName>RangerUserSync</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>RANGER</ServiceName>
                <DisplayName>Restart Solr</DisplayName>
                <ComponentName>Solr</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>CUSTOM_COMMAND</ActionName>
                <ServiceName>RANGER</ServiceName>
                <Command>enableHDFS</Command>
                <DisplayName>enableHDFS</DisplayName>
                <ComponentName>RangerAdmin</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>CUSTOM_COMMAND</ActionName>
                <ServiceName>RANGER</ServiceName>
                <Command>disableHDFS</Command>
                <DisplayName>disableHDFS</DisplayName>
                <ComponentName>RangerAdmin</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>CUSTOM_COMMAND</ActionName>
                <ServiceName>RANGER</ServiceName>
                <Command>enableHive</Command>
                <DisplayName>enableHive</DisplayName>
                <ComponentName>RangerAdmin</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>CUSTOM_COMMAND</ActionName>
                <ServiceName>RANGER</ServiceName>
                <Command>disableHive</Command>
                <DisplayName>disableHive</DisplayName>
                <ComponentName>RangerAdmin</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>CUSTOM_COMMAND</ActionName>
                <ServiceName>RANGER</ServiceName>
                <Command>enableSpark</Command>
                <DisplayName>enableSpark</DisplayName>
                <ComponentName>RangerAdmin</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>CUSTOM_COMMAND</ActionName>
                <ServiceName>RANGER</ServiceName>
                <Command>disableSpark</Command>
                <DisplayName>disableSpark</DisplayName>
                <ComponentName>RangerAdmin</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>CUSTOM_COMMAND</ActionName>
                <ServiceName>RANGER</ServiceName>
                <Command>enablePresto</Command>
                <DisplayName>enablePresto</DisplayName>
                <ComponentName>RangerAdmin</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>CUSTOM_COMMAND</ActionName>
                <ServiceName>RANGER</ServiceName>
                <Command>disablePresto</Command>
                <DisplayName>disablePresto</DisplayName>
                <ComponentName>RangerAdmin</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>CUSTOM_COMMAND</ActionName>
                <ServiceName>RANGER</ServiceName>
                <Command>enableHBase</Command>
                <DisplayName>enableHBase</DisplayName>
                <ComponentName>RangerPlugin</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>CUSTOM_COMMAND</ActionName>
                <ServiceName>RANGER</ServiceName>
                <Command>disableHBase</Command>
                <DisplayName>disableHBase</DisplayName>
                <ComponentName>RangerPlugin</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>CUSTOM_COMMAND</ActionName>
                <ServiceName>RANGER</ServiceName>
                <Command>enableKafka</Command>
                <DisplayName>enableKafka</DisplayName>
                <ComponentName>RangerPlugin</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>CUSTOM_COMMAND</ActionName>
                <ServiceName>RANGER</ServiceName>
                <Command>disableKafka</Command>
                <DisplayName>disableKafka</DisplayName>
                <ComponentName>RangerPlugin</ComponentName>
            </ServiceAction>
        </ServiceActionList>
        <ServiceName>RANGER</ServiceName>
        <notStartedNum>2</notStartedNum>
        <ServiceVersion>1.2.0</ServiceVersion>
        <ServiceDisplayName>RANGER</ServiceDisplayName>
        <onlyClient>false</onlyClient>
    </ClusterInstalledService>
    <ClusterInstalledService>
        <needRestartNum>0</needRestartNum>
        <abnormalNum>0</abnormalNum>
        <ServiceActionList>
            <ServiceAction>
                <ActionName>CONFIGURE</ActionName>
                <ServiceName>SMARTDATA</ServiceName>
                <DisplayName>Configure All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>START</ActionName>
                <ServiceName>SMARTDATA</ServiceName>
                <DisplayName>Start All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>STOP</ActionName>
                <ServiceName>SMARTDATA</ServiceName>
                <DisplayName>Stop All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>SMARTDATA</ServiceName>
                <DisplayName>Restart All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>SMARTDATA</ServiceName>
                <DisplayName>Restart Jindo Namespace Service</DisplayName>
                <ComponentName>JindoNamespaceService</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>SMARTDATA</ServiceName>
                <DisplayName>Restart Jindo Storage Service</DisplayName>
                <ComponentName>JindoStorageService</ComponentName>
            </ServiceAction>
        </ServiceActionList>
        <ServiceName>SMARTDATA</ServiceName>
        <notStartedNum>0</notStartedNum>
        <ServiceVersion>2.7.202</ServiceVersion>
        <ServiceDisplayName>SmartData</ServiceDisplayName>
        <onlyClient>false</onlyClient>
    </ClusterInstalledService>
    <ClusterInstalledService>
        <needRestartNum>0</needRestartNum>
        <abnormalNum>1</abnormalNum>
        <ServiceActionList>
            <ServiceAction>
                <ActionName>CONFIGURE</ActionName>
                <ServiceName>KNOX</ServiceName>
                <DisplayName>Configure All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>START</ActionName>
                <ServiceName>KNOX</ServiceName>
                <DisplayName>Start All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>STOP</ActionName>
                <ServiceName>KNOX</ServiceName>
                <DisplayName>Stop All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>KNOX</ServiceName>
                <DisplayName>Restart All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>KNOX</ServiceName>
                <DisplayName>Restart Knox</DisplayName>
                <ComponentName>KNOX</ComponentName>
            </ServiceAction>
        </ServiceActionList>
        <ServiceName>KNOX</ServiceName>
        <notStartedNum>1</notStartedNum>
        <ServiceVersion>1.1.0</ServiceVersion>
        <ServiceDisplayName>Knox</ServiceDisplayName>
        <onlyClient>false</onlyClient>
    </ClusterInstalledService>
    <ClusterInstalledService>
        <needRestartNum>0</needRestartNum>
        <abnormalNum>0</abnormalNum>
        <ServiceActionList>
            <ServiceAction>
                <ActionName>CONFIGURE</ActionName>
                <ServiceName>OPENLDAP</ServiceName>
                <DisplayName>Configure All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>START</ActionName>
                <ServiceName>OPENLDAP</ServiceName>
                <DisplayName>Start All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>STOP</ActionName>
                <ServiceName>OPENLDAP</ServiceName>
                <DisplayName>Stop All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>OPENLDAP</ServiceName>
                <DisplayName>Restart All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>OPENLDAP</ServiceName>
                <DisplayName>Restart OpenLDAP</DisplayName>
                <ComponentName>OpenLDAP</ComponentName>
            </ServiceAction>
        </ServiceActionList>
        <ServiceName>OPENLDAP</ServiceName>
        <notStartedNum>0</notStartedNum>
        <ServiceVersion>2.4.44</ServiceVersion>
        <ServiceDisplayName>OpenLDAP</ServiceDisplayName>
        <onlyClient>false</onlyClient>
    </ClusterInstalledService>
    <ClusterInstalledService>
        <needRestartNum>0</needRestartNum>
        <abnormalNum>0</abnormalNum>
        <ServiceActionList>
            <ServiceAction>
                <ActionName>CONFIGURE</ActionName>
                <ServiceName>BIGBOOT</ServiceName>
                <DisplayName>Configure All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>START</ActionName>
                <ServiceName>BIGBOOT</ServiceName>
                <DisplayName>Start All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>STOP</ActionName>
                <ServiceName>BIGBOOT</ServiceName>
                <DisplayName>Stop All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>BIGBOOT</ServiceName>
                <DisplayName>Restart All Components</DisplayName>
                <ComponentName>ALL COMPONENTS</ComponentName>
            </ServiceAction>
            <ServiceAction>
                <ActionName>RESTART</ActionName>
                <ServiceName>BIGBOOT</ServiceName>
                <DisplayName>Restart Bigboot Monitor</DisplayName>
                <ComponentName>BigbootMonitor</ComponentName>
            </ServiceAction>
        </ServiceActionList>
        <ServiceName>BIGBOOT</ServiceName>
        <notStartedNum>0</notStartedNum>
        <ServiceVersion>2.7.202</ServiceVersion>
        <ServiceDisplayName>Bigboot</ServiceDisplayName>
        <onlyClient>false</onlyClient>
    </ClusterInstalledService>
</ClusterInstalledServiceList>
```

`JSON` format

```
{
    "RequestId": "F14E2754-DC4E-4D9C-BB59-3136FF6EF033",
    "ClusterInstalledServiceList": {
        "ClusterInstalledService": [
            {
                "needRestartNum": 0,
                "abnormalNum": 0,
                "ServiceActionList": {
                    "ServiceAction": [
                        {
                            "ActionName": "CONFIGURE",
                            "ServiceName": "HDFS",
                            "DisplayName": "Configure All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "START",
                            "ServiceName": "HDFS",
                            "DisplayName": "Start All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "STOP",
                            "ServiceName": "HDFS",
                            "DisplayName": "Stop All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "HDFS",
                            "DisplayName": "Restart All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "HDFS",
                            "DisplayName": "Restart DataNode",
                            "ComponentName": "DataNode"
                        },
                        {
                            "ActionName": "DECOMMISSION",
                            "ServiceName": "HDFS",
                            "DisplayName": "Decommission DataNode",
                            "ComponentName": "DataNode"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "HDFS",
                            "DisplayName": "Restart HttpFS",
                            "ComponentName": "HttpFS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "HDFS",
                            "DisplayName": "Restart KMS",
                            "ComponentName": "KMS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "HDFS",
                            "DisplayName": "Restart NameNode",
                            "ComponentName": "NameNode"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "HDFS",
                            "DisplayName": "Restart SecondaryNameNode",
                            "ComponentName": "SecondaryNameNode"
                        },
                        {
                            "ActionName": "CUSTOM_COMMAND",
                            "ServiceName": "HDFS",
                            "Command": "rebalance",
                            "DisplayName": "Rebalance",
                            "ComponentName": "NameNode"
                        },
                        {
                            "ActionName": "CUSTOM_COMMAND",
                            "ServiceName": "HDFS",
                            "Command": "stop_rebalance",
                            "DisplayName": "Stop Rebalance",
                            "ComponentName": "NameNode"
                        }
                    ]
                },
                "ServiceName": "HDFS",
                "notStartedNum": 0,
                "ServiceVersion": "2.8.5",
                "ServiceDisplayName": "HDFS",
                "onlyClient": false
            },
            {
                "needRestartNum": 0,
                "abnormalNum": 0,
                "ServiceActionList": {
                    "ServiceAction": [
                        {
                            "ActionName": "CONFIGURE",
                            "ServiceName": "YARN",
                            "DisplayName": "Configure All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "START",
                            "ServiceName": "YARN",
                            "DisplayName": "Start All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "STOP",
                            "ServiceName": "YARN",
                            "DisplayName": "Stop All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "YARN",
                            "DisplayName": "Restart All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "YARN",
                            "DisplayName": "Restart JobHistory",
                            "ComponentName": "JobHistory"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "YARN",
                            "DisplayName": "Restart NodeManager",
                            "ComponentName": "NodeManager"
                        },
                        {
                            "ActionName": "DECOMMISSION",
                            "ServiceName": "YARN",
                            "DisplayName": "Decommission NodeManager",
                            "ComponentName": "NodeManager"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "YARN",
                            "DisplayName": "Restart ResourceManager",
                            "ComponentName": "ResourceManager"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "YARN",
                            "DisplayName": "Restart App Timeline Server",
                            "ComponentName": "TimeLineServer"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "YARN",
                            "DisplayName": "Restart WebAppProxyServer",
                            "ComponentName": "WebAppProxyServer"
                        },
                        {
                            "ActionName": "CUSTOM_COMMAND",
                            "ServiceName": "YARN",
                            "Command": "refreshQueues",
                            "DisplayName": "Refresh Queues",
                            "ComponentName": "ResourceManager"
                        },
                        {
                            "ActionName": "CUSTOM_COMMAND",
                            "ServiceName": "YARN",
                            "Command": "enableCGroups",
                            "DisplayName": "Enable CGroups",
                            "ComponentName": "NodeManager"
                        },
                        {
                            "ActionName": "CUSTOM_COMMAND",
                            "ServiceName": "YARN",
                            "Command": "disableCGroups",
                            "DisplayName": "Disable CGroups",
                            "ComponentName": "NodeManager"
                        }
                    ]
                },
                "ServiceName": "YARN",
                "notStartedNum": 0,
                "ServiceVersion": "2.8.5",
                "ServiceDisplayName": "YARN",
                "onlyClient": false
            },
            {
                "needRestartNum": 0,
                "abnormalNum": 0,
                "ServiceActionList": {
                    "ServiceAction": [
                        {
                            "ActionName": "CONFIGURE",
                            "ServiceName": "HIVE",
                            "DisplayName": "Configure All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "START",
                            "ServiceName": "HIVE",
                            "DisplayName": "Start All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "STOP",
                            "ServiceName": "HIVE",
                            "DisplayName": "Stop All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "HIVE",
                            "DisplayName": "Restart All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "HIVE",
                            "DisplayName": "Restart Hive MetaStore",
                            "ComponentName": "HiveMetaStore"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "HIVE",
                            "DisplayName": "Restart HiveServer2",
                            "ComponentName": "HiveServer"
                        }
                    ]
                },
                "ServiceName": "HIVE",
                "notStartedNum": 0,
                "ServiceVersion": "2.3.5",
                "ServiceDisplayName": "Hive",
                "onlyClient": false
            },
            {
                "needRestartNum": 0,
                "abnormalNum": 0,
                "ServiceActionList": {
                    "ServiceAction": [
                        {
                            "ActionName": "CONFIGURE",
                            "ServiceName": "GANGLIA",
                            "DisplayName": "Configure All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "START",
                            "ServiceName": "GANGLIA",
                            "DisplayName": "Start All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "STOP",
                            "ServiceName": "GANGLIA",
                            "DisplayName": "Stop All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "GANGLIA",
                            "DisplayName": "Restart All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "GANGLIA",
                            "DisplayName": "Restart Gmetad",
                            "ComponentName": "GMETAD"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "GANGLIA",
                            "DisplayName": "Restart Gmond",
                            "ComponentName": "GMOND"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "GANGLIA",
                            "DisplayName": "Restart Httpd",
                            "ComponentName": "HTTPD"
                        }
                    ]
                },
                "ServiceName": "GANGLIA",
                "notStartedNum": 0,
                "ServiceVersion": "3.7.2",
                "ServiceDisplayName": "Ganglia",
                "onlyClient": false
            },
            {
                "needRestartNum": 0,
                "abnormalNum": 3,
                "ServiceActionList": {
                    "ServiceAction": [
                        {
                            "ActionName": "CONFIGURE",
                            "ServiceName": "ZOOKEEPER",
                            "DisplayName": "Configure All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "START",
                            "ServiceName": "ZOOKEEPER",
                            "DisplayName": "Start All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "STOP",
                            "ServiceName": "ZOOKEEPER",
                            "DisplayName": "Stop All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "ZOOKEEPER",
                            "DisplayName": "Restart All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "ZOOKEEPER",
                            "DisplayName": "Restart ZooKeeper",
                            "ComponentName": "ZOOKEEPER"
                        }
                    ]
                },
                "ServiceName": "ZOOKEEPER",
                "notStartedNum": 3,
                "ServiceVersion": "3.5.6",
                "ServiceDisplayName": "ZooKeeper",
                "onlyClient": false
            },
            {
                "needRestartNum": 0,
                "abnormalNum": 1,
                "ServiceActionList": {
                    "ServiceAction": [
                        {
                            "ActionName": "CONFIGURE",
                            "ServiceName": "SPARK",
                            "DisplayName": "Configure All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "START",
                            "ServiceName": "SPARK",
                            "DisplayName": "Start All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "STOP",
                            "ServiceName": "SPARK",
                            "DisplayName": "Stop All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "SPARK",
                            "DisplayName": "Restart All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "SPARK",
                            "DisplayName": "Restart SparkHistory",
                            "ComponentName": "SparkHistory"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "SPARK",
                            "DisplayName": "Restart ThriftServer",
                            "ComponentName": "SparkThriftServer"
                        }
                    ]
                },
                "ServiceName": "SPARK",
                "notStartedNum": 1,
                "ServiceVersion": "2.4.5",
                "ServiceDisplayName": "Spark",
                "onlyClient": false
            },
            {
                "needRestartNum": 0,
                "abnormalNum": 4,
                "ServiceActionList": {
                    "ServiceAction": [
                        {
                            "ActionName": "CONFIGURE",
                            "ServiceName": "HBASE",
                            "DisplayName": "Configure All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "START",
                            "ServiceName": "HBASE",
                            "DisplayName": "Start All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "STOP",
                            "ServiceName": "HBASE",
                            "DisplayName": "Stop All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "HBASE",
                            "DisplayName": "Restart All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "HBASE",
                            "DisplayName": "Restart HMaster",
                            "ComponentName": "HMaster"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "HBASE",
                            "DisplayName": "Restart HRegionServer",
                            "ComponentName": "HRegionServer"
                        },
                        {
                            "ActionName": "DECOMMISSION",
                            "ServiceName": "HBASE",
                            "DisplayName": "Decommission HRegionServer",
                            "ComponentName": "HRegionServer"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "HBASE",
                            "DisplayName": "Restart ThriftServer",
                            "ComponentName": "ThriftServer"
                        }
                    ]
                },
                "ServiceName": "HBASE",
                "notStartedNum": 4,
                "ServiceVersion": "1.4.9",
                "ServiceDisplayName": "HBase",
                "onlyClient": false
            },
            {
                "needRestartNum": 0,
                "abnormalNum": 0,
                "ServiceActionList": {
                    "ServiceAction": [
                        {
                            "ActionName": "CONFIGURE",
                            "ServiceName": "HUE",
                            "DisplayName": "Configure All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "START",
                            "ServiceName": "HUE",
                            "DisplayName": "Start All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "STOP",
                            "ServiceName": "HUE",
                            "DisplayName": "Stop All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "HUE",
                            "DisplayName": "Restart All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "HUE",
                            "DisplayName": "Restart Hue",
                            "ComponentName": "Hue"
                        }
                    ]
                },
                "ServiceName": "HUE",
                "notStartedNum": 0,
                "ServiceVersion": "4.4.0",
                "ServiceDisplayName": "Hue",
                "onlyClient": false
            },
            {
                "needRestartNum": 0,
                "abnormalNum": 1,
                "ServiceActionList": {
                    "ServiceAction": [
                        {
                            "ActionName": "CONFIGURE",
                            "ServiceName": "ZEPPELIN",
                            "DisplayName": "Configure All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "START",
                            "ServiceName": "ZEPPELIN",
                            "DisplayName": "Start All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "STOP",
                            "ServiceName": "ZEPPELIN",
                            "DisplayName": "Stop All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "ZEPPELIN",
                            "DisplayName": "Restart All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "ZEPPELIN",
                            "DisplayName": "Restart Zeppelin",
                            "ComponentName": "Zeppelin"
                        }
                    ]
                },
                "ServiceName": "ZEPPELIN",
                "notStartedNum": 1,
                "ServiceVersion": "0.8.1",
                "ServiceDisplayName": "Zeppelin",
                "onlyClient": false
            },
            {
                "needRestartNum": 0,
                "abnormalNum": 1,
                "ServiceActionList": {
                    "ServiceAction": [
                        {
                            "ActionName": "CONFIGURE",
                            "ServiceName": "OOZIE",
                            "DisplayName": "Configure All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "START",
                            "ServiceName": "OOZIE",
                            "DisplayName": "Start All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "STOP",
                            "ServiceName": "OOZIE",
                            "DisplayName": "Stop All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "OOZIE",
                            "DisplayName": "Restart All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "OOZIE",
                            "DisplayName": "Restart Oozie",
                            "ComponentName": "OOZIE"
                        }
                    ]
                },
                "ServiceName": "OOZIE",
                "notStartedNum": 1,
                "ServiceVersion": "5.1.0",
                "ServiceDisplayName": "Oozie",
                "onlyClient": false
            },
            {
                "needRestartNum": 0,
                "abnormalNum": 0,
                "ServiceActionList": {
                    "ServiceAction": [
                        {
                            "ActionName": "CONFIGURE",
                            "ServiceName": "TEZ",
                            "DisplayName": "Configure All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "START",
                            "ServiceName": "TEZ",
                            "DisplayName": "Start All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "STOP",
                            "ServiceName": "TEZ",
                            "DisplayName": "Stop All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "TEZ",
                            "DisplayName": "Restart All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "TEZ",
                            "DisplayName": "Restart Tez Client",
                            "ComponentName": "TezInit"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "TEZ",
                            "DisplayName": "Restart Tomcat",
                            "ComponentName": "Tomcat"
                        }
                    ]
                },
                "ServiceName": "TEZ",
                "notStartedNum": 0,
                "ServiceVersion": "0.9.2",
                "ServiceDisplayName": "Tez",
                "onlyClient": false
            },
            {
                "needRestartNum": 0,
                "abnormalNum": 0,
                "ServiceActionList": {
                    "ServiceAction": [
                        {
                            "ActionName": "CONFIGURE",
                            "ServiceName": "PHOENIX",
                            "DisplayName": "Configure All Components",
                            "ComponentName": "ALL COMPONENTS"
                        }
                    ]
                },
                "ServiceName": "PHOENIX",
                "notStartedNum": 0,
                "ServiceVersion": "4.14.1",
                "ServiceDisplayName": "Phoenix",
                "onlyClient": false
            },
            {
                "needRestartNum": 0,
                "abnormalNum": 1,
                "ServiceActionList": {
                    "ServiceAction": [
                        {
                            "ActionName": "CONFIGURE",
                            "ServiceName": "PRESTO",
                            "DisplayName": "Configure All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "START",
                            "ServiceName": "PRESTO",
                            "DisplayName": "Start All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "STOP",
                            "ServiceName": "PRESTO",
                            "DisplayName": "Stop All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "PRESTO",
                            "DisplayName": "Restart All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "PRESTO",
                            "DisplayName": "Restart PrestoMaster",
                            "ComponentName": "PrestoMaster"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "PRESTO",
                            "DisplayName": "Restart PrestoWorker",
                            "ComponentName": "PrestoWorker"
                        }
                    ]
                },
                "ServiceName": "PRESTO",
                "notStartedNum": 1,
                "ServiceVersion": "331",
                "ServiceDisplayName": "Presto",
                "onlyClient": false
            },
            {
                "needRestartNum": 0,
                "abnormalNum": 0,
                "ServiceActionList": {
                    "ServiceAction": [
                        {
                            "ActionName": "CONFIGURE",
                            "ServiceName": "SQOOP",
                            "DisplayName": "Configure All Components",
                            "ComponentName": "ALL COMPONENTS"
                        }
                    ]
                },
                "ServiceName": "SQOOP",
                "notStartedNum": 0,
                "ServiceVersion": "1.4.7",
                "ServiceDisplayName": "Sqoop",
                "onlyClient": false
            },
            {
                "needRestartNum": 0,
                "abnormalNum": 0,
                "ServiceActionList": {
                    "ServiceAction": [
                        {
                            "ActionName": "CONFIGURE",
                            "ServiceName": "PIG",
                            "DisplayName": "Configure All Components",
                            "ComponentName": "ALL COMPONENTS"
                        }
                    ]
                },
                "ServiceName": "PIG",
                "notStartedNum": 0,
                "ServiceVersion": "0.14.0",
                "ServiceDisplayName": "Pig",
                "onlyClient": false
            },
            {
                "needRestartNum": 0,
                "abnormalNum": 7,
                "ServiceActionList": {
                    "ServiceAction": [
                        {
                            "ActionName": "CONFIGURE",
                            "ServiceName": "STORM",
                            "DisplayName": "Configure All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "START",
                            "ServiceName": "STORM",
                            "DisplayName": "Start All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "STOP",
                            "ServiceName": "STORM",
                            "DisplayName": "Stop All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "STORM",
                            "DisplayName": "Restart All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "STORM",
                            "DisplayName": "Restart Logviewer",
                            "ComponentName": "Logviewer"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "STORM",
                            "DisplayName": "Restart Nimbus",
                            "ComponentName": "Nimbus"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "STORM",
                            "DisplayName": "Restart Supervisor",
                            "ComponentName": "Supervisor"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "STORM",
                            "DisplayName": "Restart UI",
                            "ComponentName": "UI"
                        }
                    ]
                },
                "ServiceName": "STORM",
                "notStartedNum": 7,
                "ServiceVersion": "1.2.2",
                "ServiceDisplayName": "Storm",
                "onlyClient": false
            },
            {
                "needRestartNum": 0,
                "abnormalNum": 5,
                "ServiceActionList": {
                    "ServiceAction": [
                        {
                            "ActionName": "CONFIGURE",
                            "ServiceName": "KUDU",
                            "DisplayName": "Configure All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "START",
                            "ServiceName": "KUDU",
                            "DisplayName": "Start All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "STOP",
                            "ServiceName": "KUDU",
                            "DisplayName": "Stop All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "KUDU",
                            "DisplayName": "Restart All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "KUDU",
                            "DisplayName": "Restart Kudu Master",
                            "ComponentName": "KuduMaster"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "KUDU",
                            "DisplayName": "Restart Kudu Tserver",
                            "ComponentName": "KuduTserver"
                        }
                    ]
                },
                "ServiceName": "KUDU",
                "notStartedNum": 5,
                "ServiceVersion": "1.10.0",
                "ServiceDisplayName": "Kudu",
                "onlyClient": false
            },
            {
                "needRestartNum": 0,
                "abnormalNum": 0,
                "ServiceActionList": {
                    "ServiceAction": [
                        {
                            "ActionName": "CONFIGURE",
                            "ServiceName": "FLUME",
                            "DisplayName": "Configure All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "START",
                            "ServiceName": "FLUME",
                            "DisplayName": "Start All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "STOP",
                            "ServiceName": "FLUME",
                            "DisplayName": "Stop All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "FLUME",
                            "DisplayName": "Restart All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "FLUME",
                            "DisplayName": "Restart Flume Agent",
                            "ComponentName": "FlumeAgent"
                        }
                    ]
                },
                "ServiceName": "FLUME",
                "notStartedNum": 3,
                "ServiceVersion": "1.9.0",
                "ServiceDisplayName": "FLUME",
                "onlyClient": false
            },
            {
                "needRestartNum": 0,
                "abnormalNum": 1,
                "ServiceActionList": {
                    "ServiceAction": [
                        {
                            "ActionName": "CONFIGURE",
                            "ServiceName": "LIVY",
                            "DisplayName": "Configure All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "START",
                            "ServiceName": "LIVY",
                            "DisplayName": "Start All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "STOP",
                            "ServiceName": "LIVY",
                            "DisplayName": "Stop All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "LIVY",
                            "DisplayName": "Restart All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "LIVY",
                            "DisplayName": "Restart Livy",
                            "ComponentName": "Livy"
                        }
                    ]
                },
                "ServiceName": "LIVY",
                "notStartedNum": 1,
                "ServiceVersion": "0.6.0",
                "ServiceDisplayName": "LIVY",
                "onlyClient": false
            },
            {
                "needRestartNum": 0,
                "abnormalNum": 1,
                "ServiceActionList": {
                    "ServiceAction": [
                        {
                            "ActionName": "CONFIGURE",
                            "ServiceName": "SUPERSET",
                            "DisplayName": "Configure All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "START",
                            "ServiceName": "SUPERSET",
                            "DisplayName": "Start All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "STOP",
                            "ServiceName": "SUPERSET",
                            "DisplayName": "Stop All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "SUPERSET",
                            "DisplayName": "Restart All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "SUPERSET",
                            "DisplayName": "Restart Superset",
                            "ComponentName": "Superset"
                        }
                    ]
                },
                "ServiceName": "SUPERSET",
                "notStartedNum": 1,
                "ServiceVersion": "0.35.2",
                "ServiceDisplayName": "Superset",
                "onlyClient": false
            },
            {
                "needRestartNum": 0,
                "abnormalNum": 1,
                "ServiceActionList": {
                    "ServiceAction": [
                        {
                            "ActionName": "CONFIGURE",
                            "ServiceName": "FLINK",
                            "DisplayName": "Configure All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "START",
                            "ServiceName": "FLINK",
                            "DisplayName": "Start All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "STOP",
                            "ServiceName": "FLINK",
                            "DisplayName": "Stop All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "FLINK",
                            "DisplayName": "Restart All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "FLINK",
                            "DisplayName": "Restart FlinkHistoryServer",
                            "ComponentName": "FlinkHistoryServer"
                        }
                    ]
                },
                "ServiceName": "FLINK",
                "notStartedNum": 1,
                "ServiceVersion": "1.10-vvr-1.0.4",
                "ServiceDisplayName": "Flink",
                "onlyClient": false
            },
            {
                "needRestartNum": 0,
                "abnormalNum": 4,
                "ServiceActionList": {
                    "ServiceAction": [
                        {
                            "ActionName": "CONFIGURE",
                            "ServiceName": "IMPALA",
                            "DisplayName": "Configure All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "START",
                            "ServiceName": "IMPALA",
                            "DisplayName": "Start All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "STOP",
                            "ServiceName": "IMPALA",
                            "DisplayName": "Stop All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "IMPALA",
                            "DisplayName": "Restart All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "IMPALA",
                            "DisplayName": "Restart Impala Catalog Server",
                            "ComponentName": "ImpalaCatalogd"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "IMPALA",
                            "DisplayName": "Restart Impala Daemon Server",
                            "ComponentName": "Impalad"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "IMPALA",
                            "DisplayName": "Restart Impala StateStore Server",
                            "ComponentName": "ImpalaStatestored"
                        }
                    ]
                },
                "ServiceName": "IMPALA",
                "notStartedNum": 4,
                "ServiceVersion": "2.12.2",
                "ServiceDisplayName": "Impala",
                "onlyClient": false
            },
            {
                "needRestartNum": 0,
                "abnormalNum": 2,
                "ServiceActionList": {
                    "ServiceAction": [
                        {
                            "ActionName": "CONFIGURE",
                            "ServiceName": "RANGER",
                            "DisplayName": "Configure All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "START",
                            "ServiceName": "RANGER",
                            "DisplayName": "Start All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "STOP",
                            "ServiceName": "RANGER",
                            "DisplayName": "Stop All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "RANGER",
                            "DisplayName": "Restart All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "RANGER",
                            "DisplayName": "Restart RangerAdmin",
                            "ComponentName": "RangerAdmin"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "RANGER",
                            "DisplayName": "Restart RangerUserSync",
                            "ComponentName": "RangerUserSync"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "RANGER",
                            "DisplayName": "Restart Solr",
                            "ComponentName": "Solr"
                        },
                        {
                            "ActionName": "CUSTOM_COMMAND",
                            "ServiceName": "RANGER",
                            "Command": "enableHDFS",
                            "DisplayName": "enableHDFS",
                            "ComponentName": "RangerAdmin"
                        },
                        {
                            "ActionName": "CUSTOM_COMMAND",
                            "ServiceName": "RANGER",
                            "Command": "disableHDFS",
                            "DisplayName": "disableHDFS",
                            "ComponentName": "RangerAdmin"
                        },
                        {
                            "ActionName": "CUSTOM_COMMAND",
                            "ServiceName": "RANGER",
                            "Command": "enableHive",
                            "DisplayName": "enableHive",
                            "ComponentName": "RangerAdmin"
                        },
                        {
                            "ActionName": "CUSTOM_COMMAND",
                            "ServiceName": "RANGER",
                            "Command": "disableHive",
                            "DisplayName": "disableHive",
                            "ComponentName": "RangerAdmin"
                        },
                        {
                            "ActionName": "CUSTOM_COMMAND",
                            "ServiceName": "RANGER",
                            "Command": "enableSpark",
                            "DisplayName": "enableSpark",
                            "ComponentName": "RangerAdmin"
                        },
                        {
                            "ActionName": "CUSTOM_COMMAND",
                            "ServiceName": "RANGER",
                            "Command": "disableSpark",
                            "DisplayName": "disableSpark",
                            "ComponentName": "RangerAdmin"
                        },
                        {
                            "ActionName": "CUSTOM_COMMAND",
                            "ServiceName": "RANGER",
                            "Command": "enablePresto",
                            "DisplayName": "enablePresto",
                            "ComponentName": "RangerAdmin"
                        },
                        {
                            "ActionName": "CUSTOM_COMMAND",
                            "ServiceName": "RANGER",
                            "Command": "disablePresto",
                            "DisplayName": "disablePresto",
                            "ComponentName": "RangerAdmin"
                        },
                        {
                            "ActionName": "CUSTOM_COMMAND",
                            "ServiceName": "RANGER",
                            "Command": "enableHBase",
                            "DisplayName": "enableHBase",
                            "ComponentName": "RangerPlugin"
                        },
                        {
                            "ActionName": "CUSTOM_COMMAND",
                            "ServiceName": "RANGER",
                            "Command": "disableHBase",
                            "DisplayName": "disableHBase",
                            "ComponentName": "RangerPlugin"
                        },
                        {
                            "ActionName": "CUSTOM_COMMAND",
                            "ServiceName": "RANGER",
                            "Command": "enableKafka",
                            "DisplayName": "enableKafka",
                            "ComponentName": "RangerPlugin"
                        },
                        {
                            "ActionName": "CUSTOM_COMMAND",
                            "ServiceName": "RANGER",
                            "Command": "disableKafka",
                            "DisplayName": "disableKafka",
                            "ComponentName": "RangerPlugin"
                        }
                    ]
                },
                "ServiceName": "RANGER",
                "notStartedNum": 2,
                "ServiceVersion": "1.2.0",
                "ServiceDisplayName": "RANGER",
                "onlyClient": false
            },
            {
                "needRestartNum": 0,
                "abnormalNum": 0,
                "ServiceActionList": {
                    "ServiceAction": [
                        {
                            "ActionName": "CONFIGURE",
                            "ServiceName": "SMARTDATA",
                            "DisplayName": "Configure All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "START",
                            "ServiceName": "SMARTDATA",
                            "DisplayName": "Start All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "STOP",
                            "ServiceName": "SMARTDATA",
                            "DisplayName": "Stop All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "SMARTDATA",
                            "DisplayName": "Restart All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "SMARTDATA",
                            "DisplayName": "Restart Jindo Namespace Service",
                            "ComponentName": "JindoNamespaceService"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "SMARTDATA",
                            "DisplayName": "Restart Jindo Storage Service",
                            "ComponentName": "JindoStorageService"
                        }
                    ]
                },
                "ServiceName": "SMARTDATA",
                "notStartedNum": 0,
                "ServiceVersion": "2.7.202",
                "ServiceDisplayName": "SmartData",
                "onlyClient": false
            },
            {
                "needRestartNum": 0,
                "abnormalNum": 1,
                "ServiceActionList": {
                    "ServiceAction": [
                        {
                            "ActionName": "CONFIGURE",
                            "ServiceName": "KNOX",
                            "DisplayName": "Configure All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "START",
                            "ServiceName": "KNOX",
                            "DisplayName": "Start All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "STOP",
                            "ServiceName": "KNOX",
                            "DisplayName": "Stop All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "KNOX",
                            "DisplayName": "Restart All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "KNOX",
                            "DisplayName": "Restart Knox",
                            "ComponentName": "KNOX"
                        }
                    ]
                },
                "ServiceName": "KNOX",
                "notStartedNum": 1,
                "ServiceVersion": "1.1.0",
                "ServiceDisplayName": "Knox",
                "onlyClient": false
            },
            {
                "needRestartNum": 0,
                "abnormalNum": 0,
                "ServiceActionList": {
                    "ServiceAction": [
                        {
                            "ActionName": "CONFIGURE",
                            "ServiceName": "OPENLDAP",
                            "DisplayName": "Configure All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "START",
                            "ServiceName": "OPENLDAP",
                            "DisplayName": "Start All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "STOP",
                            "ServiceName": "OPENLDAP",
                            "DisplayName": "Stop All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "OPENLDAP",
                            "DisplayName": "Restart All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "OPENLDAP",
                            "DisplayName": "Restart OpenLDAP",
                            "ComponentName": "OpenLDAP"
                        }
                    ]
                },
                "ServiceName": "OPENLDAP",
                "notStartedNum": 0,
                "ServiceVersion": "2.4.44",
                "ServiceDisplayName": "OpenLDAP",
                "onlyClient": false
            },
            {
                "needRestartNum": 0,
                "abnormalNum": 0,
                "ServiceActionList": {
                    "ServiceAction": [
                        {
                            "ActionName": "CONFIGURE",
                            "ServiceName": "BIGBOOT",
                            "DisplayName": "Configure All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "START",
                            "ServiceName": "BIGBOOT",
                            "DisplayName": "Start All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "STOP",
                            "ServiceName": "BIGBOOT",
                            "DisplayName": "Stop All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "BIGBOOT",
                            "DisplayName": "Restart All Components",
                            "ComponentName": "ALL COMPONENTS"
                        },
                        {
                            "ActionName": "RESTART",
                            "ServiceName": "BIGBOOT",
                            "DisplayName": "Restart Bigboot Monitor",
                            "ComponentName": "BigbootMonitor"
                        }
                    ]
                },
                "ServiceName": "BIGBOOT",
                "notStartedNum": 0,
                "ServiceVersion": "2.7.202",
                "ServiceDisplayName": "Bigboot",
                "onlyClient": false
            }
        ]
    }
}
```

## Errors

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/Emr).

