# DescribeClusterV2 {#doc_api_Emr_DescribeClusterV2 .reference}

调用 DescribeClusterV2接口，查询集群的基本信息，包括：付费、ECS机器概况、E-MapReduce服务服务列表等。

## 调试 {#api_explorer .section}

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=DescribeClusterV2&type=RPC&version=2016-04-08)

## 请求参数 {#parameters .section}

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DescribeClusterV2|系统规定参数。取值：DescribeClusterV2。

 |
|Id|String|是|C-E331B8AC12BF\*\*\*\*|集群ID。

 |
|RegionId|String|是|cn-hangzhou|地域ID。

 |
|AccessKeyId|String|否|LTAI8ljWyu7y\*\*\*\*|阿里云AccessKey ID信息，用于标识访问者身份。

 |

## 返回数据 {#resultMapping .section}

|名称|类型|示例值|描述|
|--|--|---|--|
|ClusterInfo| | |集群详情。

 |
|AccessInfo| | |集群连接信息。

 |
|ZKLinks| | |ZooKeeper连接信息。

 |
|ZKLink| | |ZooKeeper连接信息。

 |
|Link|String|emr-worker-1,emr-header-2,emr-header-1|ZooKeeper连接地址。

 |
|Port|String|2181|ZooKeeper连接端口。

 |
|AutoScalingAllowed|Boolean|false|允许弹性扩容。

 |
|AutoScalingByLoadAllowed|Boolean|true|是否允许按负载伸缩。

 |
|AutoScalingEnable|Boolean|false|是否开启弹性扩容。

 |
|AutoScalingSpotWithLimitAllowed|Boolean|false|是否允许使用弹性伸缩竞价实例。

 |
|BootstrapActionList| | |引导操作列表。

 |
|BootstrapAction| | |引导操作列表。

 |
|Arg|String|--a|引导操作的参数。

 |
|Name|String|action\_name|引导操作的名字。

 |
|Path|String|oss://bucket/path|引导操作脚本路径。

 |
|BootstrapFailed|Boolean|false|引导操作执行结果。

 |
|ChargeType|String|PostPaid|付费方式。

 |
|Configurations|String|\[\]|无需填写。

 |
|CoreNodeInService|Integer|3|使用中的Core节点数。

 |
|CoreNodeTotal|Integer|3|Core节点总数。

 |
|CreateResource|String|ECM\_EMR|集群标记，无需关注。

 |
|CreateType|String|MANUAL|集群创建的方式。

 |
|DepositType|String|HALF\_MANAGED|集群托管方式。

 |
|EasEnable|Boolean|true|是否高安全集群。

 |
|ExpiredTime|Long|1544076205000|包年包月集群的过期时间。

 |
|FailReason| | |集群创建失败的原因。

 |
|ErrorCode|String|InvalidImageId.NotFound|创建失败错误码。

 |
|ErrorMsg|String|The specified ImageId does not exist.|创建失败错误信息。

 |
|RequestId|String|B8DC3A91-3953-4444-92BB-DBC29C47EC1A|创建集群请求 ID

 |
|GatewayClusterIds|String|C-D7958B72E59B\*\*\*\*|包含的Gateway集群ID。

 |
|GatewayClusterInfoList| | |Gateway集群概况信息。

 |
|GatewayClusterInfo| | |Gateway集群概况信息。

 |
|ClusterId|String|C-D7958B72E59B\*\*\*\*|Gateway集群ID。

 |
|ClusterName|String|gateway-name|Gateway集群名。

 |
|Status|String|IDLE|Gateway集群状态。

 |
|HighAvailabilityEnable|Boolean|true|是否高可用集群。

 |
|HostGroupList| | |集群机器组列表。

 |
|HostGroup| | |集群机器组列表。

 |
|BandWidth|String|10|带宽。

 |
|ChargeType|String|PostPaid|付费类型。

 |
|CpuCore|Integer|16|CPU核心数。

 |
|DiskCapacity|Integer|120|数据盘容量。

 |
|DiskCount|Integer|4|数据盘数量。

 |
|DiskType|String|CLOUD\_SSD|数据盘磁盘类型。

 |
|HostGroupChangeStatus|String|IN\_PROGRESS|保留字段。升配任务的执行状态，枚举值：IN\_PROGRESS、COMPLETED和FAILED。

 |
|HostGroupChangeType|String|PostPaid|机器组当前的操作类型，枚举值：RESIZE\_DISK、MODIFY\_INSTANCE\_TYPE和RESTART\_HOST\_GROUP。

 |
|HostGroupId|String|G-9D08642FB8CE\*\*\*\*|机器组ID。

 |
