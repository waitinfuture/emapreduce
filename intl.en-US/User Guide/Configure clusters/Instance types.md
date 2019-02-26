# Instance types {#concept_dfm_yk3_y2b .concept}

There are three types of node instances in an E-MapReduce cluster: master, core, and task.

Different service processes are deployed on each instance type. For example, with Hadoop, the HDFS NameNode and YARN ResourceManager services are deployed on master instances, while the HDFS DataNode and YARN NodeManager services are deployed on core instances. For task instances, because they are only used in computing tasks, only YARN NodeManager is deployed, and not HDFS-related services.

When you create a cluster, you must determine the ECS specifications for each instance type. ECS instances of the same type must be in the same instance group. If you increase the number of hosts in a core or task instance group, you can scale the cluster up at a later date. This does not apply to master instance groups.

**Note:** Task instances are supported in version 3.2.0 or later.

## Master instance {#section_xwh_1l3_y2b .section}

The master instance is where the management and control components of the cluster service are deployed. You can connect to the master instance using SSH and check service statuses in the cluster through the software's Web UI.

If you want to perform a test or run a job, log on to the master instance and submit jobs directly at the command line. By default, only one master instance is used. However, if the cluster's high availability feature is enabled, two are used.

## Core instance {#section_ywh_1l3_y2b .section}

Core instances, which are managed by master instances, store all of the data in the cluster. They also deploy computing services to perform computing tasks. If you need more data storage or are experiencing heavier workloads, you can scale core instances up at any time without impacting the operations of the cluster. For more information, please refer to [Local disks](../../../../../reseller.en-US/Block storage/Block storage/Local disks.md#) and [Block storage?](../../../../../reseller.en-US/Block storage/Block storage/What is block storage?.md#).

## Task instance {#section_zwh_1l3_y2b .section}

Task instances are responsible for computing and can quickly add computing power to a cluster. They can also scale up and down at any time without impacting the operations of the cluster. However, this instance type is optional, and if the core instance has enough computing power, task instances are not necessary. Depending on the fault tolerance \(or retries\) of the computing service, a reduction in the number of task instance nodes may cause MapReduce and Spark jobs to fail.

