# CreateClusterV2 {#doc_api_Emr_CreateClusterV2 .reference}

调用 CreateClusterV2 接口，创建一个 E-MapReduce 集群。

## 调试 {#api_explorer .section}

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=CreateClusterV2&type=RPC&version=2016-04-08)

## 请求参数 {#parameters .section}

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|CreateClusterV2|系统规定参数。取值：CreateClusterV2。

 |
|ClusterType|String|是|HADOOP|集群类型。

 |
|EmrVer|String|是|EMR-3.15.0|EMR版本。

 |
|Name|String|是|bi\_hadoop|集群的名字。长度限制为 1-64 个字符，只允许包含中文、字母、数字、-、\_。

 |
|RegionId|String|是|cn-hangzhou|地域ID。目前支持华东 1、华东 2、华南 1、华北 2、华北 3、美西、新加坡、德国。

 |
|ZoneId|String|是|cn-hangzhou-e|可用区ID。

 -   华东 1（杭州）支持：cn-hangzhou-b。
-   华北 2（北京）支持：cn-beijing-a、cn-beijing-b,cn-beijing-c。

 |
|BootstrapAction.N.Name|String|否|name|引导操作名字。

 |
|BootstrapAction.N.Path|String|否|oss://bucket/path|引导操作脚本路径。

 |
|Config.N.ConfigKey|String|否|fs.trash.interval|自定义配置项的Key。

 |
|Config.N.ConfigValue|String|否|60|自定义配置项的值。

 |
|Config.N.FileName|String|否|yarn-site|自定义配置项所属文件名。

 |
|Config.N.ServiceName|String|否|YARN|自定义配置项服务名（大写）。

 |
|HostGroup.N.HostGroupType|String|否|MASTER|机器组类型，枚举值：

 -   MASTER
-   CORE
-   TASK

 目前**MASTER**和**CORE**均只支持设置一个组。

 |
|HostGroup.N.InstanceType|String|否|ecs.mn4.2xlarge|机器组型号。

 |
|HostGroup.N.NodeCount|Integer|否|2|机器组节点数。

 |
|UserInfo.N.Password|String|否|pwd|集群密码。

 |
|UserInfo.N.UserId|String|否|12345|Knox账号的Aliyun用户ID。

 |
|UserInfo.N.UserName|String|否|tom|Knox账号用户名。

 |
|AccessKeyId|String|否|LTAI8ljWyu7y\*\*\*\*|阿里云AccessKey ID信息，用于标识访问者身份。

 |
|LogPath|String|否|oss//bucketname/path|OSS日志路径。

 |
|SecurityGroupId|String|否|sg-bp1id7ajv83kmqwq\*\*\*\*|安全组 ID。可以在ECS中创建一个然后使用。需要确认的是，如果使用已有的安全组，会被增加上默认安全组策略：入只开放22端口，出开放所有端口。

 |
|IsOpenPublicIp|Boolean|否|true|是否开启公网IP。如果开启，默认会带有8MB的带宽。

 |
|SecurityGroupName|String|否|emr-sg|需要新建的安全组名称。如果不指定安全组ID，那么将使用这个名字创建一个新的安全组。当集群创建完成以后，可以在集群详情中看到创建的安全组ID。这个安全组将会有带有默认的安全组策略：入只开放22端口，出开放所有端口。

 |
|ChargeType|String|否|PostPaid|付费类型：

 -   PostPaid：按量付费。
-   PrePaid：包年包月。

 |
|Period|Integer|否|30|包年包月时间（包月数有：1、2、3、4、5、6、7、8、9、12、24、36）。ChargeType=PrePaid 时，必填。

 |
|AutoRenew|Boolean|否|false|包年包月集群是否自动续费。

 |
|AutoPayOrder|Boolean|否|true|是否自动付费。

 |
|VpcId|String|否|vpc-bp1l4urd87xlh7i4b\*\*\*\*|VPC ID，NetType=vpc时必填。

 |
|VSwitchId|String|否|vsw-bp10tvjyc77psy0z5\*\*\*\*|交换机ID，NetType=vpc时必填。

 |
|NetType|String|否|vpc|网络类型。

 |
|UserDefinedEmrEcsRole|String|否|AliyunEmrEcsDefaultRole|用于ECS调用的EMR权限名。

 |
|OptionSoftWareList.N|RepeatList|否|\["ZOOKEEPER","LIVY"\]|可选软件列表。

 |
|HighAvailabilityEnable|Boolean|否|true|是否开启高可用集群。如果开启高可用，需要两台Master节点。

 |
|UseLocalMetaDb|Boolean|否|true|是否使用本地Hive元数据库。

 |
|IoOptimized|Boolean|否|true|是否开启I/O优化，默认**true**。

 |
|SshEnable|Boolean|否|true|是否开启SSH。

 |
|InstanceGeneration|String|否|ecs-3|ECS实例分代。

 |
|MasterPwd|String|否|pwd|Master节点SSH访问密码。需要满足ECS的密码规则：8

 -   30 个字符，且同时包含任意三项（大、小写字母、数字和特殊符号）。

 |
