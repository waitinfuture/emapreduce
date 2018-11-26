# ClusterInfo {#concept_xw1_3qb_kfb .concept}

|名称|类型|描述|
|--|--|--|
|Id|String|集群Id|
|RegionId|String|集群Region|
|Name|String|集群名称|
|CreateType|String|集群的创建类型，ON-DEMAND：按需创建，指使用执行计划动态创建出来的集群。MANUAL：手动创建的集群|
|ChargeType|String|集群的付费类型。PREPAID：包年包月，POSTPAID：按量付费|
|StartTime|Long|集群创建的时间，时间戳格式。类似：1459236255438|
|StopTime|Long|集群停止的时间，时间戳格式。类似：1459236255438|
|LogEnable|Boolean|是否开启了日志保存|
|LogPath|String|日志保存的路径|
|Status|String|[集群状态](../../../../intl.zh-CN/常见问题/附录/状态表.md#)|
|HighAvailabilityEnable|Boolean|是否为高可用集群|
|RunningTime|Integer|运行时间，单位：秒|
|MasterNodeTotal|Integer|master节点总数|
|CoreNodeTotal|Integer|core节点总数|
|FailReason|[FailReason](intl.zh-CN/开发指南/API 参考/数据类型/FailReason.md#)|失败原因|
|SoftwareInfo|Array<[SoftwareInfo](intl.zh-CN/开发指南/API 参考/数据类型/SoftwareInfo.md#)\>|软件信息|
|ImageId|String|使用的镜像Id|
|SecurityGroupId|String|安全组Id|
|SecurityGroupName|String|安全组名称|
|EcsInstances|Array<[ECSInstance](intl.zh-CN/开发指南/API 参考/数据类型/EcsInstance.md#)\>|ECS信息|
|BootstrapActions|List<[BootstrapAction](intl.zh-CN/开发指南/API 参考/数据类型/BootstrapAction.md#)\>|引导操作列表，最多16个，超过只保留前16个|
|Configurations|String|提供一个oss文件路径，该文件的内容请参见用户手册|

