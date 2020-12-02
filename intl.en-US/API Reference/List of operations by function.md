# List of operations by function

The following tables list the API operations available for use in E-MapReduce \(EMR\).

## Cluster management

|Operation|Description|
|---------|-----------|
|[CreateClusterV2](/intl.en-US/API Reference/Cluster/CreateClusterV2.md)|Creates an EMR cluster.|
|[ModifyClusterName](/intl.en-US/API Reference/Cluster/ModifyClusterName.md)|Modifies the name of a cluster.|
|[DescribeClusterV2](/intl.en-US/API Reference/Cluster/DescribeClusterV2.md)|Queries the basic information of a cluster, such as the billing method, Elastic Compute Service \(ECS\) instance overview, and EMR services.|
|[ReleaseCluster](/intl.en-US/API Reference/Cluster/ReleaseCluster.md)|Releases all nodes in a cluster.|
|[ResizeClusterV2](/intl.en-US/API Reference/Cluster/ResizeClusterV2.md)|Scales a cluster.|
|[ListClusters](/intl.en-US/API Reference/Cluster/ListClusters.md)|Queries clusters. The query results are returned by page.|
|[CreateClusterTemplate](/intl.en-US/API Reference/Cluster/CreateClusterTemplate.md)|Creates a cluster template. You can use this template to initialize new clusters during data development.|
|[CreateClusterWithTemplate](/intl.en-US/API Reference/Cluster/CreateClusterWithTemplate.md)|Creates a cluster by using a cluster template.|
|[DeleteClusterTemplate](/intl.en-US/API Reference/Cluster/DeleteClusterTemplate.md)|Deletes a cluster template.|
|[DescribeClusterTemplate](/intl.en-US/API Reference/Cluster/DescribeClusterTemplate.md)|Queries the details of a cluster template.|
|[DescribeEmrMainVersion](/intl.en-US/API Reference/Cluster/DescribeEmrMainVersion.md)|Queries the details of an EMR cluster version.|
|[ListClusterHost](/intl.en-US/API Reference/Cluster/ListClusterHost.md)|Queries the information of hosts in an EMR cluster, such as the configurations of disks, CPUs, and memory.|
|[ListClusterServiceQuickLink](/intl.en-US/API Reference/Cluster/ListClusterServiceQuickLink.md)|Queries the links of services that are used in a cluster.|
|[ListClusterTemplates](/intl.en-US/API Reference/Cluster/ListClusterTemplates.md)|Queries cluster templates.|
|[ListEmrAvailableConfig](/intl.en-US/API Reference/Cluster/ListEmrAvailableConfig.md)|Queries the available configurations that can be used to create an EMR cluster.|
|[ListEmrAvailableResource](/intl.en-US/API Reference/Cluster/ListEmrAvailableResource.md)|Queries available EMR resources.|
|[ListEmrMainVersion](/intl.en-US/API Reference/Cluster/ListEmrMainVersion.md)|Queries EMR versions.|
|[ModifyClusterTemplate](/intl.en-US/API Reference/Cluster/ModifyClusterTemplate.md)|Modifies a cluster template.|
|[DescribeClusterBasicInfo](/intl.en-US/API Reference/Cluster/DescribeClusterBasicInfo.md)|Queries basic information of a created cluster.|
|[ListClusterHostGroup](/intl.en-US/API Reference/Cluster/ListClusterHostGroup.md)|Queries host groups in a cluster.|
|[TagResources](/intl.en-US/API Reference/Tags/TagResources.md)|Creates and binds tags to specified EMR clusters.|
|[ListTagResources](/intl.en-US/API Reference/Tags/ListTagResources.md)|Queries the tags that are bound to one or more EMR clusters.|
|[UntagResources](/intl.en-US/API Reference/Tags/UntagResources.md)|Unbinds tags from specified EMR clusters. If a tag is not bound to any other clusters after it is unbound from a cluster, the tag is automatically deleted.|
|[JoinResourceGroup](/intl.en-US/API Reference/Cluster/JoinResourceGroup.md)|Adds an EMR resource to a resource group.|
|[ReleaseClusterHostGroup](/intl.en-US/API Reference/Cluster/ReleaseClusterHostGroup.md)|Removes host groups from a cluster.|

## Cluster service management

