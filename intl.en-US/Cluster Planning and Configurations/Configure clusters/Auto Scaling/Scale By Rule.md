# Scale By Rule {#concept_ibh_bkw_tgb .concept}

When you use an EMR Hadoop cluster, choose Scale By Rule as the rule type if you cannot predict the peak or valley periods for big data computing.

## Configure the number of instances in a scaling group {#section_lbx_xnw_tgb .section}

-   Maximum Instances: The maximum number of task nodes in a scaling group. Once the maximum number is exceeded, auto scaling will not be performed even if the scaling rule is met. Currently, the maximum number of instances that you can set is 1,000.
-   Minimum Instances: The minimum number of task nodes of a scaling group.

## Configure a scaling rule {#section_inn_b4w_tgb .section}

Scaling rules include scaling out rules and scaling in rules. After you disable the auto scaling feature for a cluster, all rules are deleted. You need to configure scaling rules again when you reactivate the auto scaling feature. After you switch the rule type, for example, switching from Scale by Rule to Scale By Time , the scaling rules based on the original rule type are canceled and are not to be triggered. Instances that have been added for scaling out are not to be released.

-   Rule Name: A rule name is required to be unique for each cluster. This requirement applies to rules both for scaling out and scaling in.
-   Cluster Metric: Load metrics of Hadoop YARN. For more information, see [Hadoop documentation](https://hadoop.apache.org/docs/r2.7.2/hadoop-yarn/hadoop-yarn-site/ResourceManagerRest.html#Cluster_Metrics_API).

    |E-MapReduce Auto Scaling Metrics|YARN Metrics|Description|
    |--------------------------------|------------|-----------|
    |YARN.AvailableVCores|availableVirtualCores|The number of available virtual cores|
    |YARN.PendingVCores|pendingVirtualCores|The number of pending virtual cores，EMR collects supplementary metrics.|
    |YARN.AllocatedVCores|allocatedVirtualCores|The number of allocated virtual cores|
    |YARN.ReservedVCores|reservedVirtualCores|The number of reserved virtual cores|
    |YARN.AvailableMemory|availableMB|The amount of memory available in MB|
    |YARN.PendingMemory|pendingMB|The amount of memory pending in MB，EMR collects supplementary metrics.|
    |YARN.AllocatedMemory|allocatedMB|The amount of memory allocated in MB|
    |YARN.ReservedMemory|reservedMB|The amount of memory reserved in MB|
    |YARN.AppsRuning|appsRunning|The number of applications running|
    |YARN.AppsPending|appsPending|The number of applications pending|
    |YARN.AppsKilled|appsKilled|The number of applications killed|
    |YARN.AppsFailed|appsFailed|The number of applications failed|
    |YARN.AppsCompleted|appsCompleted|The number of applications completed|
    |YARN.AppsSubmitted|appsSubmitted|The number of applications submitted|
    |YARN.AllocatedContainers|containersAllocated|The number of containers allocated|
    |YARN.PendingContainers|containersPending|The number of containers pending|
    |YARN.ReservedContainers|containersReserved|The number of containers reserved|

-   Evaluation Period and Scaling Condition: Once the aggregate of the Cluster Metric's values meets the scaling condition during an evaluation period, the number of occurrences increases by one.
-   Occurrences: Auto scaling is performed when the number of occurrences reaches the set value of Occurrences.
-   Add/Remove Nodes: The number of task nodes to add or to remove each time a cluster is scaled.
-   Cooldown \(Seconds\): The minimum interval time that is allowed between auto scaling. Auto-scaling is not to be performed during a cooldown even if the scaling condition is met.

## Configure the specifications of nodes {#section_zlz_1qw_tgb .section}

You can specify the hardware specification of the nodes that are used to scale in or scale out a cluster. Click Enable Auto-scaling to configure the specifications. You cannot modify the configurations after clicking Save. For applying new configurations, click Disable Auto-scaling and click Enable Auto-scaling again.

-   Instance types are displayed in the Available Instances list based on the specifications of the vCPU and memory you select for a node. Select the instance types that you want from the Available Instances list and move the instance types to the Selected Instances list on the right. By doing this, you have confirmed the instance types used to scale your cluster.
-   You can select a maximum of three instance types. This helps to prevent auto scaling failures due to insufficient ECS resources.
-   The minimum size of a data disk is 40 GB, and it does not matter whether you choose the Ultra Disk or the SSD Disk as the data disk type.

