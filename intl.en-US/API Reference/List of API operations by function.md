# List of API operations by function

E-MapReduce provides the following API operations.

## Cluster management

|API|Description|
|---|-----------|
|[CreateClusterV2](/intl.en-US/API Reference/Cluster/CreateClusterV2.md)|You can call this operation to create an E-MapReduce cluster.|
|[ModifyClusterName](/intl.en-US/API Reference/Cluster/ModifyClusterName.md)|You can call this operation to modify the name of a ModifyClusterName cluster.|
|[DescribeClusterV2](/intl.en-US/API Reference/Cluster/DescribeClusterV2.md)|You can call DescribeClusterV 2 to query the basic information of a cluster, including the billing method, ECS instance overview, and E-MapReduce service list.|
|[ReleaseCluster](/intl.en-US/API Reference/Cluster/ReleaseCluster.md)|You can call this operation to release all nodes in a cluster.|
|[ResizeClusterV2](/intl.en-US/API Reference/Cluster/ResizeClusterV2.md)|You can call this operation to scale out a cluster based on the configuration.|
|[ListClusters](/intl.en-US/API Reference/Cluster/ListClusters.md)|You can call this operation to query the cluster list by page.|
|[CreateClusterTemplate](/intl.en-US/API Reference/Cluster/CreateClusterTemplate.md)|You can call this operation to create a CreateClusterTemplate set template. The template can be used for data development and initialization of a new cluster.|
|[CreateClusterWithTemplate](/intl.en-US/API Reference/Cluster/CreateClusterWithTemplate.md)|You can call this operation to create a cluster by using a cluster template.|
|[DeleteClusterTemplate](/intl.en-US/API Reference/Cluster/DeleteClusterTemplate.md)|You can call this operation to delete DeleteClusterTemplate cluster template.|
|[DescribeClusterTemplate](/intl.en-US/API Reference/Cluster/DescribeClusterTemplate.md)|You can call this operation to call DescribeClusterTemplate to query cluster templates.|
|[DescribeEmrMainVersion](/intl.en-US/API Reference/Cluster/DescribeEmrMainVersion.md)|You can call this operation to query the details of an EMR Version.|
|[ListClusterHost](/intl.en-US/API Reference/Cluster/ListClusterHost.md)|You can call this operation to query hosts in a ListClusterHost, including disks, CPUs, and memory configurations.|
|[ListClusterServiceQuickLink](/intl.en-US/API Reference/Cluster/ListClusterServiceQuickLink.md)|You can call this operation to query the shortcut links of a cluster.|
|[ListClusterTemplates](/intl.en-US/API Reference/Cluster/ListClusterTemplates.md)|Call ListClusterTemplates to query the list of cluster templates.|
|[ListEmrAvailableConfig](/intl.en-US/API Reference/Cluster/ListEmrAvailableConfig.md)|Call ListEmrAvailableConfig to obtain the available EMR cluster creation configurations.|
|[ListEmrAvailableResource](/intl.en-US/API Reference/Cluster/ListEmrAvailableResource.md)|Call ListEmrAvailableResource to query available resource list.|
|[ListEmrMainVersion](/intl.en-US/API Reference/Cluster/ListEmrMainVersion.md)|You can call this operation to query ListEmrMainVersion versions of a E-MapReduce.|
|[ModifyClusterTemplate](/intl.en-US/API Reference/Cluster/ModifyClusterTemplate.md)|You can call this operation to modify ModifyClusterTemplate cluster template.|
|[DescribeClusterBasicInfo](/intl.en-US/API Reference/Cluster/DescribeClusterBasicInfo.md)|You can call this operation to query the cluster instances that have been created.|
|[ListClusterHostGroup](/intl.en-US/API Reference/Cluster/ListClusterHostGroup.md)|Call ListClusterHostGroup to query the list of cluster machine group.|
|[TagResources](/intl.en-US/API Reference/Tags/TagResources.md)|You can call this operation to create and bind tags to specified EMR clusters.|
|[ListTagResources](/intl.en-US/API Reference/Tags/ListTagResources.md)|You can call this operation to query tags that are bound to one or more EMR clusters.|
|[UntagResources](/intl.en-US/API Reference/Tags/UntagResources.md)|You can call this operation to unbind tags from specified ECS resource list. After a tag is unbound, the tag is automatically deleted if it is not bound to any other resources.|
|[JoinResourceGroup](/intl.en-US/API Reference/Cluster/JoinResourceGroup.md)|You can call this operation to add an EMR resource to a resource group.|
|[ReleaseClusterHostGroup](/intl.en-US/API Reference/Cluster/ReleaseClusterHostGroup.md)|Call ReleaseClusterHostGroup to remove nodes from an EMR cluster.|