|HostGroupName|String|主实例组|机器组名称。

 |
|HostGroupSubType|String|0|保留字段。

 |
|HostGroupType|String|CORE|机器组角色。

 |
|InstanceType|String|ecs.n4.2xlarge|机器组实例类型。

 |
|LockReason|String|0|保留字段。

 |
|LockType|String|0|保留字段。

 |
|MemoryCapacity|Integer|16|内存大小。

 |
|NodeCount|Integer|4|机器组节点数。

 |
|Nodes| | |机器节点。

 |
|Node| | |机器节点。

 |
|CreateTime|String|1543804242000|创建时间。

 |
|DaemonInfos| | |保留字段。

 |
|DaemonInfo| | |保留字段。

 |
|Name|String|0|保留字段。

 |
|DiskInfos| | |磁盘信息。

 |
|DiskInfo| | |磁盘信息。

 |
|Device|String|/dev/xvdb|磁盘名。

 |
|DiskId|String|d-bp15vg2nr3x2t0f37ko9|磁盘ID。

 |
|DiskName|String|disk1|磁盘名。

 |
|Size|Integer|80|磁盘容量（G）。

 |
|Type|String|data|磁盘类型。

 |
|EmrExpiredTime|String|2099-12-31T15:59Z|EMR超时时间。

 |
|ExpiredTime|String|2099-12-31T15:59Z|超时时间。

 |
|InnerIp|String|192.168.128.236|内网IP。

 |
|InstanceId|String|i-bp1ftve3lzvpm16hp7lo|ECS实例ID。

 |
|PubIp|String|47.99.\*\*\*.\*\*\*|公网IP。

 |
|Status|String|NORMAL|状态。

 |
|SupportIpV6|Boolean|false|是否支持IPV6。

 |
|ZoneId|String|cn-hangzhou-e|可用区。

 |
|Period|String|30|包年包月时间（天）。

 |
|HostPoolInfo| | |机器池信息。

 |
|HpBizId|String|id|机器池 ID。

 |
|HpName|String|name|机器池名称。

 |
|Id|String|C-E331B8AC12BF\*\*\*\*|集群ID。

 |
|ImageId|String|m-bp118knl07yk88y8s6cj|创建集群使用的镜像ID。

 |
|InstanceGeneration|String|ecs-3|ECS分代。

 |
|IoOptimized|Boolean|true|是否I/O优化。

 |
|LocalMetaDb|Boolean|true|是否使用Hive本地元数据库。

 |
|LogEnable|Boolean|true|是否打开集群OSS日志。

 |
|LogPath|String|oss://bucketname/path|集群OSS日志路径。

 |
|MachineType|String|ECS|集群的主机类型，目前默认为ECS。

 |
|MasterNodeInService|Integer|2|使用中的Master节点数。

 |
|MasterNodeTotal|Integer|2|Master节点总数。

 |
|MetaStoreType|String|local|统一元数据类型。

 |
|Name|String|cluster\_name|集群名。

 |
|NetType|String|vpc|集群网络类型。

 |
|Period|Integer|36|机器组的包年包月时间（包月数有1、2、3、4、5、6、7、8、9、12、24、36）。

 |
|RegionId|String|cn-hangzhou|地域ID。

 |
|RelateClusterId|String|C-D7958B72E59\*\*\*\*|针对Gateway， 关联的主集群ID。

 |
|RelateClusterInfo| | |针对Gateway, 关联的主集群信息。

 |
|ClusterId|String|C-D7958B72E59B\*\*\*\*|关联集群ID。

 |
|ClusterName|String|main|关联集群名称。

 |
|Status|String|SUCCESS|状态。

 |
|ResizeDiskEnable|Boolean|true|是否允许扩容磁盘。

 |
|RunningTime|Integer|1102|已经运行的时间（秒数）。

 |
|SecurityGroupId|String|sg-bp1bvompzxgx7q0\*\*\*\*|安全组ID。

 |
|SecurityGroupName|String|emr-default-securitygroup|安全组名。

 |
|ShowSoftwareInterface|Boolean|true|是否展示快捷链接页面。

 |
|SoftwareInfo| | |服务列表。

 |
|ClusterType|String|HADOOP|集群类型。

 |
|EmrVer|String|EMR-3.16.0|EMR版本号。

 |
|Softwares| | |服务列表。

 |
|Software| | |服务列表。

 |
|DisplayName|String|Hive|服务名称。

 |
|Name|String|HIVE|服务内部名称。

 |
|OnlyDisplay|Boolean|false|是否展现。

 |
|StartTpe|Integer|1|启动类型。

 |
|Version|String|2.3.3|服务版本。

 |
