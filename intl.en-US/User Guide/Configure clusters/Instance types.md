# Instance types {#concept_dfm_yk3_y2b .concept}

The EMR cluster consists of multiple different node instance types, namely the master, core, and task instances. Completely different service processes are available for different tasks when each of these instances is deployed. For example, we deploy Hadoop HDFS’s Name Node service and Hadoop YARN’s Resource Manager service on master instances, and Data Node service and Hadoop YARN’s Node Manager service on core instances. Task instances are only used for computing. Therefore, we deploy Hadoop YARN’s Node Manager service, rather than HDFS-related services for task instances.

When creating a cluster, you must determine ECS specifications for these three instance types. The ECS instances with the same instance type must be in the same instance group. The cluster can scale up at a later stage to accommodate the number of hosts in the appropriate instance groups \(except master instance group\).

**Note:** The task instance is supported in version 3.2.0 or later.

## Master instance {#section_xwh_1l3_y2b .section}

The master instance is the node where the cluster service’s management and control components are deployed. For example, the Hadoop YARN’s Resource Manager is deployed on the master instance node. You can connect to the master instance using SSH, and check the service status in the cluster by the Web UI of the software. At the same time, when you want to quickly test or run a job, you can log on to the master instance and submit jobs directly by command lines. When the high availability feature is turned on for the cluster, two master instance nodes are used \(by default, only one\).

## Core instance {#section_ywh_1l3_y2b .section}

The core instance is the instance node managed by the master instance. It runs the Hadoop HDFS’s Data Node service and stores all the data. It also deploys computing services, such as Hadoop YARN’s Node Manager service, to perform computing tasks. To meet the needs for more data storage or heavier computing workload, the core instance can scale up at any time without affecting the normal operations of the active cluster. It can use a variety of different storage media to store data. You can refer to the discussions about disks for details.

## Task instance {#section_zwh_1l3_y2b .section}

The task instance is an optional instance type that is specifically responsible for computing. If the core instance has sufficient computing power, task instance may not be used. The task instance can quickly add computing power to the cluster, such as Hadoop’s MapReduce tasks, and Spark executors. As HDFS data is not stored on the task instance, Hadoop HDFS’s Data Node service does not run on it. The task instance can scale up and down at any time without affecting the normal operations of the active cluster. Depending on the fault tolerance \(or retries\) of the computing service, fewer task instance nodes may cause MapReduce and Spark jobs to fail.