## Cluster service management

|API|Description|
|---|-----------|
|[AddClusterService](/intl.en-US/API Reference/Cluster service/AddClusterService.md)|Call the AddClusterService operation to add a service that is supported by the current E-MapReduce \(EMR\) version to a specified cluster.|
|[CreateResourcePool](/intl.en-US/API Reference/Cluster service/CreateResourcePool.md)|Call CreateResourcePool to create a YARN resource pool.|
|[CreateResourceQueue](/intl.en-US/API Reference/Cluster service/CreateResourceQueue.md)|Call CreateResourceQueue to create a resource queue.|
|[DeleteResourcePool](/intl.en-US/API Reference/Cluster service/DeleteResourcePool.md)|Call DeleteResourcePool to delete a resource pool.|
|[DeleteResourceQueue](/intl.en-US/API Reference/Cluster service/DeleteResourceQueue.md)|Call DeleteResourceQueue to delete a resource queue.|
|[DescribeClusterOperationHostTaskLog](/intl.en-US/API Reference/Cluster service/DescribeClusterOperationHostTaskLog.md)|You can call this operation to query the operation logs of a specific task on a specified host in the cluster operation history.|
|[DescribeClusterResourcePoolSchedulerType](/intl.en-US/API Reference/Cluster service/Describeclusterclusternetworkoolschedulertype.md)|Call DescribeClusterResourcePoolSchedulerType API to view the resource pool policy type.|
|[DescribeClusterService](/intl.en-US/API Reference/Cluster service/DescribeClusterService.md)|You can call this operation to query the details of the services installed on a cluster.|
|[DescribeClusterServiceConfig](/intl.en-US/API Reference/Cluster service/DescribeClusterServiceConfig.md)|You can call this operation to query the configuration details of a service in a cluster.|
|[DescribeClusterServiceConfigTag](/intl.en-US/API Reference/Cluster service/DescribeClusterServiceConfigTag.md)|You can call this operation to query the values of configuration tags of a specified service of a cluster. The results returned by this operation can be used to filter configuration items.|
|[ListClusterHostComponent](/intl.en-US/API Reference/Cluster service/ListClusterHostComponent.md)|You can call this operation to query the components installed on the hosts in the cluster.|
|[ListClusterOperation](/intl.en-US/API Reference/Cluster service/ListClusterOperation.md)|You can call this operation to query the operation records of a cluster.|
|[ListClusterOperationTask](/intl.en-US/API Reference/Cluster service/ListClusterOperationTask.md)|You can call this operation to query the tasks of a specified host on which a specific operation is performed.|
|[ListClusterOperationHost](/intl.en-US/API Reference/Cluster service/ListClusterOperationHost.md)|You can call this operation to query the list of physical servers used to perform operations.|
|[ListClusterOperationHostTask](/intl.en-US/API Reference/Cluster service/ListClusterOperationHostTask.md)|You can call this operation to query the tasks of a specified host on which a specific operation is performed.|
|[ListClusterInstalledService](/intl.en-US/API Reference/Cluster service/ListClusterInstalledService.md)|You can call this operation to query ListClusterInstalledService that have been installed in a cluster.|
|[ListClusterService](/intl.en-US/API Reference/Cluster service/ListClusterService.md)|You can call this operation to query the services of a cluster.|
|[ListClusterServiceComponentHealthInfo](/intl.en-US/API Reference/Cluster service/ListClusterServiceComponentHealthInfo.md)|You can call this operation to query the health information of the components of a specified service in a cluster.|
|[ListClusterServiceConfigHistory](/intl.en-US/API Reference/Cluster service/ListClusterServiceConfigHistory.md)|You can call this operation to query the configuration history of a specified service on a cluster.|
|[ListResourcePool](/intl.en-US/API Reference/Cluster service/ListResourcePool.md)|Call ListResourcePool to query the list of resource pools.|
|[ModifyClusterServiceConfig](/intl.en-US/API Reference/Cluster service/ModifyClusterServiceConfig.md)|You can call this operation to modify the configurations of a specified service of a cluster.|
|[ModifyResourcePool](/intl.en-US/API Reference/Cluster service/ModifyResourcePool.md)|Call ModifyResourcePool to update the resource pool.|
|[ModifyResourcePoolSchedulerType](/intl.en-US/API Reference/Cluster service/ModifyResourcePoolSchedulerType.md)|Call ModifyResourcePoolSchedulerType to modify the scheduling mode of the resource pool.|
|[ModifyResourceQueue](/intl.en-US/API Reference/Cluster service/ModifyResourceQueue.md)|Call ModifyResourceQueue to modify the resource queue.|
|[RefreshClusterResourcePool](/intl.en-US/API Reference/Cluster service/RefreshClusterResourcePool.md)|Call RefreshClusterResourcePool to synchronize the resource pool configuration to the cluster.|
|[RunClusterServiceAction](/intl.en-US/API Reference/Cluster service/RunClusterServiceAction.md)|You can call this operation to RunClusterServiceAction a specified service of a cluster.|

