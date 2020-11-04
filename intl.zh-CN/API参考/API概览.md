# API概览

E-MapReduce提供以下API接口。

## 集群

|API|描述|
|---|--|
|[CreateClusterV2](/intl.zh-CN/API参考/集群/CreateClusterV2.md)|调用CreateClusterV2接口，创建一个E-MapReduce集群。|
|[ModifyClusterName](/intl.zh-CN/API参考/集群/ModifyClusterName.md)|调用 ModifyClusterName接口，修改集群名称。|
|[DescribeClusterV2](/intl.zh-CN/API参考/集群/DescribeClusterV2.md)|调用 DescribeClusterV2接口，查询集群的基本信息，包括：付费、ECS机器概况和E-MapReduce服务列表等。|
|[ReleaseCluster](/intl.zh-CN/API参考/集群/ReleaseCluster.md)|调用 ReleaseCluster接口，释放集群所有节点。|
|[ResizeClusterV2](/intl.zh-CN/API参考/集群/ResizeClusterV2.md)|调用 ResizeClusterV2接口，根据配置扩容集群。|
|[ListClusters](/intl.zh-CN/API参考/集群/ListClusters.md)|调用 ListClusters接口，分页查询集群列表。|
|[CreateClusterTemplate](/intl.zh-CN/API参考/集群/CreateClusterTemplate.md)|调用CreateClusterTemplate接口，创建一个E-MapReduce集模板，可用于数据开发初始化新集群。|
|[CreateClusterWithTemplate](/intl.zh-CN/API参考/集群/CreateClusterWithTemplate.md)|调用 CreateClusterWithTemplate接口，通过集群模版创建集群。|
|[DeleteClusterTemplate](/intl.zh-CN/API参考/集群/DeleteClusterTemplate.md)|调用 DeleteClusterTemplate接口，删除集群模版。|
|[DescribeClusterTemplate](/intl.zh-CN/API参考/集群/DescribeClusterTemplate.md)|调用 DescribeClusterTemplate接口，查询集群模版详情。|
|[DescribeEmrMainVersion](/intl.zh-CN/API参考/集群/DescribeEmrMainVersion.md)|调用 DescribeEmrMainVersion接口，查询集群EMR版本详情。|
|[ListClusterHost](/intl.zh-CN/API参考/集群/ListClusterHost.md)|调用ListClusterHost接口，查询集群主机列表，包括磁盘和CPU内存配置。|
|[ListClusterServiceQuickLink](/intl.zh-CN/API参考/集群/ListClusterServiceQuickLink.md)|调用ListClusterServiceQuickLink接口，查询集群快捷链接列表。|
|[ListClusterTemplates](/intl.zh-CN/API参考/集群/ListClusterTemplates.md)|调用ListClusterTemplates接口，查询集群模版列表。|
|[ListEmrAvailableConfig](/intl.zh-CN/API参考/集群/ListEmrAvailableConfig.md)|调用ListEmrAvailableConfig接口，获取可用的EMR集群创建配置。|
|[ListEmrAvailableResource](/intl.zh-CN/API参考/集群/ListEmrAvailableResource.md)|调用ListEmrAvailableResource接口，查询可用资源列表。|
|[ListEmrMainVersion](/intl.zh-CN/API参考/集群/ListEmrMainVersion.md)|调用ListEmrMainVersion接口，查询 E-MapReduce 版本列表。|
|[ModifyClusterTemplate](/intl.zh-CN/API参考/集群/ModifyClusterTemplate.md)|调用ModifyClusterTemplate接口，修改集群模版。|
|[DescribeClusterBasicInfo](/intl.zh-CN/API参考/集群/DescribeClusterBasicInfo.md)|调用DescribeClusterBasicInfo接口，查询已创建的集群实例。|
|[ListClusterHostGroup](/intl.zh-CN/API参考/集群/ListClusterHostGroup.md)|调用ListClusterHostGroup接口，查询集群机器组列表。|
|[TagResources](/intl.zh-CN/API参考/标签/TagResources.md)|调用TagResources接口，为指定的EMR集群列表统一创建并绑定标签。|
|[ListTagResources](/intl.zh-CN/API参考/标签/ListTagResources.md)|调用ListTagResources接口，查询一个或多个EMR集群已经绑定的标签列表。|
|[UntagResources](/intl.zh-CN/API参考/标签/UntagResources.md)|调用UntagResources接口，为指定的ECS资源列表统一解绑标签。解绑后，如果该标签没有绑定其他任何资源，会被自动删除。|
|[JoinResourceGroup](/intl.zh-CN/API参考/集群/JoinResourceGroup.md)|调用JoinResourceGroup接口，将一个EMR相关资源加入一个资源组。|
|[ReleaseClusterHostGroup](/intl.zh-CN/API参考/集群/ReleaseClusterHostGroup.md)|调用ReleaseClusterHostGroup接口，进行EMR集群节点缩容。|