|KeyPairName|String|否|test\_pair|密钥对。

 |
|HostComponentInfo.N.HostName|String|否|name1|主机名。

 |
|HostComponentInfo.N.ServiceName|String|否|name2|服务名。

 |
|HostComponentInfo.N.ComponentNameList.N|RepeatList|否|5|组件列表。

 |
|MachineType|String|否|ECS|机器类型。

 |
|DepositType|String|否|HALF\_MANAGED|托管类型。

 |
|HostGroup.N.ClusterId|String|否|0|保留字段，无需填写。

 |
|HostGroup.N.HostGroupId|String|否|0|保留字段。

 |
|HostGroup.N.HostGroupName|String|否|主实例组|机器组名字。

 |
|HostGroup.N.Comment|String|否|0|保留字段，无需填写。

 |
|HostGroup.N.CreateType|String|否|0|保留字段，无需填写。

 |
|HostGroup.N.ChargeType|String|否|PostPaid|机器组机器付费类型。

 |
|HostGroup.N.Period|Integer|否|30|包年包月时间（包月数有1、2、3、4、5、6、7、8、9、12、24、36）。HostGroup.n.ChargeType=PrePaid时，必填。

 |
|HostGroup.N.DiskType|String|否|CLOUD\_SSD|机器组的数据盘类型。

 |
|HostGroup.N.DiskCapacity|Integer|否|80|机器组的数据盘容量。

 |
|HostGroup.N.DiskCount|Integer|否|4|机器组的数据盘数量。

 |
|HostGroup.N.SysDiskType|String|否|CLOUD\_SSD|机器组的系统盘类型。

 |
|HostGroup.N.SysDiskCapacity|Integer|否|80|机器组的系统盘容量。

 |
|HostGroup.N.AutoRenew|Boolean|否|false|机器组机器是否自动续费。

 |
|HostGroup.N.VSwitchId|String|否|vsw-bp10tvjyc77psy0z5\*\*\*\*|虚拟交换机ID。

 |
|HostGroup.N.GpuDriver|String|否|cuda9|GPU驱动。

 |
|BootstrapAction.N.Arg|String|否|--a=b|引导操作参数。

 |
|UseCustomHiveMetaDB|Boolean|否|false|保留字段，无需填写。

 |
|InitCustomHiveMetaDB|Boolean|否|false|保留字段，无需填写。

 |
|Config.N.Encrypt|String|否|0|保留字段。

 |
|Config.N.Replace|String|否|0| 

 保留字段。

 |
|Configurations|String|否|0|无需填写。

 |
|EasEnable|Boolean|否|false|是否高安全集群。

 |
|RelatedClusterId|String|否|C-D7958B72E59B\*\*\*\*|当前集群是gateway时，其关联的主集群ID。

 |
|WhiteListType|String|否|""|暂时无需填写。

 |
|AuthorizeContent|String|否|0|暂时无需填写。

 |
|MetaStoreConf|String|否|\{"dbUrl":"jdbc:mysql://rm-xxxxx.mysql.rds.aliyuncs.com:3306/hivemeta3?createDatabaseIfNotExist=true&characterEncoding=UTF-8","dbUserName":"user","dbPassword":"password"\}|一个JSON字段，包含dbUrl, dbUserName, dbPassword分别代表RDS的连接串、用户名和密码。dbUrl 中要带上库名

 |
|MetaStoreType|String|否|local|可选值：local, unified, user\_rds 分别代表集群内部元数据、统一元数据和用户自建RDS作为元数据。

 |

## 返回数据 {#resultMapping .section}

|名称|类型|示例值|描述|
|--|--|---|--|
|ClusterId|String|C-D7958B72E59B\*\*\*\*|集群ID。

 |
|CoreOrderId|String|0|Core节点订单ID。

 |
|EmrOrderId|String|0|EMR订单ID。

 |
|MasterOrderId|String|0|Master节点订单ID。

 |
|RequestId|String|BF4FBAC6-B03E-4BFB-B6DB-EB53C34F2E22|请求ID。

 |

## 示例 {#demo .section}

请求示例

``` {#request_demo}

http(s)://[Endpoint]/?Action=CreateClusterV2
&BootstrapAction.1.1ame=name
&BootstrapAction.1.Path=oss://bucket/path
&ClusterType=HADOOP
&Config.1.ConfigKey=fs.trash.interval
&Config.1.ConfigValue=60
&Config.1.FileName=yarn-site
&Config.1.ServiceName=YARN
&EmrVer=EMR-3.15.0
&HostGroup.1.HostGroupType=MASTER
&HostGroup.1.InstanceType=ecs.mn4.2xlarge
&HostGroup.1.1odeCount=2
&Name=bi_hadoop
&RegionId=cn-hangzhou
&UserInfo.1.Password=pwd
&UserInfo.1.UserId=12345
&UserInfo.1.UserName=tom
&<公共请求参数>

```

正常返回示例

`XML` 格式