## Data platform operations

|API|Description|
|---|-----------|
|[CloneFlow](/intl.en-US/API Reference/Data development/CloneFlow.md)|Call the CloneFlow method to clone a workflow.|
|[CloneFlowJob](/intl.en-US/API Reference/Data development/CloneFlowJob.md)|Call the CloneFlowJob interface to clone a job.|
|[CreateFlowCategory](/intl.en-US/API Reference/Data development/CreateFlowCategory.md)|Call CreateFlowCategory to create a directory.|
|[CreateFlowForWeb](/intl.en-US/API Reference/Data development/CreateFlowForWeb.md)|You can call this operation to create a workflow.|
|[CreateFlowJob](/intl.en-US/API Reference/Data development/CreateFlowJob.md)|You can call this operation to create a data development job.|
|[CreateFlowProject](/intl.en-US/API Reference/Data development/CreateFlowProject.md)|Call the CreateFlowProject to create a data analytics project.|
|[CreateFlowProjectClusterSetting](/intl.en-US/API Reference/Data development/CreateFlowProjectClusterSetting.md)|Call the CreateFlowProjectClusterSetting to create a project cluster.|
|[CreateFlowProjectUser](/intl.en-US/API Reference/Data development/CreateFlowProjectUser.md)|Call CreateFlowProjectUser to add a project user.|
|[DeleteFlow](/intl.en-US/API Reference/Data development/DeleteFlow.md)|You can call this operation to delete a workflow.|
|[DeleteFlowCategory](/intl.en-US/API Reference/Data development/DeleteFlowCategory.md)|You can call this operation to delete a DeleteFlowCategory directory.|
|[DeleteFlowJob](/intl.en-US/API Reference/Data development/DeleteFlowJob.md)|Call the DeleteFlowJob to delete a job.|
|[DeleteFlowProject](/intl.en-US/API Reference/Data development/DeleteFlowProject.md)|Call DeleteFlowProject to delete a data development project.|
|[DeleteFlowProjectClusterSetting](/intl.en-US/API Reference/Data development/DeleteFlowProjectClusterSetting.md)|Call the DeleteFlowProjectClusterSetting API to delete the Project cluster settings.|
|[DeleteFlowProjectUser](/intl.en-US/API Reference/Data development/DeleteFlowProjectUser.md)|Call DeleteFlowProjectUser to delete a project user.|
|[DescribeFlow](/intl.en-US/API Reference/Data development/DescribeFlow.md)|You can call this operation to query workflow information.|
|[DescribeFlowCategory](/intl.en-US/API Reference/Data development/DescribeFlowCategory.md)|You can call this operation to query the DescribeFlowCategory of a directory.|
|[DescribeFlowCategoryTree](/intl.en-US/API Reference/Data development/DescribeFlowCategoryTree.md)|Call DescribeFlowCategoryTree to obtain a directory tree.|
|[DescribeFlowInstance](/intl.en-US/API Reference/Data development/DescribeFlowInstance.md)|You can call this operation to obtain the DescribeFlowInstance of a workflow instance.|
|[DescribeFlowJob](/intl.en-US/API Reference/Data development/DescribeFlowJob.md)|You can call this operation to query the information about a job.|
|[DescribeFlowNodeInstance](/intl.en-US/API Reference/Data development/DescribeFlowNodeInstance.md)|You can call this operation to query the details of a node instance, such as a workflow node instance or a job running node instance.|
|[DescribeFlowNodeInstanceContainerLog](/intl.en-US/API Reference/Data development/DescribeFlowNodeInstanceContainerLog.md)|Call DescribeFlowNodeInstanceContainerLog to query the logs of a node container.|
|[DescribeFlowNodeInstanceLauncherLog](/intl.en-US/API Reference/Data development/DescribeFlowNodeInstanceLauncherLog.md)|You can call this operation to query the launcher log of a node instance.|
|[DescribeFlowProject](/intl.en-US/API Reference/Data development/DescribeFlowProject.md)|You can call this operation to query the details of a DescribeFlowProject.|
|[DescribeFlowProjectClusterSetting](/intl.en-US/API Reference/Data development/DescribeFlowProjectClusterSetting.md)|You can call this operation to query the cluster settings of a DescribeFlowProjectClusterSetting project.|
|[KillFlowJob](/intl.en-US/API Reference/Data development/KillFlowJob.md)|Call this operation to stop a job instance.|
|[ListFlow](/intl.en-US/API Reference/Data development/ListFlow.md)|Call the ListFlow operation to query the list of workflows.|
|[ListFlowCluster](/intl.en-US/API Reference/Data development/ListFlowCluster.md)|You can call this operation to query the clusters available for a project.|
|[ListFlowClusterAll](/intl.en-US/API Reference/Data development/ListFlowClusterAll.md)|You can call this operation to query the clusters available for data development.|
|[ListFlowClusterAllHosts](/intl.en-US/API Reference/Data development/ListFlowClusterAllHosts.md)|You can call this operation to query the whitelist of the specified cluster in the project settings. Only master nodes and gateway nodes are supported.|
|[ListFlowInstance](/intl.en-US/API Reference/Data development/ListFlowInstance.md)|Call ListFlowInstance to query the list of workflow instances.|
|[ListFlowJob](/intl.en-US/API Reference/Data development/ListFlowJob.md)|You can call this operation to query jobs.|
|[ListFlowJobHistory](/intl.en-US/API Reference/Data development/ListFlowJobHistory.md)|You can call this operation to query the running instances of a job.|
|[ListFlowNodeInstance](/intl.en-US/API Reference/Data development/ListFlowNodeInstance.md)|Call ListFlowNodeInstance to query workflow node instances.|
|[ListFlowNodeInstanceContainerStatus](/intl.en-US/API Reference/Data development/ListFlowNodeInstanceContainerStatus.md)|Call the ListFlowNodeInstanceContainerStatus API to query the container status of the node instance.|
|[ListFlowNodeSqlResult](/intl.en-US/API Reference/Data development/ListFlowNodeSqlResult.md)|Call the ListFlowNodeSqlResult API to query the sql query results of the node instance.|
|[ListFlowProject](/intl.en-US/API Reference/Data development/ListFlowProject.md)|Call ListFlowProject to query the project list.|
|[ListFlowProjectClusterSetting](/intl.en-US/API Reference/Data development/ListFlowProjectClusterSetting.md)|Call the ListFlowProjectClusterSetting to query the cluster configurations of a project.|
|[ListFlowProjectUser](/intl.en-US/API Reference/Data development/ListFlowProjectUser.md)|You can call this operation to query the list of project users.|
|[ModifyFlow](/intl.en-US/API Reference/Data development/ModifyFlow.md)|You can call this operation to modify a workflow.|
|[ListFlowCategory](/intl.en-US/API Reference/Data development/ListFlowCategory.md)|Call ListFlowCategory to display the workflow directory.|
|[ModifyFlowProjectClusterSetting](/intl.en-US/API Reference/Data development/ModifyFlowProjectClusterSetting.md)|Call the ModifyFlowProjectClusterSetting API to modify the cluster settings of a project.|
|[ModifyFlowCategory](/intl.en-US/API Reference/Data development/ModifyFlowCategory.md)|You can call this operation to rename a directory.|
|[ModifyFlowForWeb](/intl.en-US/API Reference/Data development/ModifyFlowForWeb.md)|Call the ModifyFlowForWeb interface to modify a workflow that contains graphic information.|
|[ModifyFlowJob](/intl.en-US/API Reference/Data development/ModifyFlowJob.md)|You can call this operation to modify a data development job.|
|[RerunFlow](/intl.en-US/API Reference/Data development/RerunFlow.md)|You can call this operation to rerun a workflow instance. Before you rerun a workflow instance, make sure that the workflow instance has ended.|
|[ResumeFlow](/intl.en-US/API Reference/Data development/ResumeFlow.md)|Call the ResumeFlow operation to resume a paused workflow.|
|[SubmitFlow](/intl.en-US/API Reference/Data development/SubmitFlow.md)|You can call this operation to submit a workflow.|
|[SubmitFlowJob](/intl.en-US/API Reference/Data development/SubmitFlowJob.md)|You can call this operation to submit a job. You can only run a job instance at a time.|
|[SuspendFlow](/intl.en-US/API Reference/Data development/SuspendFlow.md)|Call suspending flow to suspend a workflow.|
|[ModifyFlowProject](/intl.en-US/API Reference/Data development/ModifyFlowProject.md)|You can call this operation to modify ModifyFlowProject data development project.|
|[ListFlowClusterHost](/intl.en-US/API Reference/Data development/ListFlowClusterHost.md)|You can call this operation to query the list of submitable jobs of a cluster in a ListFlowClusterHost.|