|Operation|Description|
|---------|-----------|
|[AddClusterService](/intl.en-US/API Reference/Cluster service/AddClusterService.md)|Adds a service that is supported by the current EMR version to a specified cluster.|
|[CreateResourcePool](/intl.en-US/API Reference/Cluster service/CreateResourcePool.md)|Creates a YARN resource pool.|
|[CreateResourceQueue](/intl.en-US/API Reference/Cluster service/CreateResourceQueue.md)|Creates a resource queue.|
|[DeleteResourcePool](/intl.en-US/API Reference/Cluster service/DeleteResourcePool.md)|Deletes a specified resource pool.|
|[DeleteResourceQueue](/intl.en-US/API Reference/Cluster service/DeleteResourceQueue.md)|Deletes a resource queue.|
|[DescribeClusterOperationHostTaskLog](/intl.en-US/API Reference/Cluster service/DescribeClusterOperationHostTaskLog.md)|Queries the operations logs of a specified task on a specified host of a cluster.|
|[DescribeClusterResourcePoolSchedulerType](/intl.en-US/API Reference/Cluster service/Describeclusterclusternetworkoolschedulertype.md)|Queries the scheduling policies of resource pools.|
|[DescribeClusterService](/intl.en-US/API Reference/Cluster service/DescribeClusterService.md)|Queries the details of a service in a cluster.|
|[DescribeClusterServiceConfig](/intl.en-US/API Reference/Cluster service/DescribeClusterServiceConfig.md)|Queries the configuration details of a specified service in a cluster.|
|[DescribeClusterServiceConfigTag](/intl.en-US/API Reference/Cluster service/DescribeClusterServiceConfigTag.md)|Queries the configuration tags and tag values of a specified service in a cluster. The responses that are returned can be used to filter configuration items.|
|[ListClusterHostComponent](/intl.en-US/API Reference/Cluster service/ListClusterHostComponent.md)|Queries the components that are installed on each host of a cluster.|
|[ListClusterOperation](/intl.en-US/API Reference/Cluster service/ListClusterOperation.md)|Queries the actions that are taken on a cluster.|
|[ListClusterOperationTask](/intl.en-US/API Reference/Cluster service/ListClusterOperationTask.md)|Queries the tasks of the cluster on which a specific action is taken.|
|[ListClusterOperationHost](/intl.en-US/API Reference/Cluster service/ListClusterOperationHost.md)|Queries the hosts on which a specific action is taken.|
|[ListClusterOperationHostTask](/intl.en-US/API Reference/Cluster service/ListClusterOperationHostTask.md)|Queries the tasks of the host on which a specific action is taken.|
|[ListClusterInstalledService](/intl.en-US/API Reference/Cluster service/ListClusterInstalledService.md)|Queries the services that are installed in a cluster.|
|[ListClusterService](/intl.en-US/API Reference/Cluster service/ListClusterService.md)|Queries services in a cluster.|
|[ListClusterServiceComponentHealthInfo](/intl.en-US/API Reference/Cluster service/ListClusterServiceComponentHealthInfo.md)|Queries the health information of the components of a specified service in a cluster.|
|[ListClusterServiceConfigHistory](/intl.en-US/API Reference/Cluster service/ListClusterServiceConfigHistory.md)|Queries the configuration change history of a specified service in a cluster.|
|[ListResourcePool](/intl.en-US/API Reference/Cluster service/ListResourcePool.md)|Queries resource pools.|
|[ModifyClusterServiceConfig](/intl.en-US/API Reference/Cluster service/ModifyClusterServiceConfig.md)|Modifies the configuration information of a specified service in a cluster.|
|[ModifyResourcePool](/intl.en-US/API Reference/Cluster service/ModifyResourcePool.md)|Updates a resource pool.|
|[ModifyResourcePoolSchedulerType](/intl.en-US/API Reference/Cluster service/ModifyResourcePoolSchedulerType.md)|Modifies the scheduler type of a resource pool.|
|[ModifyResourceQueue](/intl.en-US/API Reference/Cluster service/ModifyResourceQueue.md)|Modifies a resource queue.|
|[RefreshClusterResourcePool](/intl.en-US/API Reference/Cluster service/RefreshClusterResourcePool.md)|Synchronizes the configuration of a resource pool to a cluster.|
|[RunClusterServiceAction](/intl.en-US/API Reference/Cluster service/RunClusterServiceAction.md)|Performs a specified action on a specified service in a cluster.|

## Tag management

|Operation|Description|
|---------|-----------|
|[TagResources](/intl.en-US/API Reference/Tags/TagResources.md)|Creates and binds tags to specified EMR clusters.|
|[ListTagResources](/intl.en-US/API Reference/Tags/ListTagResources.md)|Queries the tags that are bound to one or more EMR clusters.|
|[UntagResources](/intl.en-US/API Reference/Tags/UntagResources.md)|Unbinds tags from specified EMR clusters.|

## Auto scaling

