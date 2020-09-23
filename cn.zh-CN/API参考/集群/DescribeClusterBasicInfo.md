# DescribeClusterBasicInfo

调用DescribeClusterBasicInfo接口，查询已创建集群的基本信息。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=DescribeClusterBasicInfo&type=RPC&version=2016-04-08)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DescribeClusterBasicInfo|系统规定参数。对于您自行拼凑HTTP或HTTPS URL发起的API请求，该参数为必选参数。取值：DescribeClusterBasicInfo。 |
|ClusterId|String|是|C-0EF9B0EC8564\*\*\*\*|集群的ID。您可以通过[ListClusters](~~28147~~)接口查看集群的ID。 |
|RegionId|String|是|cn-huhehaote|地域ID。您可以通过[DescribeRegions](~~25609~~)接口查看最新的阿里云地域列表。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|ClusterInfo|Struct| |集群详情信息。 |
|AccessInfo|Struct| |集群的访问地址信息。 |
|ZKLinks|Array of ZKLink| |ZooKeeper的链接地址信息列表。 |
|ZKLink| | | |
|Link|String|192.168.\*.\*|ZooKeeper的访问链接地址。 |
|Port|String|2181|ZooKeeper的端口。 |
|AutoScalingAllowed|Boolean|true|集群是否支持弹性伸缩：

 -   true：是
-   false：否 |
|AutoScalingByLoadAllowed|Boolean|true|集群是否支持按负载伸缩：

 -   true：是
-   false：否 |
|AutoScalingEnable|Boolean|true|集群当前是否开启了弹性伸缩：

 -   true：是
-   false：否 |
|AutoScalingSpotWithLimitAllowed|Boolean|true|弹性伸缩是否允许使用竞价实例（spotInstance）：

 -   true：是
-   false：否 |
|AutoScalingVersion|String|无|保留字段。 |
|AutoScalingWithGraceAllowed|Boolean|true|是否优雅下线：

 -   true：是
-   false：否 |
|BootstrapActionList|Array of BootstrapAction| |集群的引导脚本信息。 |
|BootstrapAction| | | |
|Arg|String|name=NAME|引导脚本参数。 |
|Name|String|boot1|引导脚本名称。 |
|Path|String|oss://script/boot1|引导脚本的OSS路径。 |
|BootstrapFailed|Boolean|false|引导操作的执行结果：

 -   true：执行成功。
-   false：执行失败。 |
|ChargeType|String|PostPaid|集群的付费类型：

 -   PostPaid：按量付费集群
-   PrePaid：包年包月集群 |
|ClusterId|String|C-02582A4A415E\*\*\*\*|集群的ID。 |
|Configurations|String|""|保留字段，目前该字段无返回值，无需关注。 |
|CoreNodeInService|Integer|0|已废弃字段，目前默认返回0，后续会移除该字段。 |
|CoreNodeTotal|Integer|0|已废弃字段，目前默认返回0，后续会移除该字段。 |
|CreateResource|String|ECM\_EMR|集群的部署方式：

 -   ECM\_EMR：默认值。
-   RAW\_EMR：部分比较旧的EMR主版本，例如1.3.0等创建的集群。 |
|CreateType|String|MANUAL|集群的创建方式：

 -   MANUAL：手动创建
-   ON-DEMAND：自动创建 |
|DepositType|String|HALF\_MANAGED|集群的托管类型：

 -   HALF\_MANAGED：半托管
-   MANAGED：全托管 |
|EasEnable|Boolean|false|集群的高安全模式：

 -   true：高安全集群
-   false：非高安全集群 |
|ExpiredTime|Long|1564367083000|付费类型为包年包月的集群的过期时间。

 **说明：** 按量付费类型的集群不显示此参数。 |