|StartTime|Long|1543804234000|集群启动时间（时间戳）。

 |
|Status|String|IDLE|集群状态。

 |
|StopTime|Long|1543804234000|集群停止时间。

 |
|TaskNodeInService|Integer|2|使用中的Task节点数。

 |
|TaskNodeTotal|Integer|2|Task节点总数。

 |
|UserDefinedEmrEcsRole|String|AliyunEmrEcsDefaultRole|使用的EMR权限名。

 |
|UserId|String|125046002175\*\*\*\*|用户ID。

 |
|VSwitchId|String|vsw-bp11qjbyggil4pow0\*\*\*\*|虚拟交换机ID。

 |
|VpcId|String|vpc-bp1l4urd87xlh7i4\*\*\*\*|VPC ID。

 |
|ZoneId|String|cn-hangzhou-b|可用区ID。

 |
|RequestId|String|14E9C045-9B8D-4D1E-8D23-FC0027B6D947|请求ID。

 |

## 示例 {#demo .section}

请求示例

``` {#request_demo}

http(s)://[Endpoint]/?Action=DescribeClusterV2
&Id=C-E331B8AC12BF****
&RegionId=cn-hangzhou
&<公共请求参数>

```

正常返回示例

`XML` 格式

``` {#xml_return_success_demo}
<DescribeClusterV2Response>
	  <data>
		    <ClusterInfo>
			      <AutoScalingAllowed>true</AutoScalingAllowed>
			      <AutoScalingEnable>false</AutoScalingEnable>
			      <AutoScalingSpotWithLimitAllowed>true</AutoScalingSpotWithLimitAllowed>
			      <BootstrapActionList></BootstrapActionList>
			      <BootstrapFailed>false</BootstrapFailed>
			      <ChargeType>PostPaid</ChargeType>
			      <Configurations></Configurations>
			      <CoreNodeInService>2</CoreNodeInService>
			      <CoreNodeTotal>2</CoreNodeTotal>
			      <CreateResource>ECM_EMR</CreateResource>
			      <CreateType>MANUAL</CreateType>
			      <EasEnable>true</EasEnable>
			      <GatewayClusterInfoList></GatewayClusterInfoList>
			      <HighAvailabilityEnable>true</HighAvailabilityEnable>
			      <HostGroupList>
				        <HostGroup>
					          <ChargeType>PostPaid</ChargeType>
					          <CpuCore>8</CpuCore>
					          <DiskCapacity>80</DiskCapacity>
					          <DiskCount>1</DiskCount>
					          <DiskType>CLOUD_SSD</DiskType>
					          <HostGroupId>G-79AA94922DFB****</HostGroupId>
					          <HostGroupName>主实例组</HostGroupName>
					          <HostGroupType>MASTER</HostGroupType>
					          <InstanceType>ecs.n4.2xlarge</InstanceType>
					          <MemoryCapacity>16</MemoryCapacity>
					          <NodeCount>2</NodeCount>
					          <Nodes>
						            <Node>
							              <CreateTime>1543804242000</CreateTime>
							              <DaemonInfos></DaemonInfos>
							              <DiskInfos>
								                <DiskInfo>
									                  <Device>/dev/xvdb</Device>
									                  <DiskId>d-bp15vg2nr3x2t0f37ko9</DiskId>
									                  <Size>80</Size>
									                  <Type>data</Type>
								                </DiskInfo>
								                <DiskInfo>
									                  <Device>/dev/xvda</Device>
									                  <DiskId>d-bp16c7qipsxtrq97mybg</DiskId>
									                  <Size>120</Size>
									                  <Type>system</Type>
								                </DiskInfo>
							              </DiskInfos>
							              <EmrExpiredTime>null</EmrExpiredTime>
							              <ExpiredTime>2099-12-31T15:59Z</ExpiredTime>
							              <InnerIp>192.168.*.*</InnerIp>
							              <InstanceId>i-bp16c7qipsxtrq99****</InstanceId>
							              <PubIp>47.97.*.*</PubIp>
							              <Status>NORMAL</Status>
							              <ZoneId>cn-hangzhou-b</ZoneId>
						            </Node>
						            <Node>
							              <CreateTime>1543804242000</CreateTime>
							              <DaemonInfos></DaemonInfos>
							              <DiskInfos>
								                <DiskInfo>
									                  <Device>/dev/xvdb</Device>
									                  <DiskId>d-bp1568nnv4ev81672qe6</DiskId>
									                  <Size>80</Size>
									                  <Type>data</Type>
								                </DiskInfo>
								                <DiskInfo>
									                  <Device>/dev/xvda</Device>
									                  <DiskId>d-bp16z8pr0y2vgfkghuba</DiskId>
									                  <Size>120</Size>
									                  <Type>system</Type>
								                </DiskInfo>
							              </DiskInfos>
							              <EmrExpiredTime>null</EmrExpiredTime>
							              <ExpiredTime>2099-12-31T15:59Z</ExpiredTime>
							              <InnerIp>192.168.*.*</InnerIp>
							              <InstanceId>i-bp1ftve3lzvpm16h****</InstanceId>
							              <PubIp>47.99.*.*</PubIp>
							              <Status>NORMAL</Status>
							              <ZoneId>cn-hangzhou-b</ZoneId>
						            </Node>
					          </Nodes>
				        </HostGroup>
				        <HostGroup>
					          <ChargeType>PostPaid</ChargeType>
					          <CpuCore>8</CpuCore>
					          <DiskCapacity>80</DiskCapacity>
					          <DiskCount>4</DiskCount>
					          <DiskType>CLOUD_SSD</DiskType>
					          <HostGroupId>G-9D08642FB8CE****</HostGroupId>
					          <HostGroupName>核心实例组</HostGroupName>
					          <HostGroupType>CORE</HostGroupType>
					          <InstanceType>ecs.n4.2xlarge</InstanceType>
					          <MemoryCapacity>16</MemoryCapacity>
					          <NodeCount>2</NodeCount>
					          <Nodes>
						            <Node>
							              <CreateTime>1543804244000</CreateTime>
							              <DaemonInfos></DaemonInfos>
							              <DiskInfos>
								                <DiskInfo>
									                  <Device>/dev/xvde</Device>
									                  <DiskId>d-bp1568nnv4ev81672qe5</DiskId>
									                  <Size>80</Size>
									                  <Type>data</Type>
								                </DiskInfo>
								                <DiskInfo>
									                  <Device>/dev/xvdd</Device>
									                  <DiskId>d-bp1967p3xp86y3wiudgq</DiskId>
									                  <Size>80</Size>
									                  <Type>data</Type>
								                </DiskInfo>
								                <DiskInfo>
									                  <Device>/dev/xvdc</Device>
									                  <DiskId>d-bp11qsh4zwrwup8jm9rr</DiskId>
									                  <Size>80</Size>
									                  <Type>data</Type>
								                </DiskInfo>
								                <DiskInfo>
									                  <Device>/dev/xvdb</Device>
									                  <DiskId>d-bp1d1juyl8z8aeuza9co</DiskId>
									                  <Size>80</Size>
									                  <Type>data</Type>
								                </DiskInfo>
								                <DiskInfo>
									                  <Device>/dev/xvda</Device>
									                  <DiskId>d-bp1ftve3lzvpm16i37y1</DiskId>
									                  <Size>80</Size>
									                  <Type>system</Type>
								                </DiskInfo>
							              </DiskInfos>
							              <EmrExpiredTime>null</EmrExpiredTime>
							              <ExpiredTime>2099-12-31T15:59Z</ExpiredTime>
							              <InnerIp>192.168.*.*</InnerIp>
							              <InstanceId>i-bp1gyhphkjplgljr****</InstanceId>
							              <PubIp></PubIp>
							              <Status>NORMAL</Status>
							              <ZoneId>cn-hangzhou-b</ZoneId>
						            </Node>
						            <Node>
							              <CreateTime>1543804247000</CreateTime>
							              <DaemonInfos></DaemonInfos>
							              <DiskInfos>
								                <DiskInfo>
									                  <Device>/dev/xvde</Device>
									                  <DiskId>d-bp12zhcmd96101qhz8ko</DiskId>
									                  <Size>80</Size>
									                  <Type>data</Type>
								                </DiskInfo>
								                <DiskInfo>
									                  <Device>/dev/xvdd</Device>
									                  <DiskId>d-bp13rttoh3l1gj84gcas</DiskId>
									                  <Size>80</Size>
									                  <Type>data</Type>
								                </DiskInfo>
								                <DiskInfo>
									                  <Device>/dev/xvdc</Device>
									                  <DiskId>d-bp1cnruigppe54k6fx18</DiskId>
									                  <Size>80</Size>
									                  <Type>data</Type>
								                </DiskInfo>
								                <DiskInfo>
									                  <Device>/dev/xvdb</Device>
									                  <DiskId>d-bp1967p3xp86y3wiudgr</DiskId>
									                  <Size>80</Size>
									                  <Type>data</Type>
								                </DiskInfo>
								                <DiskInfo>
									                  <Device>/dev/xvda</Device>
									                  <DiskId>d-bp1ik6bdp8fpsr8zsebj</DiskId>
									                  <Size>80</Size>
									                  <Type>system</Type>
								                </DiskInfo>
							              </DiskInfos>
							              <EmrExpiredTime>null</EmrExpiredTime>
							              <ExpiredTime>2099-12-31T15:59Z</ExpiredTime>
							              <InnerIp>192.168*.*</InnerIp>
							              <InstanceId>i-bp1ftve3lzvpm16h****</InstanceId>
							              <PubIp></PubIp>
							              <Status>NORMAL</Status>
							              <ZoneId>cn-hangzhou-b</ZoneId>
						            </Node>
					          </Nodes>
				        </HostGroup>
			      </HostGroupList>
			      <Id>C-E331B8AC12BF****</Id>
			      <ImageId>m-bp118knl07yk88y8s6cj</ImageId>
			      <IoOptimized>true</IoOptimized>
			      <LocalMetaDb>true</LocalMetaDb>
			      <MasterNodeInService>2</MasterNodeInService>
			      <MasterNodeTotal>0</MasterNodeTotal>
			      <Name>df-test-safe</Name>
			      <NetType>vpc</NetType>
			      <RegionId>cn-hangzhou</RegionId>
			      <ResizeDiskEnable>true</ResizeDiskEnable>
			      <RunningTime>1102</RunningTime>
			      <SecurityGroupId>sg-bp1bvompzxgx7q0l****</SecurityGroupId>
			      <SecurityGroupName>emr-default-securitygroup</SecurityGroupName>
			      <ShowSoftwareInterface>false</ShowSoftwareInterface>
			      <SoftwareInfo>
				        <ClusterType>HADOOP</ClusterType>
				        <EmrVer>EMR-3.16.0_pre</EmrVer>
				        <Softwares>
					          <Software>
						            <DisplayName>HDFS</DisplayName>
						            <Name>HDFS</Name>
						            <OnlyDisplay>false</OnlyDisplay>
						            <StartTpe>1</StartTpe>
						            <Version>2.7.2-1.3.2</Version>
					          </Software>
					          <Software>
						            <DisplayName>YARN</DisplayName>
						            <Name>YARN</Name>
						            <OnlyDisplay>false</OnlyDisplay>
						            <StartTpe>1</StartTpe>
						            <Version>2.7.2-1.3.2</Version>
					          </Software>
					          <Software>
						            <DisplayName>Hive</DisplayName>
						            <Name>HIVE</Name>
						            <OnlyDisplay>false</OnlyDisplay>
						            <StartTpe>1</StartTpe>
						            <Version>2.3.3-1.0.3</Version>
					          </Software>
					          <Software>
						            <DisplayName>Ganglia</DisplayName>
						            <Name>GANGLIA</Name>
						            <OnlyDisplay>false</OnlyDisplay>
						            <StartTpe>1</StartTpe>
						            <Version>3.7.2</Version>
					          </Software>
					          <Software>
						            <DisplayName>Zookeeper</DisplayName>
						            <Name>ZOOKEEPER</Name>
						            <OnlyDisplay>false</OnlyDisplay>
						            <StartTpe>1</StartTpe>
						            <Version>3.4.13</Version>
					          </Software>
					          <Software>
						            <DisplayName>Spark</DisplayName>
						            <Name>SPARK</Name>
						            <OnlyDisplay>false</OnlyDisplay>
						            <StartTpe>1</StartTpe>
						            <Version>2.3.2-1.0.1</Version>
					          </Software>
					          <Software>
						            <DisplayName>HBase</DisplayName>
						            <Name>HBASE</Name>
						            <OnlyDisplay>false</OnlyDisplay>
						            <StartTpe>1</StartTpe>
						            <Version>1.1.1-1.0.2</Version>
					          </Software>
					          <Software>
						            <DisplayName>HUE</DisplayName>
						            <Name>HUE</Name>
						            <OnlyDisplay>false</OnlyDisplay>
						            <StartTpe>1</StartTpe>
						            <Version>4.1.0</Version>
					          </Software>
					          <Software>
						            <DisplayName>Zeppelin</DisplayName>
						            <Name>ZEPPELIN</Name>
						            <OnlyDisplay>false</OnlyDisplay>
						            <StartTpe>1</StartTpe>
						            <Version>0.8.0</Version>
					          </Software>
					          <Software>
						            <DisplayName>Tez</DisplayName>
						            <Name>TEZ</Name>
						            <OnlyDisplay>false</OnlyDisplay>
						            <StartTpe>1</StartTpe>
						            <Version>0.9.1-1.0.2</Version>
					          </Software>
					          <Software>
						            <DisplayName>Presto</DisplayName>
						            <Name>PRESTO</Name>
						            <OnlyDisplay>false</OnlyDisplay>
						            <StartTpe>1</StartTpe>
						            <Version>0.208</Version>
					          </Software>
					          <Software>
						            <DisplayName>Sqoop</DisplayName>
						            <Name>SQOOP</Name>
						            <OnlyDisplay>false</OnlyDisplay>
						            <StartTpe>1</StartTpe>
						            <Version>1.4.7-1.0.0</Version>
					          </Software>
					          <Software>
						            <DisplayName>Pig</DisplayName>
						            <Name>PIG</Name>
						            <OnlyDisplay>false</OnlyDisplay>
						            <StartTpe>1</StartTpe>
						            <Version>0.14.0</Version>
					          </Software>
					          <Software>
						            <DisplayName>ApacheDS</DisplayName>
						            <Name>APACHEDS</Name>
						            <OnlyDisplay>false</OnlyDisplay>
						            <StartTpe>1</StartTpe>
						            <Version>2.0.0</Version>
					          </Software>
					          <Software>
						            <DisplayName>HAS</DisplayName>
						            <Name>HAS</Name>
						            <OnlyDisplay>false</OnlyDisplay>
						            <StartTpe>1</StartTpe>
						            <Version>1.1.1</Version>
					          </Software>
					          <Software>
						            <DisplayName>Knox</DisplayName>
						            <Name>KNOX</Name>
						            <OnlyDisplay>false</OnlyDisplay>
						            <StartTpe>1</StartTpe>
						            <Version>0.13.0-0.0.3</Version>
					          </Software>
				        </Softwares>
			      </SoftwareInfo>
			      <StartTime>1543804234000</StartTime>
			      <Status>IDLE</Status>
			      <TaskNodeInService>0</TaskNodeInService>
			      <TaskNodeTotal>0</TaskNodeTotal>
			      <UserDefinedEmrEcsRole>AliyunEmrEcsDefaultRole</UserDefinedEmrEcsRole>
			      <UserId>125046002175****</UserId>
			      <VSwitchId>vsw-bp11qjbyggil4pow0****</VSwitchId>
			      <VpcId>vpc-bp1l4urd87xlh7i4b****</VpcId>
			      <ZoneId>cn-hangzhou-b</ZoneId>
		    </ClusterInfo>
		    <RequestId>14E9C045-9B8D-4D1E-8D23-FC0027B6D947</RequestId>
	  </data>
	  <requestId>14E9C045-9B8D-4D1E-8D23-FC0027B6D947</requestId>
</DescribeClusterV2Response>
```

