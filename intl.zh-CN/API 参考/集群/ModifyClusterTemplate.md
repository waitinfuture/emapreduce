# ModifyClusterTemplate {#doc_api_Emr_ModifyClusterTemplate .reference}

调用 ModifyClusterTemplate 接口，修改集群模版。

## 调试 {#api_explorer .section}

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=ModifyClusterTemplate&type=RPC&version=2016-04-08)

## 请求参数 {#parameters .section}

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ModifyClusterTemplate|系统规定参数。取值：ModifyClusterTemplate。

 |
|BizId|String|是|CT-4A6799A79D73\*\*\*\*|集群模版ID。

 |
|ClusterType|String|是|HADOOP|集群类型。

 |
|EmrVer|String|是|EMR-3.15.0|EMR版本。

 |
|RegionId|String|是|cn-hangzhou|区域ID。

 |
|TemplateName|String|是|new\_template\_name|集群模版名。

 |
|ZoneId|String|是|cn-hangzhou-b|可用区ID。

 |
|AccessKeyId|String|否|LTAI8ljWyu7y\*\*\*\*|阿里云AccessKey ID信息，用于标识访问者身份。

 |
|AutoRenew|Boolean|否|false|包年包月集群是否自动续费。

 |
|BootstrapAction.N.Arg|String|否|--a|引导操作参数。

 |
|BootstrapAction.N.Name|String|否|action\_name|引导操作名。

 |
|BootstrapAction.N.Path|String|否|oss://bucket/path|引导操作脚本路径。

 |
|ChargeType|String|否|PostPaid|付费类型。

 |
|Config.N.ConfigKey|String|否|fs.trash.interval|自定义配置项的Key。

 |
|Config.N.ConfigValue|String|否|60|自定义配置项的值。

 |
|Config.N.Encrypt|String|否|0|保留字段，无需设置。

 |
|Config.N.FileName|String|否|yarn-site|自定义配置项所属文件名。

 |
|Config.N.Replace|String|否|0|保留字段，无需设置。

 |
|Config.N.ServiceName|String|否|YARN|自定义配置项服务名（大写）。

 |
|Configurations|String|否|\[\{"classification": "core-site","properties": \{"fs.trash.interval": "61"\}\},\{"classification": "hadoop-log4j","properties": \{"hadoop.log.file": "hadoop1.log","hadoop.root.logger": "INFO","a.b.c": "ABC"\}\}\]|软件自定义配置。（集群启动前，可以指定一个json文件修改软件配置。）

 |
|DepositType|String|否|HALF\_MANAGED|托管类型。

 |
|EasEnable|Boolean|否|true|是否高安全集群。

 |
|HighAvailabilityEnable|Boolean|否|true|是否高可用集群。

 |
|HostGroup.N.AutoRenew|Boolean|否|true|机器组机器是否自动续费。

 |
|HostGroup.N.ChargeType|String|否|PostPaid|付费方式。

 |
|HostGroup.N.ClusterId|String|否|0|保留字段。

 |
|HostGroup.N.Comment|String|否|comment|保留字段。

 |
|HostGroup.N.CreateType|String|否|ON\_DEMAND|保留字段。

 |
|HostGroup.N.DiskCapacity|Integer|否|80|机器组的数据盘容量。

 |
|HostGroup.N.DiskCount|Integer|否|4|机器组的数据盘数量。

 |
|HostGroup.N.DiskType|String|否|CLOUD\_SSD|机器组的数据盘类型。

 |
|HostGroup.N.HostGroupId|String|否|0|保留字段。

 |
|HostGroup.N.HostGroupName|String|否|主实例组|机器组名字。

 |
|HostGroup.N.HostGroupType|String|否|MASTER|机器组类型。

 |
|HostGroup.N.InstanceType|String|否|ecs.mn4.2xlarge|机器组型号。

 |
|HostGroup.N.MultiInstanceTypes|String|否|ecs.sn1.xlarge,ecs.sn2.xlarge|多规格机器型号列表，逗号隔开。

 |
|HostGroup.N.NodeCount|Integer|否|4|机器组节点数。

 |
|HostGroup.N.Period|Integer|否|36|机器组的包年包月时间（包月数有1、2、3、4、5、6、7、8、9、12、24、36）。

 |