``` {#xml_return_success_demo}
<CreateClusterV2Response>
	  <ClusterId>C-4DE6DA872B0E****</ClusterId>
	  <RequestId>F4DE89FB-7054-475C-B7E2-B9A38152DA7E</RequestId>
</CreateClusterV2Response>
```

`JSON` 格式

``` {#json_return_success_demo}
{
	"ClusterId":"C-4DE6DA872B0E****",
	"RequestId":"F4DE89FB-7054-475C-B7E2-B9A38152DA7E"
}
```

## 错误码 { .section}

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|403|Forbbiden|User not authorized to operate on the specified resource.|没有权限操作指定资源，联系主账号授权|
|400|ECSInfo.DiskSize.TooSmall|disk size per ecs should be \>= 80GB.|磁盘容量太小，加大磁盘容量|
|403|User.OtherUserResource.NotAllow|It is not allowed to operate other user's resource|不能操作其它用户的资源|
|403|User.Account.Abnormal|The User Account maybe is out of service!|用户帐号已经停止服务|
|403|Master.Pwd.Cannot.Blank|Master password can not be blank when enable password!|Master节点的密码不能为空，填写Master的密码|
|403|LogPath.Cannot.Blank|Log path can not be blank when enablbe log!|日志路径不能为空，请填写正确的参数|
|403|EMR.Version.Not.Exist|Specified emr version \[%s\] does not exist in region \[%s\]!|指定EMR版本不存在，选择正确的EMR版本|
|400|HighAvailability.Master.NodeCount.Not.Match|HighAvailability parameter does not match the master's node count|高可用参数与master数量不匹配，HA集群需要master节点个数为2|
|400|HighAvailability.is.not.permitted.in.this.emr.version|HighAvailability parameter is not permitted in this emr version|该EMR版本不支持HA集群，切换EMR版本|
|400|InvalidParameter.Period|Invalid parameter 'period'.|包年包月类型的period参数不合规范|
|400|Balance.Not.Enough|Account balance is not enough!|帐号没有足够的余额，账户至少有100元人民币余额|
|403|VSwitch.NotBelongTo.Zone|VSwitchid should belong to the ZoneId!|指定交换机不属于该可用区|
|400|InsufficientBalance|Your account does not have enough balance|帐号没有足够的余额，帐号至少有100元余额|
|400|Create.PrePaid.Cluster.Failed|Create prepaid cluster order failed:\[%s\]|创建包年包月集群订单失败|
|400|ECSInfo.ECSOrder.INVALID|invalid parameter format\(ecsorderinfo\)|创建订单参数错误|
|400|Ecs.InstanceType.NotSupported|Unsupported ecs instance type \[%s\] at zone \[%s\] with IO-optimized \[%s\] and network type \[%s\].|实例规格不支持，选择其它实例规格|
|400|DiskType.Invalid|Unsupported disk type \[%s\] at zone \[%s\] with IO-optimized \[%s\] and network type \[%s\].|磁盘类型不支持，更换磁盘类型|
|400|Unsupported.DiskType|Ecs instance type \[%s\] does not support disk type \[%\]|磁盘类型不支持|
|400|Unsupported.ZoneId|Zone \[%s\] is invalid or not supported in emr|EMR不支持该可用区，切换可用区|
|400|ECSInfo.DiskSize.TooBig|Disk size exceeded max value limit.|磁盘容量超过磁盘限制，减少磁盘容量|
|400|ECSInfo.DiskCount.ExceedLimit|Disk count exceeded max value limit.|磁盘块数超过限制，减少磁盘块数|
|400|ECSInfo.NodeType.Unsupported|the specify node type is unsupport.|指定节点类型不支持，切换节点类型|
|400|Must.Specify.MasterNode|master node is mandatory.|请指定Master节点信息|
|400|Only.Support.One.Master|Only one master node is supported in emr cluster|在EMR集群中只支持一个主节点|
|400|Have.Orders.Wait.For.Pay|Have other orders wait for pay|有另外的待付款订单|
|400|Unsupported.IoOptimization.Option|IO-optimization option \[%s\] is not supported at zone \[%s\] and network type \[%s\].|该可用区中的该网络类型不支持IO优化机型|
|400|Unsupported.EcsInstanceGeneration|Unsupported ecs instance generation \[%s\] at zone \[%s\] with IO-optimized \[%s\] and network type \[%s\].|该可用区中的该网络类型不支持IO优化机型|
|400|AuthRealNameNotPass|User real name authenticate failed!|帐号没有经过实名认证，进行实名认证|
|403|EMR.Version.OptionSoftWare.UnSupported|only emr version \>= 2.0.0 support optionsoftware.|不能选择可选的软件，只有EMR2.0.0以上版本可以装可选软件|
|403|Check.Account.Failed|Verify account's registration info failed:\[%s\].|验证帐号失败，帐号信息不完整，请补全帐号信息|
|500|InternalError|The request processing has failed due to some unknown error.|内部错误，请提工单|

访问[错误中心](https://error-center.alibabacloud.com/status/product/Emr)查看更多错误码。