## 集群服务

|API|描述|
|---|--|
|[AddClusterService](/intl.zh-CN/API参考/集群服务/AddClusterService.md)|调用AddClusterService接口，为指定的集群添加当前集群的主版本支持的某项服务。|
|[CreateResourcePool](/intl.zh-CN/API参考/集群服务/CreateResourcePool.md)|调用CreateResourcePool接口，创建YARN资源池。|
|[CreateResourceQueue](/intl.zh-CN/API参考/集群服务/CreateResourceQueue.md)|调用CreateResourceQueue接口，创建资源队列。|
|[DeleteResourcePool](/intl.zh-CN/API参考/集群服务/DeleteResourcePool.md)|调用DeleteResourcePool接口，删除指定资源池。|
|[DeleteResourceQueue](/intl.zh-CN/API参考/集群服务/DeleteResourceQueue.md)|调用DeleteResourceQueue接口，删除资源队列。|
|[DescribeClusterOperationHostTaskLog](/intl.zh-CN/API参考/集群服务/DescribeClusterOperationHostTaskLog.md)|调用DescribeClusterOperationHostTaskLog接口，获取集群操作历史中，指定主机上的指定task的执行日志详情。|
|[DescribeClusterResourcePoolSchedulerType](/intl.zh-CN/API参考/集群服务/DescribeClusterResourcePoolSchedulerType.md)|调用DescribeClusterResourcePoolSchedulerType接口，查看资源池策略类型。|
|[DescribeClusterService](/intl.zh-CN/API参考/集群服务/DescribeClusterService.md)|调用DescribeClusterService接口，查询集群当前安装服务的详情信息。|
|[DescribeClusterServiceConfig](/intl.zh-CN/API参考/集群服务/DescribeClusterServiceConfig.md)|调用 DescribeClusterServiceConfig 接口查询集群指定服务的配置详情信息。|
|[DescribeClusterServiceConfigTag](/intl.zh-CN/API参考/集群服务/DescribeClusterServiceConfigTag.md)|调用 DescribeClusterServiceConfigTag 接口，查询集群指定服务的配置标签值列表。此接口的返回结果可用于筛选配置项。|
|[ListClusterHostComponent](/intl.zh-CN/API参考/集群服务/ListClusterHostComponent.md)|调用ListClusterHostComponent接口，获取集群各个主机上安装的组件列表。|
|[ListClusterOperation](/intl.zh-CN/API参考/集群服务/ListClusterOperation.md)|调用ListClusterOperation接口，查询集群的操作历史列表。|
|[ListClusterOperationTask](/intl.zh-CN/API参考/集群服务/ListClusterOperationTask.md)|调用ListClusterOperationTask接口，查询指定的操作历史中主机对应的任务列表信息。|
|[ListClusterOperationHost](/intl.zh-CN/API参考/集群服务/ListClusterOperationHost.md)|调用ListClusterOperationHost接口，查询操作历史的操作机器列表。|
|[ListClusterOperationHostTask](/intl.zh-CN/API参考/集群服务/ListClusterOperationHostTask.md)|调用ListClusterOperationHostTask接口，查询指定的操作历史中主机对应的任务列表信息。|
|[ListClusterInstalledService](/intl.zh-CN/API参考/集群服务/ListClusterInstalledService.md)|调用ListClusterInstalledService接口，查询集群当前已安装的服务列表信息。|
|[ListClusterService](/intl.zh-CN/API参考/集群服务/ListClusterService.md)|调用ListClusterService接口，查询集群的服务列表信息。|
|[ListClusterServiceComponentHealthInfo](/intl.zh-CN/API参考/集群服务/ListClusterServiceComponentHealthInfo.md)|调用ListClusterServiceComponentHealthInfo接口，获取集群指定服务对应的组件健康信息列表。|
|[ListClusterServiceConfigHistory](/intl.zh-CN/API参考/集群服务/ListClusterServiceConfigHistory.md)|调用ListClusterServiceConfigHistory接口，查询集群指定服务的配置修改历史信息的接口。|
|[ListResourcePool](/intl.zh-CN/API参考/集群服务/ListResourcePool.md)|调用ListResourcePool接口，查询资源池列表。|
|[ModifyClusterServiceConfig](/intl.zh-CN/API参考/集群服务/ModifyClusterServiceConfig.md)|调用ModifyClusterServiceConfig接口，修改集群指定服务的配置信息。|
|[ModifyResourcePool](/intl.zh-CN/API参考/集群服务/ModifyResourcePool.md)|调用ModifyResourcePool接口，更新资源池。|
|[ModifyResourcePoolSchedulerType](/intl.zh-CN/API参考/集群服务/ModifyResourcePoolSchedulerType.md)|调用ModifyResourcePoolSchedulerType接口，修改资源池调度类型。|
|[ModifyResourceQueue](/intl.zh-CN/API参考/集群服务/ModifyResourceQueue.md)|调用ModifyResourceQueue接口，修改资源队列。|
|[RefreshClusterResourcePool](/intl.zh-CN/API参考/集群服务/RefreshClusterResourcePool.md)|调用RefreshClusterResourcePool接口，同步资源池配置到集群。|
|[RunClusterServiceAction](/intl.zh-CN/API参考/集群服务/RunClusterServiceAction.md)|调用RunClusterServiceAction接口，对集群的指定服务，运行指定的操作。|