|ExtraInfo|String|-None-|Stack附加信息。 |
|FailReason|Struct| |集群创建失败时的失败原因。 |
|ErrorCode|String|ClusterId.NotFound|集群创建失败时的错误码。 |
|ErrorMsg|String|ClusterId \[null\] does not exist.|集群创建失败时的错误信息。 |
|RequestId|String|FDFFCB97-1609-469A-B153-F511CA9FC1F5|集群创建失败时，对应的后台请求ID。 |
|GatewayClusterIds|String|C-xxx|废弃字段，后续版本中将移除该字段。 |
|GatewayClusterInfoList|Array of GatewayClusterInfo| |当前集群关联的Gateway集群信息列表。 |
|GatewayClusterInfo| | | |
|ClusterId|String|C-1AD4D95AF8B6\*\*\*\*|Gateway集群的ID。 |
|ClusterName|String|test\_gateway|Gateway集群名称。 |
|Status|String|IDLE|Gateway集群状态：

 -   CREATING：集群创建中
-   CREATE\_FAILED：集群创建失败
-   RUNNING：运行中
-   IDLE：集群空闲
-   RELEASING：集群释放中
-   RELEASE\_FAILED：集群释放失败
-   RELEASED：集群已释放
-   WAIT\_FOR\_PAY：待支付
-   ABNORMAL：集群状态异常 |
|HighAvailabilityEnable|Boolean|true|集群是否为高可用集群。 |
|HostPoolInfo|Struct| |集群对应的机器池信息。当集群是通过机器池创建时，该字段会返回对应的机器池信息。 |
|HpBizId|String|HP-xxx|机器池ID。 |
|HpName|String|test\_hp|机器池名称。 |
|ImageId|String|m-hp37dmahkdx7h2b5\*\*\*\*|集群的节点使用的ECS镜像ID。 |
|InstanceGeneration|String|ecs-2|废弃字段，后续版本中将移除该字段。 |
|IoOptimized|Boolean|true|废弃字段，后续版本中将移除该字段。 |
|K8sClusterId|String|无|保留字段。 |
|LocalMetaDb|Boolean|true|集群是否使用的本地HIVE元数据库：

 -   true：本地元数据库
-   false：统一元数据库 |
|LogEnable|Boolean|false|废弃字段，后续版本中将移除该字段。 |
|LogPath|String|""|废弃字段，后续版本中将移除该字段。 |
|MachineType|String|ECS|集群的节点类型，默认为ECS。 |
|MasterNodeInService|Integer|0|废弃字段，后续版本中将移除该字段。 |
|MasterNodeTotal|Integer|0|废弃字段，后续版本中将移除该字段。 |
|MetaStoreType|String|LOCAL|集群的统一元数据类型：

 -   LOCAL
-   USER\_RDS
-   UNIFIED |
|Name|String|test\_cluster|集群名称。 |
|NetType|String|vpc|集群的网络类型。 |
|Period|Integer|0|废弃字段，后续版本中将移除该字段。 |
|RegionId|String|cn-huhehaote-a|地域ID。 |
|RelateClusterId|String|C-xxx|废弃字段，后续版本中将移除该字段。 |
|RelateClusterInfo|Struct| |Gateway集群对应的主集群信息。 |
|ClusterId|String|C-4A502AECB275\*\*\*\*|关联集群ID。 |
|ClusterName|String|test\_hadoop|关联集群名称。 |
|Status|String|IDLE|关联集群状态。 |
|ResizeDiskEnable|Boolean|true|当前集群是否支持磁盘扩容功能。 |
|RunningTime|Integer|1583000|集群当前的运行时长。 |
|SecurityGroupId|String|sg-xxx|集群的安全组ID。 |
|SecurityGroupName|String|emr-default-securitygroup|集群的安全组名称。 |
|ShowSoftwareInterface|Boolean|false|废弃字段，后续版本中将移除该字段。 |
|SoftwareInfo|Struct| |集群上部署的软件服务信息。 |
|ClusterType|String|HADOOP|集群类型。 |
|EmrVer|String|EMR-3.21.0|EMR版本。 |
|Softwares|Array of Software| |服务信息列表。 |
|Software| | | |
|DisplayName|String|HDFS|服务的显示名称。 |
|Name|String|HDFS|服务名称。 |
|OnlyDisplay|Boolean|false|废弃字段，后续版本中将移除该字段。 |
|StartTpe|Integer|1|废弃字段，后续版本中将移除该字段。 |
|Version|String|2.8.5|服务的版本信息。 |
|StartTime|Long|1564367083000|集群的开始时间。 |
|Status|String|IDLE|集群的状态：

 -   CREATING：集群创建中
