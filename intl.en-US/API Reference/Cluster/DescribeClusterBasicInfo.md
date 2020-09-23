# DescribeClusterBasicInfo

You can call this operation to query the basic information of a cluster.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=Emr&api=DescribeClusterBasicInfo&type=RPC&version=2016-04-08)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|DescribeClusterBasicInfo|The operation that you want to perform. For API requests using the HTTP or HTTPS URL, this parameter is required. Set this parameter to DescribeClusterBasicInfo. |
|ClusterId|String|Yes|C-0EF9B0EC8564\*\*\*\*|The ID of the cluster. You can use [ListClusters](~~28147~~) to view the cluster ID. |
|RegionId|String|Yes|cn-huhehaote|The region ID of the resource. You can use [DescribeRegions](~~25609~~) to view the latest list of Alibaba Cloud regions. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|ClusterInfo|Struct| |The detailed information about the cluster. |
|AccessInfo|Struct| |The access address of the cluster. |
|ZKLinks|Array of ZKLink| |The list of ZooKeeper link addresses. |
|ZKLink| | | |
|Link|String|192.168.\*. \*|The ZooKeeper endpoint address. |
|Port|String|2181|The ZooKeeper port. |
|AutoScalingAllowed|Boolean|true|Indicates whether the cluster supports auto scaling.

 -   true
-   false |
|AutoScalingByLoadAllowed|Boolean|true|Indicates whether the cluster supports scaling by load.

 -   true
-   false |
|AutoScalingEnable|Boolean|true|Indicates whether auto scaling is enabled for the cluster.

 -   true
-   false |
|AutoScalingSpotWithLimitAllowed|Boolean|true|Auto Scaling whether to allow the use of spotInstance.

 -   true
-   false |
|AutoScalingVersion|String|None|A reserved field. |
|AutoScalingWithGraceAllowed|Boolean|true|Graceful disconnection:

 -   true
-   false |
|BootstrapActionList|Array of BootstrapAction| |The list of bootstrap actions. |
|BootstrapAction| | | |
|Arg|String|name=NAME|The parameters of the bootstrap script. |
|Name|String|boot1|The name of the bootstrap script. |
|Path|String|oss://script/boot1|The OSS path of the bootstrap script. |
|BootstrapFailed|Boolean|false|The execution result of the bootstrap action:

 -   true: The operation is successful.
-   false: The operation failed. |
|ChargeType|String|PostPaid|The billing method of the cluster. Valid values:

 -   PostPaid: pay-as-you-go cluster
-   PrePaid: subscription cluster |
|ClusterId|String|C-02582A4A415E\*\*\*\*|The ID of the cluster. |
|Configurations|String|""|A reserved field. Currently, there is no return value for this field, so you do not need to pay attention to this field. |
|CoreNodeInService|Integer|0|A deprecated field. Currently, 0 is returned by default. This field will be removed from later versions. |
|CoreNodeTotal|Integer|0|A deprecated field. Currently, 0 is returned by default. This field will be removed from later versions. |
|CreateResource|String|ECM\_EMR|The deployment mode of the cluster.

 -   ECM\_EMR: default value.
-   RAW\_EMR: some older e-MapReduce versions, such as 1.3.0. |
|CreateType|String|MANUAL|The method of creating the cluster.

 -   MANUAL: Creates a value manually.
-   ON-DEMAND: Automatically created |
|DepositType|String|HALF\_MANAGED|The hosting type of the cluster. Valid values:

 -   HALF\_MANAGED: Semi-managed
-   MANAGED: fully MANAGED |
|EasEnable|Boolean|false|High-security mode of the cluster:

 -   true: high-security cluster
-   false: not a high-security cluster. |
|ExpiredTime|Long|1564367083000|The expiration time of a subscription cluster.

 **Note:** This parameter is not available for pay-as-you-go clusters. |
|ExtraInfo|String|-None-|The additional information about the Stack. |
|FailReason|Struct| |The reason why the cluster failed to be created. |
|ErrorCode|String|ClusterId.NotFound|The error code generated when cluster creation fails. |
|ErrorMsg|String|ClusterId \[null\] does not exist.|The error message displayed when cluster creation failed. |
|RequestId|String|FDFFCB97-1609-469A-B153-F511CA9FC1F5|The ID of the request in the background when cluster creation failed. |
|GatewayClusterIds|String|C-xxx|A deprecated field. This field will be removed from later versions. |
|GatewayClusterInfoList|Array of GatewayClusterInfo| |The list of gateway clusters associated with the cluster. |
|GatewayClusterInfo| | | |
|ClusterId|String|C-1AD4D95AF8B6\*\*\*\*|The ID of the Gateway cluster. |
|ClusterName|String|test\_gateway|The name of the Gateway cluster. |
|Status|String|IDLE|The status of the Gateway cluster. Valid values:

 -   CREATING: the cluster is being created.