## 标签

|API|描述|
|---|--|
|[TagResources](/intl.zh-CN/API参考/标签/TagResources.md)|调用TagResources接口，为指定的EMR集群列表统一创建并绑定标签。|
|[ListTagResources](/intl.zh-CN/API参考/标签/ListTagResources.md)|调用ListTagResources接口，查询一个或多个EMR集群已经绑定的标签列表。|
|[UntagResources](/intl.zh-CN/API参考/标签/UntagResources.md)|调用UntagResources接口，为指定的EMR集群列统一解绑标签。|

## 弹性伸缩

|API|描述|
|---|--|
|[CreateScalingGroupV2](/intl.zh-CN/API参考/弹性伸缩/CreateScalingGroupV2.md)|调用CreateScalingGroupV2接口，创建伸缩组。|
|[AddScalingConfigItemV2](/intl.zh-CN/API参考/弹性伸缩/AddScalingConfigItemV2.md)|调用AddScalingConfigItemV2接口，新建弹性伸缩配置项。|
|[修改伸缩组](/intl.zh-CN/API参考/弹性伸缩/ModifyScalingGroupV2.md)|调用ModifyScalingGroupV2接口，修改伸缩组基础信息。|
|[ListScalingGroupV2](/intl.zh-CN/API参考/弹性伸缩/ListScalingGroupV2.md)|调用ListScalingGroupV2接口，查看伸缩组列表。|
|[ListScalingConfigItemV2](/intl.zh-CN/API参考/弹性伸缩/ListScalingConfigItemV2.md)|调用ListScalingConfigItemV2接口，查看伸缩配置项列表。|
|[ListScalingActivityV2](/intl.zh-CN/API参考/弹性伸缩/ListScalingActivityV2.md)|调用ListScalingActivityV2接口，查看伸缩活动列表。|
|[DescribeScalingConfigItemV2](/intl.zh-CN/API参考/弹性伸缩/DescribeScalingConfigItemV2.md)|调用DescribeScalingConfigItemV2接口，获取伸缩配置项详情。|
|[DescribeScalingGroupInstanceV2](/intl.zh-CN/API参考/弹性伸缩/DescribeScalingGroupInstanceV2.md)|调用DescribeScalingGroupInstanceV2接口，获取一个正在运行中的伸缩组实例详情。|
|[DescribeScalingGroupV2](/intl.zh-CN/API参考/弹性伸缩/DescribeScalingGroupV2.md)|调用DescribeScalingGroupV2接口，获取伸缩组详情。|
|[RunScalingActionV2](/intl.zh-CN/API参考/弹性伸缩/RunScalingActionV2.md)|调用RunScalingActionV2接口，执行伸缩组示例控制动作。|
|[RemoveScalingConfigItemV2](/intl.zh-CN/API参考/弹性伸缩/RemoveScalingConfigItemV2.md)|调用RemoveScalingConfigItemV2接口，删除伸缩配置项。|

## 数据开发