|Operation|Description|
|---------|-----------|
|[CreateScalingGroupV2](/intl.en-US/API Reference/Auto Scaling/CreateScalingGroupV2.md)|Creates a scaling group.|
|[AddScalingConfigItemV2](/intl.en-US/API Reference/Auto Scaling/AddScalingConfigItemV2.md)|Adds a scaling configuration item.|
|[ModifyScalingGroupV2](/intl.en-US/API Reference/Auto Scaling/ModifyScalingGroupV2.md)|Modifies the basic information of a scaling group.|
|[ListScalingGroupV2](/intl.en-US/API Reference/Auto Scaling/ListScalingGroupV2.md)|Queries scaling groups.|
|[ListScalingConfigItemV2](/intl.en-US/API Reference/Auto Scaling/ListScalingConfigItemV2.md)|Queries scaling configuration items.|
|[ListScalingActivityV2](/intl.en-US/API Reference/Auto Scaling/ListScalingActivityV2.md)|Queries scaling activities.|
|[DescribeScalingConfigItemV2](/intl.en-US/API Reference/Auto Scaling/DescribeScalingConfigItemV2.md)|Queries the details of a scaling configuration item.|
|[DescribeScalingGroupInstanceV2](/intl.en-US/API Reference/Auto Scaling/DescribeScalingGroupInstanceV2.md)|Queries the instance details of a running scaling group.|
|[DescribeScalingGroupV2](/intl.en-US/API Reference/Auto Scaling/DescribeScalingGroupV2.md)|Queries the details of a scaling group.|
|[RunScalingActionV2](/intl.en-US/API Reference/Auto Scaling/RunScalingActionV2.md)|Performs a sample control action on a scaling group.|
|[RemoveScalingConfigItemV2](/intl.en-US/API Reference/Auto Scaling/RemoveScalingConfigItemV2.md)|Removes a scaling configuration item.|

## Data development

