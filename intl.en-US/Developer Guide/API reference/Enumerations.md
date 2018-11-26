# Enumerations {#concept_mzn_c5b_kfb .concept}

Enumerations

## Cluster {#section_tzd_y5b_kfb .section}

-   createType \(the type of the cluster creation\):

    |Enumerator|Description|
    |----------|-----------|
    |ON-DEMAND|Creates a cluster on demand.|
    |MANUAL|Creates a cluster manually.|

-   clusterType \(The type of the cluster\):

    |Enumerator|Description|
    |----------|-----------|
    |HADOOP|Apache Hadoop.|
    |HBASE|Apache HBase.|

-   clusterStatus \(the status of the cluster\):

    |Enumerator|Description|
    |----------|-----------|
    |CREATING|The cluster is being created.|
    |CREATE\_FAILED|The cluster fails to be created.|
    |RUNNING|The cluster is running.|
    |IDLE|The cluster is idle.|
    |RELEASING|The cluster is being released.|
    |RELEASE\_FAILED|The cluster fails to be released.|
    |RELEASED|The cluster has been released.|
    |WAIT\_FOR\_PAY|The cluster has not been paid.|
    |ABNORMAL|The status of the cluster is abnormal.|

-   DiskType \(the type of the disk\):

    |Enumerator|Description|
    |----------|-----------|
    |CLOUD|Cloud disk.|
    |CLOUD\_EFFICIENCY|Ultra disk.|
    |CLOUD\_SSD|SSD disk.|

-   ecs instance status

    |Enumerator|Description|
    |----------|-----------|
    |ABNORMAL|The status of the instance is abnormal.|
    |NORMAL|The status of the instance is normal.|
    |RESIZING|The instance is being resized.|
    |INITIALIZING|The instance is being initialized.|
    |RELEASED|The instance has been released.|


## Execution plan {#section_j43_z5b_kfb .section}

-   executionPlan Instance Status \(the status of the execution plan instance\):

    |Enumerator|Description|
    |----------|-----------|
    |WF\_INSTNACE\_READY|The instance is not running.|
    |WF\_INSTNACE\_RUNNING|The instance is running.|
    |WF\_INSTNACE\_FAILED|The instance fails to run.|
    |WF\_INSTNACE\_SUCCESS|The instance runs successfully.|

-   executionPlan status \(the status of the execution plan\):

    |Enumerator|Description|
    |----------|-----------|
    |SCHEDULING|The execution plan is being scheduled.|
    |NO\_SCHEDULE|The execution plan has not been scheduled. Either the scheduling of the execution plan is paused or the execution plan is executed manually.|

-   Strategy

    |Enumerator|Description|
    |----------|-----------|
    |RUN\_MANUALLY|Manual execution.|
    |SCHEDULE|Periodic scheduling.|

-   timeUnit \(the unit of the time interval for periodic scheduling\)

    |Enumerator|Description|
    |----------|-----------|
    |DAY|The day.|
    |HOUR|The hour.|


## Job {#section_o34_jvb_kfb .section}

-   jobType \(the type of the job\):

    |Enumerator|Description|
    |----------|-----------|
    |SPARK|A Spark job.|
    |HADOOP|A Hadoop MapReduce job.|
    |HIVE|A Hive job.|
    |PIG|A PIG job.|

-   jobFailAct \(The action to take after failure\):

    |Enumerator|Description|
    |----------|-----------|
    |STOP|Stops running the job.|
    |CONTINUE|Continues to run the subsequent jobs.|

-   job instance status

    |Enumerator|Description|
    |----------|-----------|
    |JOB\_INSTANCE\_WAITING|The instance is waiting to run.|
    |JOB\_INSTANCE\_SUBMISSION\_FAILED|The instance fails to be submitted.|
    |JOB\_INSTANCE\_RUNNING|The instance is running.|
    |JOB\_INSTANCE\_SUCCESS|The instance runs successfully.|
    |JOB\_INSTANCE\_FAILED|The instance fails to run.|
    |JOB\_INSTANCE\_CANCELING|The instance is being canceled.|
    |JOB\_INSTANCE\_CANCELED|The instance has been canceled.|
    |JOB\_INSTANCE\_CANCEL\_FAILED|The instance fails to be canceled.|