-   CREATE\_FAILED：集群创建失败
-   RUNNING：运行中
-   IDLE：集群空闲
-   RELEASING：集群释放中
-   RELEASE\_FAILED：集群释放失败
-   RELEASED：集群已释放
-   WAIT\_FOR\_PAY：待支付
-   ABNORMAL：集群状态异常 |
|StopTime|Long|1564367083000|停止时间。 |
|TaskNodeInService|Integer|0|废弃字段，后续版本中将移除该字段。 |
|TaskNodeTotal|Integer|0|废弃字段，后续版本中将移除该字段。 |
|UserDefinedEmrEcsRole|String|AliyunEmrEcsDefaultRole|E-MapReduce的服务角色名称。 |
|UserId|String|11131321311\*\*\*\*|用户ID。 |
|VSwitchId|String|vsw-xxx|交换机ID。 |
|VpcId|String|vpc-xxx|VPC的ID。 |
|ZoneId|String|cn-huhehaote-a|可用区ID。 |
|RequestId|String|BB81FF02-2FE0-4DB0-A04B-A0876D21EDD7|请求ID。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=DescribeClusterBasicInfo
&ClusterId=C-0EF9B0EC8564****
&RegionId=cn-huhehaote
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<ClusterInfo>
    <ImageId>m-hp37dmahkdx7h2b5****</ImageId>
    <SecurityGroupId>sg-hp360xqw5l179aty****</SecurityGroupId>
    <DepositType>HALF_MANAGED</DepositType>
    <CoreNodeInService>0</CoreNodeInService>
    <ZoneId>cn-huhehaote-a</ZoneId>
    <AutoScalingSpotWithLimitAllowed>false</AutoScalingSpotWithLimitAllowed>
    <IoOptimized>true</IoOptimized>
    <TaskNodeInService>0</TaskNodeInService>
    <MachineType>ECS</MachineType>
    <AutoScalingAllowed>false</AutoScalingAllowed>
    <ShowSoftwareInterface>false</ShowSoftwareInterface>
    <ResizeDiskEnable>false</ResizeDiskEnable>
    <SecurityGroupName>sg-hp360xqw5l179aty****</SecurityGroupName>
    <BootstrapActionList>
    </BootstrapActionList>
    <CreateResource>ECM_EMR</CreateResource>
    <RegionId>cn-huhehaote</RegionId>
    <GatewayClusterInfoList>
    </GatewayClusterInfoList>
    <NetType>vpc</NetType>
    <MasterNodeInService>0</MasterNodeInService>
    <AutoScalingByLoadAllowed>true</AutoScalingByLoadAllowed>
    <SoftwareInfo>
        <Softwares>
            <Software>
                <Name>HDFS</Name>
                <OnlyDisplay>false</OnlyDisplay>
                <Version>2.8.5</Version>
                <DisplayName>HDFS</DisplayName>
                <StartTpe>1</StartTpe>
            </Software>
            <Software>
                <Name>YARN</Name>
                <OnlyDisplay>false</OnlyDisplay>
                <Version>2.8.5</Version>
                <DisplayName>YARN</DisplayName>
                <StartTpe>1</StartTpe>
            </Software>
            <Software>
                <Name>HIVE</Name>
                <OnlyDisplay>false</OnlyDisplay>
                <Version>3.1.1</Version>
                <DisplayName>Hive</DisplayName>
                <StartTpe>1</StartTpe>
            </Software>
            <Software>
                <Name>GANGLIA</Name>
                <OnlyDisplay>false</OnlyDisplay>
                <Version>3.7.2</Version>
                <DisplayName>Ganglia</DisplayName>
                <StartTpe>1</StartTpe>
            </Software>
            <Software>
                <Name>ZOOKEEPER</Name>
                <OnlyDisplay>false</OnlyDisplay>
                <Version>3.4.13</Version>
                <DisplayName>Zookeeper</DisplayName>
                <StartTpe>1</StartTpe>
            </Software>
            <Software>
                <Name>SPARK</Name>
                <OnlyDisplay>false</OnlyDisplay>
                <Version>2.4.3</Version>
                <DisplayName>Spark</DisplayName>
                <StartTpe>1</StartTpe>
            </Software>
            <Software>
                <Name>HBASE</Name>
                <OnlyDisplay>false</OnlyDisplay>
                <Version>1.4.9</Version>
                <DisplayName>HBase</DisplayName>
                <StartTpe>1</StartTpe>
            </Software>
            <Software>
                <Name>HUE</Name>
                <OnlyDisplay>false</OnlyDisplay>
                <Version>4.4.0</Version>
                <DisplayName>HUE</DisplayName>
                <StartTpe>1</StartTpe>
            </Software>
            <Software>
                <Name>ZEPPELIN</Name>
                <OnlyDisplay>false</OnlyDisplay>
                <Version>0.8.1</Version>
                <DisplayName>Zeppelin</DisplayName>
                <StartTpe>1</StartTpe>
            </Software>
            <Software>
                <Name>OOZIE</Name>
                <OnlyDisplay>false</OnlyDisplay>
                <Version>5.1.0</Version>
                <DisplayName>Oozie</DisplayName>
                <StartTpe>1</StartTpe>
            </Software>
            <Software>
                <Name>TEZ</Name>
                <OnlyDisplay>false</OnlyDisplay>
                <Version>0.9.1</Version>
                <DisplayName>Tez</DisplayName>
                <StartTpe>1</StartTpe>
            </Software>
            <Software>
                <Name>PHOENIX</Name>
                <OnlyDisplay>false</OnlyDisplay>
                <Version>4.14.1</Version>
                <DisplayName>Phoenix</DisplayName>
                <StartTpe>1</StartTpe>
            </Software>
            <Software>
                <Name>PRESTO</Name>
                <OnlyDisplay>false</OnlyDisplay>
                <Version>0.213</Version>
                <DisplayName>Presto</DisplayName>
                <StartTpe>1</StartTpe>
            </Software>
            <Software>
                <Name>SQOOP</Name>
                <OnlyDisplay>false</OnlyDisplay>
                <Version>1.4.7</Version>
                <DisplayName>Sqoop</DisplayName>
                <StartTpe>1</StartTpe>
            </Software>
            <Software>
                <Name>PIG</Name>
                <OnlyDisplay>false</OnlyDisplay>
                <Version>0.14.0</Version>
                <DisplayName>Pig</DisplayName>
                <StartTpe>1</StartTpe>
            </Software>
            <Software>
                <Name>STORM</Name>
                <OnlyDisplay>false</OnlyDisplay>
                <Version>1.2.2</Version>
                <DisplayName>Storm</DisplayName>
                <StartTpe>1</StartTpe>
            </Software>
            <Software>
                <Name>IMPALA</Name>
                <OnlyDisplay>false</OnlyDisplay>
                <Version>2.12.2</Version>
                <DisplayName>Impala</DisplayName>
                <StartTpe>1</StartTpe>
            </Software>
            <Software>
                <Name>FLINK</Name>
                <OnlyDisplay>false</OnlyDisplay>
                <Version>1.7.2</Version>
                <DisplayName>Flink</DisplayName>
                <StartTpe>1</StartTpe>
            </Software>
            <Software>
                <Name>RANGER</Name>
                <OnlyDisplay>false</OnlyDisplay>
                <Version>1.2.0</Version>
                <DisplayName>Ranger</DisplayName>
                <StartTpe>1</StartTpe>
            </Software>
            <Software>
                <Name>KNOX</Name>
                <OnlyDisplay>false</OnlyDisplay>
                <Version>1.1.0</Version>
                <DisplayName>Knox</DisplayName>
                <StartTpe>1</StartTpe>
            </Software>
            <Software>
                <Name>SUPERSET</Name>
                <OnlyDisplay>false</OnlyDisplay>
                <Version>0.28.1</Version>
                <DisplayName>Superset</DisplayName>
                <StartTpe>1</StartTpe>
            </Software>
            <Software>
                <Name>BIGBOOT</Name>
                <OnlyDisplay>false</OnlyDisplay>
                <Version>1.0.0</Version>
                <DisplayName>Bigboot</DisplayName>
                <StartTpe>1</StartTpe>
            </Software>
            <Software>
                <Name>LIVY</Name>
                <OnlyDisplay>false</OnlyDisplay>
                <Version>0.6.0</Version>
                <DisplayName>LIVY</DisplayName>
                <StartTpe>1</StartTpe>
            </Software>
            <Software>
                <Name>SMARTDATA</Name>
                <OnlyDisplay>false</OnlyDisplay>
                <Version>1.0.0</Version>
                <DisplayName>SmartData</DisplayName>
                <StartTpe>1</StartTpe>
            </Software>
            <Software>
                <Name>FLUME</Name>
                <OnlyDisplay>false</OnlyDisplay>
                <Version>1.8.0</Version>
                <DisplayName>FLUME</DisplayName>
                <StartTpe>1</StartTpe>
            </Software>
            <Software>
                <Name>ANALYTICS-ZOO</Name>
                <OnlyDisplay>false</OnlyDisplay>
                <Version>0.5.0</Version>
                <DisplayName>analytics-zoo</DisplayName>
                <StartTpe>1</StartTpe>
            </Software>
            <Software>
                <Name>APACHEDS</Name>
                <OnlyDisplay>false</OnlyDisplay>
                <Version>2.0.0</Version>
                <DisplayName>ApacheDS</DisplayName>
                <StartTpe>1</StartTpe>
            </Software>
        </Softwares>
        <EmrVer>EMR-3.21.1</EmrVer>
        <ClusterType>HADOOP</ClusterType>
    </SoftwareInfo>
    <CreateType>MANUAL</CreateType>
    <VSwitchId>vsw-hp333ghqgdxkk3djb****</VSwitchId>
    <VpcId>vpc-hp3fawn3tu8ny436o****</VpcId>
    <TaskNodeTotal>0</TaskNodeTotal>
    <BootstrapFailed>false</BootstrapFailed>
    <CoreNodeTotal>0</CoreNodeTotal>
    <StartTime>1564132024000</StartTime>
    <MasterNodeTotal>0</MasterNodeTotal>
    <ChargeType>PostPaid</ChargeType>
    <Configurations/>
    <UserId>107992689699****</UserId>
    <Name>EMR-API-test</Name>
    <EasEnable>false</EasEnable>
    <AutoScalingEnable>false</AutoScalingEnable>
    <Status>IDLE</Status>
    <LocalMetaDb>true</LocalMetaDb>
    <HighAvailabilityEnable>false</HighAvailabilityEnable>
    <ClusterId>C-02582A4A415E****</ClusterId>
    <RunningTime>1004</RunningTime>
    <UserDefinedEmrEcsRole>AliyunEmrEcsDefaultRole</UserDefinedEmrEcsRole>