|Operation|Description|
|---------|-----------|
|[CloneFlow](/intl.en-US/API Reference/Data development/CloneFlow.md)|Clones a workflow.|
|[CloneFlowJob](/intl.en-US/API Reference/Data development/CloneFlowJob.md)|Clones a job.|
|[CreateFlowCategory](/intl.en-US/API Reference/Data development/CreateFlowCategory.md)|Creates a directory.|
|[CreateFlowForWeb](/intl.en-US/API Reference/Data development/CreateFlowForWeb.md)|Creates a custom graphical workflow.|
|[CreateFlowJob](/intl.en-US/API Reference/Data development/CreateFlowJob.md)|Creates a data development job.|
|[CreateFlowProject](/intl.en-US/API Reference/Data development/CreateFlowProject.md)|Creates a data development project.|
|[CreateFlowProjectClusterSetting](/intl.en-US/API Reference/Data development/CreateFlowProjectClusterSetting.md)|Creates cluster settings for a project.|
|[CreateFlowProjectUser](/intl.en-US/API Reference/Data development/CreateFlowProjectUser.md)|Adds a RAM user for a project.|
|[DeleteFlow](/intl.en-US/API Reference/Data development/DeleteFlow.md)|Deletes a workflow.|
|[DeleteFlowCategory](/intl.en-US/API Reference/Data development/DeleteFlowCategory.md)|Deletes a workflow directory.|
|[DeleteFlowJob](/intl.en-US/API Reference/Data development/DeleteFlowJob.md)|Deletes a job.|
|[DeleteFlowProject](/intl.en-US/API Reference/Data development/DeleteFlowProject.md)|Deletes a data development project.|
|[DeleteFlowProjectClusterSetting](/intl.en-US/API Reference/Data development/DeleteFlowProjectClusterSetting.md)|Deletes cluster settings from a project.|
|[DeleteFlowProjectUser](/intl.en-US/API Reference/Data development/DeleteFlowProjectUser.md)|Deletes a RAM user from a project.|
|[DescribeFlow](/intl.en-US/API Reference/Data development/DescribeFlow.md)|Queries the information of a workflow.|
|[DescribeFlowCategory](/intl.en-US/API Reference/Data development/DescribeFlowCategory.md)|Queries the information of a directory.|
|[DescribeFlowCategoryTree](/intl.en-US/API Reference/Data development/DescribeFlowCategoryTree.md)|Queries the information of a directory tree.|
|[DescribeFlowInstance](/intl.en-US/API Reference/Data development/DescribeFlowInstance.md)|Queries the information of a workflow instance.|
|[DescribeFlowJob](/intl.en-US/API Reference/Data development/DescribeFlowJob.md)|Queries the information of a job.|
|[DescribeFlowNodeInstance](/intl.en-US/API Reference/Data development/DescribeFlowNodeInstance.md)|Queries the details of the instance on which a workflow node or a job node runs.|
|[DescribeFlowNodeInstanceContainerLog](/intl.en-US/API Reference/Data development/DescribeFlowNodeInstanceContainerLog.md)|Queries the log of a container that runs on a node instance.|
|[DescribeFlowNodeInstanceLauncherLog](/intl.en-US/API Reference/Data development/DescribeFlowNodeInstanceLauncherLog.md)|Queries the log of a launcher that runs on a node instance.|
|[DescribeFlowProject](/intl.en-US/API Reference/Data development/DescribeFlowProject.md)|Queries the details of a project.|
|[DescribeFlowProjectClusterSetting](/intl.en-US/API Reference/Data development/DescribeFlowProjectClusterSetting.md)|Queries the cluster settings for a project.|
|[KillFlowJob](/intl.en-US/API Reference/Data development/KillFlowJob.md)|Terminates a job instance.|
|[ListFlow](/intl.en-US/API Reference/Data development/ListFlow.md)|Queries workflows.|
|[ListFlowCluster](/intl.en-US/API Reference/Data development/ListFlowCluster.md)|Queries the clusters that are available for a project.|
|[ListFlowClusterAll](/intl.en-US/API Reference/Data development/ListFlowClusterAll.md)|Queries all the clusters that are available in a specified region for data development.|
|[ListFlowClusterAllHosts](/intl.en-US/API Reference/Data development/ListFlowClusterAllHosts.md)|Queries the member hosts that you can add to the whitelist of a project. Only master nodes and the nodes in a gateway cluster are supported.|
|[ListFlowInstance](/intl.en-US/API Reference/Data development/ListFlowInstance.md)|Queries workflow instances.|
|[ListFlowJob](/intl.en-US/API Reference/Data development/ListFlowJob.md)|Queries jobs.|
|[ListFlowJobHistory](/intl.en-US/API Reference/Data development/ListFlowJobHistory.md)|Queries job instances.|
|[ListFlowNodeInstance](/intl.en-US/API Reference/Data development/ListFlowNodeInstance.md)|Queries the instances on which workflow nodes run.|
|[ListFlowNodeInstanceContainerStatus](/intl.en-US/API Reference/Data development/ListFlowNodeInstanceContainerStatus.md)|Queries the status of containers that run on a node instance.|
|[ListFlowNodeSqlResult](/intl.en-US/API Reference/Data development/ListFlowNodeSqlResult.md)|Queries the SQL results of a node instance.|
|[ListFlowProject](/intl.en-US/API Reference/Data development/ListFlowProject.md)|Queries projects.|
|[ListFlowProjectClusterSetting](/intl.en-US/API Reference/Data development/ListFlowProjectClusterSetting.md)|Queries the cluster settings for a project.|
|[ListFlowProjectUser](/intl.en-US/API Reference/Data development/ListFlowProjectUser.md)|Queries the users of a project.|
|[ModifyFlow](/intl.en-US/API Reference/Data development/ModifyFlow.md)|Modifies a workflow.|
|[ListFlowCategory](/intl.en-US/API Reference/Data development/ListFlowCategory.md)|Queries workflow directories.|
|[ModifyFlowProjectClusterSetting](/intl.en-US/API Reference/Data development/ModifyFlowProjectClusterSetting.md)|Modifies the cluster settings for a project.|
|[ModifyFlowCategory](/intl.en-US/API Reference/Data development/ModifyFlowCategory.md)|Renames a directory.|
|[ModifyFlowForWeb](/intl.en-US/API Reference/Data development/ModifyFlowForWeb.md)|Modifies a custom graphical workflow.|
|[ModifyFlowJob](/intl.en-US/API Reference/Data development/ModifyFlowJob.md)|Modifies a data development job.|
|[RerunFlow](/intl.en-US/API Reference/Data development/RerunFlow.md)|Reruns a workflow instance. Before you rerun a workflow instance, make sure that the previous execution of the workflow instance is completed.|
|[ResumeFlow](/intl.en-US/API Reference/Data development/ResumeFlow.md)|Resumes a workflow.|
|[SubmitFlow](/intl.en-US/API Reference/Data development/SubmitFlow.md)|Submits a workflow.|
|[SubmitFlowJob](/intl.en-US/API Reference/Data development/SubmitFlowJob.md)|Submits a job. You can run only one job instance at a time.|
|[SuspendFlow](/intl.en-US/API Reference/Data development/SuspendFlow.md)|Suspends a workflow.|
|[ModifyFlowProject](/intl.en-US/API Reference/Data development/ModifyFlowProject.md)|Modifies a data development project.|
|[ListFlowClusterHost](/intl.en-US/API Reference/Data development/ListFlowClusterHost.md)|Queries the hosts that can submit jobs in a cluster.|

