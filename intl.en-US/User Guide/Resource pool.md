# Resource pool {#concept_gny_rlj_z2b .concept}

The Dynamic Resource Pools function is a usage strategy for the Yarn application. E-MapReduce Yarn uses the capacity scheduler by default. EMR Yarn uses the capacity scheduler by default.

## Enable Resource Pool {#section_u1h_xlj_z2b .section}

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://emr.console.aliyun.com/console) and enter the cluster list page.
2.  Click the **Manage** link corresponding to the cluster you want to configure.
3.  In the service list, click **YARN**, and go to the Yarn configuration page.
4.  At the top of the page, click the **Resource Pool** tab to go to the resource pool configuration page.
5.  Click **Initialize Resource Pool** button to select a scheduler policy. E-MapReduce supports [Capacity Scheduler](http://hadoop.apache.org/docs/r2.7.3/hadoop-yarn/hadoop-yarn-site/CapacityScheduler.html) and [Fair Scheduler](http://hadoop.apache.org/docs/r2.7.3/hadoop-yarn/hadoop-yarn-site/FairScheduler.html).

    **Note:** With the Resource Pool function enabled, once the Capacity Scheduler or Fair Scheduler is selected, it cannot be changed later. You can just configure relevant parameters in the resource pool.


## Configure Resource Pool {#section_tr1_dmj_z2b .section}

When you select the scheduler policy, click the drop-down list of **More settings** for the global configuration. It is recommended that the system administrator set these configurations.

-   **Capacity Scheduler**

    |EMR parameters|Hadoop Yarn parameters|
    |:-------------|:---------------------|
    |Default configuration-maximum number of applications|Configure global parameters yarn.scheduler.capacity.maximum-applications or yarn.scheduler.capacity.<queue-path\>.maximum-applications|
    |Default configuration-maximum am ratio|Configure global parameters yarn.scheduler.capacity.maximum-am-resource-percent or yarn.scheduler.capacity.<queue-path\>.maximum-am-resource-percent|
    |Default configuration-resources calculation class|yarn.scheduler.capacity.resource-calculator|
    |Default configuration-number of node delays|yarn.scheduler.capacity.node-locality-delay|
    |Placement rules|Configure mappings between users/groups and queue|
    |Placement rules: queue mapping override|yarn.scheduler.capacity.queue-mappings-override.enable|
    |ACL settings-enable ResourceManager ACL|yarn.acl.enable on the yarn-site|
    |ACL settings-manage ACL|yarn.admin.acl on the yarn-site|
    |User limit-acl\_submit\_applications|Configure global parameter yarn.scheduler.capacity.root.<queue-path\>.acl\_submit\_applications|
    |User limit-acl\_administer\_queue|Configure global parameter yarn.scheduler.capacity.root.<queue-path\>.acl\_administer\_queue|

    |EMR parameters|Hadoop Yarn parameters|
    |:-------------|:---------------------|
    |Resource Pool name|Queue name in yarn|
    |Resource pool limit-proportion|yarn.scheduler.capacity.<queue-path\>.capacity|
    |Resource pool limit-miniUserLimit|yarn.scheduler.capacity.<queue-path\>.minimum-user-limit-percent|
    |Resource pool limit-maximumCapacity|yarn.scheduler.capacity.<queue-path\>.maximum-capacity|
    |Resource pool limit-limit on the proportion of individual users|yarn.scheduler.capacity.<queue-path\>.user-limit-factor|
    |Resource pool limit-maximum memory|yarn.scheduler.capacity.<queue-path\>.maximum-allocation-mb|
    |Resource pool limit-maximum number of cores|yarn.scheduler.capacity.<queue-path\>.maximum-allocation-vcores|
    |Resource pool limit-maximum number of applications|The priority of the parameter is higher than the global variable yarn.scheduler.capacity.maximum-applications / yarn.scheduler.capacity.<queue-path\>.maximum-applications|
    |Resource pool limit-maximum AM resource percentage|The priority of the parameter is higher than the global variable yarn.scheduler.capacity.maximum-am-resource-percent or yarn.scheduler.capacity.<queue-path\>.maximum-am-resource-percent|
    |Submit permission control|The priority of the parameter is higher than the global variable yarn.scheduler.capacity.root.<queue-path\>.acl\_submit\_applications|
    |Manage access control policies|The priority of the parameter is higher than the global variable yarn.scheduler.capacity.root.<queue-path\>.acl\_administer\_queue|

-   **Fair Scheduler**

    |EMR parameters|Hadoop Yarn parameters|
    |:-------------|:---------------------|
    |ACL settings-enable ResourceManager ACL|yarn.acl.enable on the yarn-site|
    |ACL settings-manage ACL|yarn.admin.acl on the yarn-site|

    |EMR parameters|Hadoop Yarn parameters|
    |:-------------|:---------------------|
    |Enable the preemption mode|yarn.scheduler.fair.preemption|
    |Resource pool name|Queue name in yarn|
    |Preemption-enable the preemption mode|yarn.scheduler.fair.preemption|
    |Preemption-fair share preemption threshold|yarn.scheduler.fair.preemption.cluster-utilization-threshold|

    After the resource pool configuration is complete, click **Synchronize Configuration to Cluster** to make the configuration take effect.


## Disable Resource Pool {#section_glp_vmj_z2b .section}

If you want to configure the resource pool directly through XML, click **Disable Resource Pool** button first in the **Resource Pool** page, and then go to the **Configuration** page of YARN.