</ClusterInfo>
<RequestId>BB81FF02-2FE0-4DB0-A04B-A0876D21EDD7</RequestId>
```

`JSON` 格式

```
{
    "ClusterInfo": {
        "ImageId": "m-hp37dmahkdx7h2b5****",
        "SecurityGroupId": "sg-hp360xqw5l179aty****",
        "DepositType": "HALF_MANAGED",
        "CoreNodeInService": 0,
        "ZoneId": "cn-huhehaote-a",
        "AutoScalingSpotWithLimitAllowed": false,
        "IoOptimized": true,
        "TaskNodeInService": 0,
        "MachineType": "ECS",
        "AutoScalingAllowed": false,
        "ShowSoftwareInterface": false,
        "ResizeDiskEnable": false,
        "SecurityGroupName": "sg-hp360xqw5l179aty****",
        "BootstrapActionList": {
            "BootstrapAction": []
        },
        "CreateResource": "ECM_EMR",
        "RegionId": "cn-huhehaote",
        "GatewayClusterInfoList": {
            "GatewayClusterInfo": []
        },
        "NetType": "vpc",
        "MasterNodeInService": 0,
        "AutoScalingByLoadAllowed": true,
        "SoftwareInfo": {
            "Softwares": {
                "Software": [
                    {
                        "Name": "HDFS",
                        "OnlyDisplay": false,
                        "Version": "2.8.5",
                        "DisplayName": "HDFS",
                        "StartTpe": 1
                    },
                    {
                        "Name": "YARN",
                        "OnlyDisplay": false,
                        "Version": "2.8.5",
                        "DisplayName": "YARN",
                        "StartTpe": 1
                    },
                    {
                        "Name": "HIVE",
                        "OnlyDisplay": false,
                        "Version": "3.1.1",
                        "DisplayName": "Hive",
                        "StartTpe": 1
                    },
                    {
                        "Name": "GANGLIA",
                        "OnlyDisplay": false,
                        "Version": "3.7.2",
                        "DisplayName": "Ganglia",
                        "StartTpe": 1
                    },
                    {
                        "Name": "ZOOKEEPER",
                        "OnlyDisplay": false,
                        "Version": "3.4.13",
                        "DisplayName": "Zookeeper",
                        "StartTpe": 1
                    },
                    {
                        "Name": "SPARK",
                        "OnlyDisplay": false,
                        "Version": "2.4.3",
                        "DisplayName": "Spark",
                        "StartTpe": 1
                    },
                    {
                        "Name": "HBASE",
                        "OnlyDisplay": false,
                        "Version": "1.4.9",
                        "DisplayName": "HBase",
                        "StartTpe": 1
                    },
                    {
                        "Name": "HUE",
                        "OnlyDisplay": false,
                        "Version": "4.4.0",
                        "DisplayName": "HUE",
                        "StartTpe": 1
                    },
                    {
                        "Name": "ZEPPELIN",
                        "OnlyDisplay": false,
                        "Version": "0.8.1",
                        "DisplayName": "Zeppelin",
                        "StartTpe": 1
                    },
                    {
                        "Name": "OOZIE",
                        "OnlyDisplay": false,
                        "Version": "5.1.0",
                        "DisplayName": "Oozie",
                        "StartTpe": 1
                    },
                    {
                        "Name": "TEZ",
                        "OnlyDisplay": false,
                        "Version": "0.9.1",
                        "DisplayName": "Tez",
                        "StartTpe": 1
                    },
                    {
                        "Name": "PHOENIX",
                        "OnlyDisplay": false,
                        "Version": "4.14.1",
                        "DisplayName": "Phoenix",
                        "StartTpe": 1
                    },
                    {
                        "Name": "PRESTO",
                        "OnlyDisplay": false,
                        "Version": "0.213",
                        "DisplayName": "Presto",
                        "StartTpe": 1
                    },
                    {
                        "Name": "SQOOP",
                        "OnlyDisplay": false,
                        "Version": "1.4.7",
                        "DisplayName": "Sqoop",
                        "StartTpe": 1
                    },
                    {
                        "Name": "PIG",
                        "OnlyDisplay": false,
                        "Version": "0.14.0",
                        "DisplayName": "Pig",
                        "StartTpe": 1
                    },
                    {
                        "Name": "STORM",
                        "OnlyDisplay": false,
                        "Version": "1.2.2",
                        "DisplayName": "Storm",
                        "StartTpe": 1
                    },
                    {
                        "Name": "IMPALA",
                        "OnlyDisplay": false,
                        "Version": "2.12.2",
                        "DisplayName": "Impala",
                        "StartTpe": 1
                    },
                    {
                        "Name": "FLINK",
                        "OnlyDisplay": false,
                        "Version": "1.7.2",
                        "DisplayName": "Flink",
                        "StartTpe": 1
                    },
                    {
                        "Name": "RANGER",
                        "OnlyDisplay": false,
                        "Version": "1.2.0",
                        "DisplayName": "Ranger",
                        "StartTpe": 1
                    },
                    {
                        "Name": "KNOX",
                        "OnlyDisplay": false,
                        "Version": "1.1.0",
                        "DisplayName": "Knox",
                        "StartTpe": 1
                    },
                    {
                        "Name": "SUPERSET",
                        "OnlyDisplay": false,
                        "Version": "0.28.1",
                        "DisplayName": "Superset",
                        "StartTpe": 1
                    },
                    {
                        "Name": "BIGBOOT",
                        "OnlyDisplay": false,
                        "Version": "1.0.0",
                        "DisplayName": "Bigboot",
                        "StartTpe": 1
                    },
                    {
                        "Name": "LIVY",
                        "OnlyDisplay": false,
                        "Version": "0.6.0",
                        "DisplayName": "LIVY",
                        "StartTpe": 1
                    },
                    {
                        "Name": "SMARTDATA",
                        "OnlyDisplay": false,
                        "Version": "1.0.0",
                        "DisplayName": "SmartData",
                        "StartTpe": 1
                    },
                    {
                        "Name": "FLUME",
                        "OnlyDisplay": false,
                        "Version": "1.8.0",
                        "DisplayName": "FLUME",
                        "StartTpe": 1
                    },
                    {
                        "Name": "ANALYTICS-ZOO",
                        "OnlyDisplay": false,
                        "Version": "0.5.0",
                        "DisplayName": "analytics-zoo",
                        "StartTpe": 1
                    },
                    {
                        "Name": "APACHEDS",
                        "OnlyDisplay": false,
                        "Version": "2.0.0",
                        "DisplayName": "ApacheDS",
                        "StartTpe": 1
                    }
                ]
            },
            "EmrVer": "EMR-3.21.1",
            "ClusterType": "HADOOP"
        },
        "CreateType": "MANUAL",
        "VSwitchId": "vsw-hp333ghqgdxkk3djb****",
        "VpcId": "vpc-hp3fawn3tu8ny436o****",
        "TaskNodeTotal": 0,
        "BootstrapFailed": false,
        "CoreNodeTotal": 0,
        "StartTime": 1564132024000,
        "MasterNodeTotal": 0,
        "ChargeType": "PostPaid",
        "Configurations": "",
        "UserId": "107992689699****",
        "Name": "EMR-API-test",
        "EasEnable": false,
        "AutoScalingEnable": false,
        "Status": "IDLE",
        "LocalMetaDb": true,
        "HighAvailabilityEnable": false,
        "ClusterId": "C-02582A4A415E****",
        "RunningTime": 1004,
        "UserDefinedEmrEcsRole": "AliyunEmrEcsDefaultRole"
    },
    "RequestId": "BB81FF02-2FE0-4DB0-A04B-A0876D21EDD7"
}
```