-   CREATE\_FAILED: cluster creation failed.
-   RUNNING
-   IDLE: the cluster is IDLE.
-   RELEASING: the cluster is being released.
-   RELEASE\_FAILED: failed to release the cluster.
-   RELEASED: the cluster is RELEASED.
-   WAIT\_FOR\_PAY: to be paid
-   ABNORMAL: the cluster is ABNORMAL. |
|HighAvailabilityEnable|Boolean|true|Indicates whether the cluster is a high-availability cluster. |
|HostPoolInfo|Struct| |The information about the host pool that is used to create the cluster. This parameter is returned when the cluster is created by using the host pool. |
|HpBizId|String|HP-xxx|The ID of the host pool. |
|HpName|String|test\_hp|The name of the host pool. |
|ImageId|String|m-hp37dmahkdx7h2b5\*\*\*\*|The ID of the ECS image used by nodes in the cluster. |
|InstanceGeneration|String|ecs-2|A deprecated field. This field will be removed from later versions. |
|IoOptimized|Boolean|true|A deprecated field. This field will be removed from later versions. |
|K8sClusterId|String|None|A reserved field. |
|LocalMetaDb|Boolean|true|Indicates whether the cluster uses the local HIVE Metadatabase. Valid values:

 -   true: local Metadatabase
-   false: Unified Metadatabase. |
|LogEnable|Boolean|false|A deprecated field. This field will be removed from later versions. |
|LogPath|String|""|A deprecated field. This field will be removed from later versions. |
|MachineType|String|ECS|The node type of the cluster. Default value: ECS. |
|MasterNodeInService|Integer|0|A deprecated field. This field will be removed from later versions. |
|MasterNodeTotal|Integer|0|A deprecated field. This field will be removed from later versions. |
|MetaStoreType|String|LOCAL|Unified metadata type for clusters:

 -   LOCAL
-   USER\_RDS
-   UNIFIED |
|Name|String|test\_cluster|The name of the cluster. |
|NetType|String|vpc|The network type of the cluster. |
|Period|Integer|0|A deprecated field. This field will be removed from later versions. |
|RegionId|String|cn-huhehaote-a|The region ID of the resource. |
|RelateClusterId|String|C-xxx|A deprecated field. This field will be removed from later versions. |
|RelateClusterInfo|Struct| |The information about the primary cluster associated with the gateway cluster. |
|ClusterId|String|C-4A502AECB275\*\*\*\*|The ID of the EMR cluster. |
|ClusterName|String|test\_hadoop|The name of the EMR cluster. |
|Status|String|IDLE|The status of the associated cluster. |
|ResizeDiskEnable|Boolean|true|Indicates whether the cluster supports disk resizing. |
|RunningTime|Integer|1583000|The running duration of the cluster. Unit: milliseconds. |
|SecurityGroupId|String|sg-xxx|The ID of the security group of the cluster. |
|SecurityGroupName|String|emr-default-securitygroup|The name of the security group of the cluster. |
|ShowSoftwareInterface|Boolean|false|A deprecated field. This field will be removed from later versions. |
|SoftwareInfo|Struct| |The information about the software services deployed in the cluster. |
|ClusterType|String|HADOOP|The type of the clusters. |
|EmrVer|String|EMR-3.21.0|The EMR version. |
|Softwares|Array of Software| |The information of the service. |
|Software| | | |
|DisplayName|String|HDFS|The display name of the service. |
|Name|String|HDFS|The service name. |
|OnlyDisplay|Boolean|false|A deprecated field. This field will be removed from later versions. |
|StartTpe|Integer|1|A deprecated field. This field will be removed from later versions. |
|Version|String|2.8.5|The version of the service. |
|StartTime|Long|1564367083000|The time when the cluster started. |
|Status|String|IDLE|The status of the cluster. Valid values:

 -   CREATING: the cluster is being created.
-   CREATE\_FAILED: cluster creation failed.
-   RUNNING
-   IDLE: the cluster is IDLE.
-   RELEASING: the cluster is being released.
-   RELEASE\_FAILED: failed to release the cluster.
-   RELEASED: the cluster is RELEASED.
-   WAIT\_FOR\_PAY: to be paid
-   ABNORMAL: the cluster is ABNORMAL. |
|StopTime|Long|1564367083000|The time when the cluster stops. |
|TaskNodeInService|Integer|0|A deprecated field. This field will be removed from later versions. |
|TaskNodeTotal|Integer|0|A deprecated field. This field will be removed from later versions. |
|UserDefinedEmrEcsRole|String|AliyunEmrEcsDefaultRole|The name of the service role of E-MapReduce. |
|UserId|String|11131321311\*\*\*\*|The ID of the user account. |
|VSwitchId|String|vsw-xxx|The ID of the VSwitch. |
|VpcId|String|vpc-xxx|The ID of the VPC. |
|ZoneId|String|cn-huhehaote-a|The ID of the zone. |
|RequestId|String|BB81FF02-2FE0-4DB0-A04B-A0876D21EDD7|The ID of the request. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=DescribeClusterBasicInfo
&ClusterId=C-0EF9B0EC8564****
&RegionId=cn-huhehaote
&<Common request parameters>
```

Sample success responses

`XML` format

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

`JSON` format

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

## Error codes

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|403|Params.Illegal|The specified parameters are wrongly formed..|The error message returned because the format of the specified parameter is invalid.|
|404|ClusterId.NotFound|ClusterId \[%s\] does not exist.|The error message returned because the specified cluster ID does not exist. Make sure that the cluster ID is valid.|
|403|User.OtherUserResource.NotAllow|It is not allowed to operate other user's resource|The error message returned because you are not allowed to manage resources of other users.|
|500|InternalError|The request processing has failed due to some unknown error.|The error message returned because the request processing has failed due to an internal error. Submit a ticket.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/Emr).