|HostGroup.N.SysDiskCapacity|Integer|否|80|机器组的系统盘容量。

 |
|HostGroup.N.SysDiskType|String|否|CLOUD\_SSD|机器组的系统盘类型。

 |
|HostGroup.N.VSwitchId|String|否|vsw-bp10tvjyc77psy0z5\*\*\*\*|交换机ID。

 |
|InitCustomHiveMetaDb|Boolean|否|false|保留字段，无需填写。

 |
|InstanceGeneration|String|否|ecs-3|ECS实例分代。

 |
|IoOptimized|Boolean|否|true|是否I/O优化。

 |
|IsOpenPublicIp|Boolean|否|true|是否使用公网IP。

 |
|KeyPairName|String|否|yourKeyPair\*\*\*\*|密钥对。

 |
|LogPath|String|否|oss//bucketname/path|OSS日志路径。

 |
|MachineType|String|否|ECS|机器类型。

 |
|MasterPwd|String|否|pwd|Master节点SSH访问密码。

 |
|MetaStoreConf|String|否|\{"dbUrl":"jdbc:mysql://yourhost:3306/instance","dbUserName":"db1","dbPassword":"pwd"\}|MetaStoreType设置为user\_rds时有效。元数据RDS的设置。

 |
|MetaStoreType|String|否|local|local, unified, user\_rds 分别代表集群默认元数据，E-MapReduce统一元数据，用户自定义RDS作为元数据。

 |
|NetType|String|否|vpc|网络类型。

 |
|OptionSoftWareList.N|RepeatList|否|\["ZOOKEEPER","LIVY"\]|可选软件列表。

 |
|Period|Integer|否|36|机器组的包年包月时间（包月数有1、2、3、4、5、6、7、8、9、12、24、36）。

 |
|SecurityGroupId|String|否|sg-bp1id7ajv83kmqwq\*\*\*\*|安全组ID。

 |
|SecurityGroupName|String|否|emr\_sg|安全组名字。

 |
|SshEnable|Boolean|否|true|是否打开SSH访问。

 |
|UseCustomHiveMetaDb|Boolean|否|false|保留字段，无需填写。

 |
|UseLocalMetaDb|Boolean|否|true|是否使用本地Hive元数据库。

 |
|UserDefinedEmrEcsRole|String|否|AliyunEmrEcsDefaultRole|用于ECS调用的EMR权限名。

 |
|VSwitchId|String|否|vsw-bp10tvjyc77psy0z5\*\*\*\*|交换机ID。

 |
|VpcId|String|否|vpc-bp1l4urd87xlh7i4b\*\*\*\*|VPC ID。

 |

## 返回数据 {#resultMapping .section}

|名称|类型|示例值|描述|
|--|--|---|--|
|ClusterTemplateId|String|CT-4A6799A79D73\*\*\*\*|集群模版ID。

 |
|RequestId|String|BF4FBAC6-B03E-4BFB-B6DB-EB53C34F2E22|请求ID。

 |

## 示例 {#demo .section}

请求示例

``` {#request_demo}

http(s)://[Endpoint]/?Action=ModifyClusterTemplate
&BizId=CT-4A6799A79D73****
&BootstrapAction.1.1ame=action_name
&BootstrapAction.1.Path=oss://bucket/path
&ClusterType=HADOOP
&Config.1.ConfigKey=fs.trash.interval
&Config.1.ConfigValue=60
&Config.1.FileName=yarn-site
&Config.1.ServiceName=YARN
&EmrVer=EMR-3.15.0
&HostGroup.1.HostGroupType=MASTER
&HostGroup.1.InstanceType=ecs.mn4.2xlarge
&HostGroup.1.1odeCount=4
&RegionId=cn-hangzhou
&TemplateName=new_template_name
&<公共请求参数>

```

正常返回示例

`XML` 格式

``` {#xml_return_success_demo}
<ModifyClusterTemplateResponse>
	  <RequestId>5EDD1207-5DAB-42F9-9BF9-7591286A8F3F</RequestId>
</ModifyClusterTemplateResponse>
```

`JSON` 格式

``` {#json_return_success_demo}
{
	"RequestId":"5EDD1207-5DAB-42F9-9BF9-7591286A8F3F"
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

访问[错误中心](https://error-center.alibabacloud.com/status/product/Emr)查看更多错误码。