|API|描述|
|---|--|
|[CloneFlow](/intl.zh-CN/API参考/数据开发/CloneFlow.md)|调用CloneFlow接口，克隆工作流。|
|[CloneFlowJob](/intl.zh-CN/API参考/数据开发/CloneFlowJob.md)|调用CloneFlowJob接口，克隆作业。|
|[CreateFlowCategory](/intl.zh-CN/API参考/数据开发/CreateFlowCategory.md)|调用CreateFlowCategory接口，创建目录文件夹。|
|[CreateFlowForWeb](/intl.zh-CN/API参考/数据开发/CreateFlowForWeb.md)|调用CreateFlowForWeb接口，创建自定义图形工作流。|
|[CreateFlowJob](/intl.zh-CN/API参考/数据开发/CreateFlowJob.md)|调用 CreateFlowJob 接口，创建数据开发作业。|
|[CreateFlowProject](/intl.zh-CN/API参考/数据开发/CreateFlowProject.md)|调用CreateFlowProject接口，创建数据开发项目。|
|[CreateFlowProjectClusterSetting](/intl.zh-CN/API参考/数据开发/CreateFlowProjectClusterSetting.md)|调用CreateFlowProjectClusterSetting接口，创建项目集群设置。|
|[CreateFlowProjectUser](/intl.zh-CN/API参考/数据开发/CreateFlowProjectUser.md)|调用CreateFlowProjectUser接口，添加项目用户。|
|[DeleteFlow](/intl.zh-CN/API参考/数据开发/DeleteFlow.md)|调用DeleteFlow接口，删除工作流|
|[DeleteFlowCategory](/intl.zh-CN/API参考/数据开发/DeleteFlowCategory.md)|调用DeleteFlowCategory接口，删除工作流目录。|
|[DeleteFlowJob](/intl.zh-CN/API参考/数据开发/DeleteFlowJob.md)|调用DeleteFlowJob接口，删除作业。|
|[DeleteFlowProject](/intl.zh-CN/API参考/数据开发/DeleteFlowProject.md)|调用DeleteFlowProject接口，删除数据开发项目。|
|[DeleteFlowProjectClusterSetting](/intl.zh-CN/API参考/数据开发/DeleteFlowProjectClusterSetting.md)|调用DeleteFlowProjectClusterSetting接口，删除项目集群设置。|
|[DeleteFlowProjectUser](/intl.zh-CN/API参考/数据开发/DeleteFlowProjectUser.md)|调用DeleteFlowProjectUser接口，删除项目用户。|
|[DescribeFlow](/intl.zh-CN/API参考/数据开发/DescribeFlow.md)|调用DescribeFlow接口，查询工作流信息。|
|[DescribeFlowCategory](/intl.zh-CN/API参考/数据开发/DescribeFlowCategory.md)|调用DescribeFlowCategory接口，查询目录详细信息。|
|[DescribeFlowCategoryTree](/intl.zh-CN/API参考/数据开发/DescribeFlowCategoryTree.md)|调用DescribeFlowCategoryTree接口，获取目录树。|
|[DescribeFlowInstance](/intl.zh-CN/API参考/数据开发/DescribeFlowInstance.md)|调用DescribeFlowInstance接口，获取工作流实例信息。|
|[DescribeFlowJob](/intl.zh-CN/API参考/数据开发/DescribeFlowJob.md)|调用DescribeFlowJob接口，查询作业信息。|
|[DescribeFlowNodeInstance](/intl.zh-CN/API参考/数据开发/DescribeFlowNodeInstance.md)|调用DescribeFlowNodeInstance接口，查询节点实例详情（工作流节点实例， 作业运行节点实例）。|
|[DescribeFlowNodeInstanceContainerLog](/intl.zh-CN/API参考/数据开发/DescribeFlowNodeInstanceContainerLog.md)|调用DescribeFlowNodeInstanceContainerLog接口，查询节点实例容器日志。|
|[DescribeFlowNodeInstanceLauncherLog](/intl.zh-CN/API参考/数据开发/DescribeFlowNodeInstanceLauncherLog.md)|调用DescribeFlowNodeInstanceLauncherLog接口，查询节点实例启动器日志。|
|[DescribeFlowProject](/intl.zh-CN/API参考/数据开发/DescribeFlowProject.md)|调用DescribeFlowProject接口，查询项目详情。|
|[DescribeFlowProjectClusterSetting](/intl.zh-CN/API参考/数据开发/DescribeFlowProjectClusterSetting.md)|调用DescribeFlowProjectClusterSetting接口，查询项目集群设置详情。|
|[KillFlowJob](/intl.zh-CN/API参考/数据开发/KillFlowJob.md)|调用KillFlowJob接口，停止作业实例。|
|[ListFlow](/intl.zh-CN/API参考/数据开发/ListFlow.md)|调用ListFlow接口，查询工作流列表。|
|[ListFlowCluster](/intl.zh-CN/API参考/数据开发/ListFlowCluster.md)|调用ListFlowCluster接口，查询项目中可用的集群列表。|
|[ListFlowClusterAll](/intl.zh-CN/API参考/数据开发/ListFlowClusterAll.md)|调用ListFlowClusterAll接口，查询数据开发可用的集群列表。|
|[ListFlowClusterAllHosts](/intl.zh-CN/API参考/数据开发/ListFlowClusterAllHosts.md)|调用ListFlowClusterAllHosts接口，查询给定集群可在项目设置中设置到白名单中的主机列表，目前支持master和gateway节点。|
|[ListFlowInstance](/intl.zh-CN/API参考/数据开发/ListFlowInstance.md)|调用ListFlowInstance接口，查询工作流实例列表。|
|[ListFlowJob](/intl.zh-CN/API参考/数据开发/ListFlowJob.md)|调用ListFlowJob接口，查询作业列表。|
|[ListFlowJobHistory](/intl.zh-CN/API参考/数据开发/ListFlowJobHistory.md)|调用ListFlowJobHistory接口，查询作业的运行实例列表。|
|[ListFlowNodeInstance](/intl.zh-CN/API参考/数据开发/ListFlowNodeInstance.md)|调用ListFlowNodeInstance接口，查询工作流节点实例列表。|
|[ListFlowNodeInstanceContainerStatus](/intl.zh-CN/API参考/数据开发/ListFlowNodeInstanceContainerStatus.md)|调用ListFlowNodeInstanceContainerStatus接口，查询节点实例的容器状态详情。|
|[ListFlowNodeSqlResult](/intl.zh-CN/API参考/数据开发/ListFlowNodeSqlResult.md)|调用ListFlowNodeSqlResult接口，查询节点实例 sql 结果。|
|[ListFlowProject](/intl.zh-CN/API参考/数据开发/ListFlowProject.md)|调用ListFlowProject接口，查询项目列表。|
|[ListFlowProjectClusterSetting](/intl.zh-CN/API参考/数据开发/ListFlowProjectClusterSetting.md)|调用ListFlowProjectClusterSetting接口，查询项目集群设置列表。|
|[ListFlowProjectUser](/intl.zh-CN/API参考/数据开发/ListFlowProjectUser.md)|调用ListFlowProjectUser接口，查询项目用户列表。|
|[ModifyFlow](/intl.zh-CN/API参考/数据开发/ModifyFlow.md)|调用ModifyFlow接口，修改工作流。|
|[ListFlowCategory](/intl.zh-CN/API参考/数据开发/ListFlowCategory.md)|调用ListFlowCategory接口，展示工作流目录。|
|[ModifyFlowProjectClusterSetting](/intl.zh-CN/API参考/数据开发/ModifyFlowProjectClusterSetting.md)|调用ModifyFlowProjectClusterSetting接口，修改项目集群设置。|
|[ModifyFlowCategory](/intl.zh-CN/API参考/数据开发/ModifyFlowCategory.md)|调用ModifyFlowCategory接口，重命名目录。|
|[ModifyFlowForWeb](/intl.zh-CN/API参考/数据开发/ModifyFlowForWeb.md)|调用ModifyFlowForWeb接口，修改带有图形信息的工作流。|
|[ModifyFlowJob](/intl.zh-CN/API参考/数据开发/ModifyFlowJob.md)|调用ModifyFlowJob接口，修改数据开发作业。|
|[RerunFlow](/intl.zh-CN/API参考/数据开发/RerunFlow.md)|调用 RerunFlo接口，重跑工作流实例，要求工作流实例已经结束。|
|[ResumeFlow](/intl.zh-CN/API参考/数据开发/ResumeFlow.md)|调用ResumeFlow接口，恢复暂停的工作流。|
|[SubmitFlow](/intl.zh-CN/API参考/数据开发/SubmitFlow.md)|调用SubmitFlow接口，提交运行工作流。|
|[SubmitFlowJob](/intl.zh-CN/API参考/数据开发/SubmitFlowJob.md)|调用SubmitFlowJob接口，提交运行作业，每次只允许存在一个正在运行的实例。|
|[SuspendFlow](/intl.zh-CN/API参考/数据开发/SuspendFlow.md)|调用SuspendFlow接口，暂停工作流。|
|[ModifyFlowProject](/intl.zh-CN/API参考/数据开发/ModifyFlowProject.md)|调用ModifyFlowProject接口，修改数据开发项目。|
|[ListFlowClusterHost](/intl.zh-CN/API参考/数据开发/ListFlowClusterHost.md)|调用ListFlowClusterHost接口，查询集群可提交作业的客户端列表。|