`JSON` 格式

``` {#json_return_success_demo}
{
	"requestId":"14E9C045-9B8D-4D1E-8D23-FC0027B6D947",
	"data":{
		"ClusterInfo":{
			"SecurityGroupId":"sg-bp1bvompzxgx7q0l****",
			"ImageId":"m-bp118knl07yk88y8s6cj",
			"MasterNodeInService":2,
			"HostGroupList":{
				"HostGroup":[
					{
						"ChargeType":"PostPaid",
						"NodeCount":2,
						"DiskCount":1,
						"HostGroupType":"MASTER",
						"HostGroupName":"主实例组",
						"MemoryCapacity":16,
						"Nodes":{
							"Node":[
								{
									"Status":"NORMAL",
									"InnerIp":"192.168.*.*",
									"EmrExpiredTime":"null",
									"InstanceId":"i-bp16c7qipsxtrq99****",
									"CreateTime":"1543804242000",
									"ZoneId":"cn-hangzhou-b",
									"PubIp":"47.97.*.*",
									"DaemonInfos":{
										"DaemonInfo":[]
									},
									"ExpiredTime":"2099-12-31T15:59Z",
									"DiskInfos":{
										"DiskInfo":[
											{
												"Device":"/dev/xvdb",
												"Type":"data",
												"DiskId":"d-bp15vg2nr3x2t0f37ko9",
												"Size":80
											},
											{
												"Device":"/dev/xvda",
												"Type":"system",
												"DiskId":"d-bp16c7qipsxtrq97mybg",
												"Size":120
											}
										]
									}
								},
								{
									"Status":"NORMAL",
									"InnerIp":"192.168.*.*",
									"EmrExpiredTime":"null",
									"InstanceId":"i-bp1ftve3lzvpm16h****",
									"CreateTime":"1543804242000",
									"ZoneId":"cn-hangzhou-b",
									"PubIp":"47.99.*.*",
									"DaemonInfos":{
										"DaemonInfo":[]
									},
									"ExpiredTime":"2099-12-31T15:59Z",
									"DiskInfos":{
										"DiskInfo":[
											{
												"Device":"/dev/xvdb",
												"Type":"data",
												"DiskId":"d-bp1568nnv4ev81672qe6",
												"Size":80
											},
											{
												"Device":"/dev/xvda",
												"Type":"system",
												"DiskId":"d-bp16z8pr0y2vgfkghuba",
												"Size":120
											}
										]
									}
								}
							]
						},
						"DiskCapacity":80,
						"CpuCore":8,
						"HostGroupId":"G-79AA94922DFB****",
						"InstanceType":"ecs.n4.2xlarge",
						"DiskType":"CLOUD_SSD"
					},
					{
						"ChargeType":"PostPaid",
						"NodeCount":2,
						"DiskCount":4,
						"HostGroupType":"CORE",
						"HostGroupName":"核心实例组",
						"MemoryCapacity":16,
						"Nodes":{
							"Node":[
								{
									"Status":"NORMAL",
									"InnerIp":"192.168.*.*",
									"EmrExpiredTime":"null",
									"InstanceId":"i-bp1gyhphkjplgljr****",
									"CreateTime":"1543804244000",
									"ZoneId":"cn-hangzhou-b",
									"PubIp":"",
									"DaemonInfos":{
										"DaemonInfo":[]
									},
									"ExpiredTime":"2099-12-31T15:59Z",
									"DiskInfos":{
										"DiskInfo":[
											{
												"Device":"/dev/xvde",
												"Type":"data",
												"DiskId":"d-bp1568nnv4ev81672qe5",
												"Size":80
											},
											{
												"Device":"/dev/xvdd",
												"Type":"data",
												"DiskId":"d-bp1967p3xp86y3wiudgq",
												"Size":80
											},
											{
												"Device":"/dev/xvdc",
												"Type":"data",
												"DiskId":"d-bp11qsh4zwrwup8jm9rr",
												"Size":80
											},
											{
												"Device":"/dev/xvdb",
												"Type":"data",
												"DiskId":"d-bp1d1juyl8z8aeuza9co",
												"Size":80
											},
											{
												"Device":"/dev/xvda",
												"Type":"system",
												"DiskId":"d-bp1ftve3lzvpm16i37y1",
												"Size":80
											}
										]
									}
								},
								{
									"Status":"NORMAL",
									"InnerIp":"192.168*.*",
									"EmrExpiredTime":"null",
									"InstanceId":"i-bp1ftve3lzvpm16h****",
									"CreateTime":"1543804247000",
									"ZoneId":"cn-hangzhou-b",
									"PubIp":"",
									"DaemonInfos":{
										"DaemonInfo":[]
									},
									"ExpiredTime":"2099-12-31T15:59Z",
									"DiskInfos":{
										"DiskInfo":[
											{
												"Device":"/dev/xvde",
												"Type":"data",
												"DiskId":"d-bp12zhcmd96101qhz8ko",
												"Size":80
											},
											{
												"Device":"/dev/xvdd",
												"Type":"data",
												"DiskId":"d-bp13rttoh3l1gj84gcas",
												"Size":80
											},
											{
												"Device":"/dev/xvdc",
												"Type":"data",
												"DiskId":"d-bp1cnruigppe54k6fx18",
												"Size":80
											},
											{
												"Device":"/dev/xvdb",
												"Type":"data",
												"DiskId":"d-bp1967p3xp86y3wiudgr",
												"Size":80
											},
											{
												"Device":"/dev/xvda",
												"Type":"system",
												"DiskId":"d-bp1ik6bdp8fpsr8zsebj",
												"Size":80
											}
										]
									}
								}
							]
						},
						"DiskCapacity":80,
						"CpuCore":8,
						"HostGroupId":"G-9D08642FB8CE****",
						"InstanceType":"ecs.n4.2xlarge",
						"DiskType":"CLOUD_SSD"
					}
				]
			},
			"SoftwareInfo":{
				"Softwares":{
					"Software":[
						{
							"Name":"HDFS",
							"OnlyDisplay":false,
							"Version":"2.7.2-1.3.2",
							"DisplayName":"HDFS",
							"StartTpe":1
						},
						{
							"Name":"YARN",
							"OnlyDisplay":false,
							"Version":"2.7.2-1.3.2",
							"DisplayName":"YARN",
							"StartTpe":1
						},
						{
							"Name":"HIVE",
							"OnlyDisplay":false,
							"Version":"2.3.3-1.0.3",
							"DisplayName":"Hive",
							"StartTpe":1
						},
						{
							"Name":"GANGLIA",
							"OnlyDisplay":false,
							"Version":"3.7.2",
							"DisplayName":"Ganglia",
							"StartTpe":1
						},
						{
							"Name":"ZOOKEEPER",
							"OnlyDisplay":false,
							"Version":"3.4.13",
							"DisplayName":"Zookeeper",
							"StartTpe":1
						},
						{
							"Name":"SPARK",
							"OnlyDisplay":false,
							"Version":"2.3.2-1.0.1",
							"DisplayName":"Spark",
							"StartTpe":1
						},
						{
							"Name":"HBASE",
							"OnlyDisplay":false,
							"Version":"1.1.1-1.0.2",
							"DisplayName":"HBase",
							"StartTpe":1
						},
						{
							"Name":"HUE",
							"OnlyDisplay":false,
							"Version":"4.1.0",
							"DisplayName":"HUE",
							"StartTpe":1
						},
						{
							"Name":"ZEPPELIN",
							"OnlyDisplay":false,
							"Version":"0.8.0",
							"DisplayName":"Zeppelin",
							"StartTpe":1
						},
						{
							"Name":"TEZ",
							"OnlyDisplay":false,
							"Version":"0.9.1-1.0.2",
							"DisplayName":"Tez",
							"StartTpe":1
						},
						{
							"Name":"PRESTO",
							"OnlyDisplay":false,
							"Version":"0.208",
							"DisplayName":"Presto",
							"StartTpe":1
						},
						{
							"Name":"SQOOP",
							"OnlyDisplay":false,
							"Version":"1.4.7-1.0.0",
							"DisplayName":"Sqoop",
							"StartTpe":1
						},
						{
							"Name":"PIG",
							"OnlyDisplay":false,
							"Version":"0.14.0",
							"DisplayName":"Pig",
							"StartTpe":1
						},
						{
							"Name":"APACHEDS",
							"OnlyDisplay":false,
							"Version":"2.0.0",
							"DisplayName":"ApacheDS",
							"StartTpe":1
						},
						{
							"Name":"HAS",
							"OnlyDisplay":false,
							"Version":"1.1.1",
							"DisplayName":"HAS",
							"StartTpe":1
						},
						{
							"Name":"KNOX",
							"OnlyDisplay":false,
							"Version":"0.13.0-0.0.3",
							"DisplayName":"Knox",
							"StartTpe":1
						}
					]
				},
				"EmrVer":"EMR-3.16.0_pre",
				"ClusterType":"HADOOP"
			},
			"CoreNodeInService":2,
			"ZoneId":"cn-hangzhou-b",
			"CreateType":"MANUAL",
			"VSwitchId":"vsw-bp11qjbyggil4pow0****",
			"VpcId":"vpc-bp1l4urd87xlh7i4b****",
			"AutoScalingSpotWithLimitAllowed":true,
			"TaskNodeTotal":0,
			"IoOptimized":true,
			"TaskNodeInService":0,
			"BootstrapFailed":false,
			"CoreNodeTotal":2,
			"StartTime":1543804234000,
			"MasterNodeTotal":0,
			"ChargeType":"PostPaid",
			"Configurations":"",
			"AutoScalingAllowed":true,
			"UserId":"125046002175****",
			"ShowSoftwareInterface":false,
			"EasEnable":true,
			"AutoScalingEnable":false,
			"Name":"df-test-safe",
			"Status":"IDLE",
			"ResizeDiskEnable":true,
			"SecurityGroupName":"emr-default-securitygroup",
			"HighAvailabilityEnable":true,
			"LocalMetaDb":true,
			"BootstrapActionList":{
				"BootstrapAction":[]
			},
			"CreateResource":"ECM_EMR",
			"RegionId":"cn-hangzhou",
			"RunningTime":1102,
			"GatewayClusterInfoList":{
				"GatewayClusterInfo":[]
			},
			"Id":"C-E331B8AC12BF****",
			"NetType":"vpc",
			"UserDefinedEmrEcsRole":"AliyunEmrEcsDefaultRole"
		},
		"RequestId":"14E9C045-9B8D-4D1E-8D23-FC0027B6D947"
	}
}
```

## 错误码 { .section}

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|403|Params.Illegal|The specified parameters are wrongly formed..|指定参数格式错误|
|404|ClusterId.NotFound|ClusterId \[%s\] does not exist.|Cluster Id不存在，确认集群的Cluster Id|
|403|User.OtherUserResource.NotAllow|It is not allowed to operate other user's resource|不能操作其它用户的资源|
|500|InternalError|The request processing has failed due to some unknown error.|内部错误，请提工单|

访问[错误中心](https://error-center.alibabacloud.com/status/product/Emr)查看更多错误码。

