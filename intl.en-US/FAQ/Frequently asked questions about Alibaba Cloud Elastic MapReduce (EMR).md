# Frequently asked questions about Alibaba Cloud Elastic MapReduce \(EMR\) {#concept_bw2_5s1_pfb .concept}

## Q: What is the difference between a job and an execution plan? {#section_ybr_zs1_pfb .section}

A: In EMR, two steps are required to run a job:

-   Create a job

    In EMR, creating a “job” creates a "job running configuration" which cannot be run directly. If you have created a "job" in EMR, you have created a "configuration about how to run the job." The configuration contains the Job JAR to be run for the job, the input and output addresses of data and certain running parameters. After you create such a configuration and provide a name for it, a job is defined. However, when you want to debug the running job, the execution plan is required.

-   Create an execution plan

    The execution plan associates the job with the cluster. Through the execution plan, we can combine multiple jobs into a job sequence and prepare a running cluster for the job. The execution can also automatically create a temporary cluster or associate the job with an existing cluster. The execution plan also helps to configure a periodical execution plan for the job sequence and automatically releases the cluster after the task is completed. You can also view the execution status and log entries on the execution record list.


## Q: How do I view the job log entries? {#section_cgv_jt1_pfb .section}

A: In the EMR system, the system has uploaded the job running log entries to OSS by jobid. The path is set by you when you create the cluster. You can view the job log entries on the webpage. If you log on to the master node to submit jobs or run scripts, the logs can be determined by your script.

## Q: How do I log on to the core node? {#section_bf4_nt1_pfb .section}

A: Use the following steps:

1.  Switch to the hadoop account on the master node.

    ```
    su hadoop
    ```

2.  Log on to the corresponding core node with password-free SSH authentication.

    ```
    ssh emr-worker-1
    ```

3.  Get root privileges through the sudo command.

    ```
    sudo vi /etc/hosts
    ```


## Q: How to view logs on OSS? {#section_nvk_xt1_pfb .section}

A: users can also find all the log files directly from the OSS and download them. However, since OSS does not directly view the logs, it can be much more difficult to use it. If you have enabled logging and specified a log location for OSS, how can you find the job log entries? For example, this path of OSS://mybucket/emr/spark.

1.  1. Go to the execution plan page, find the corresponding execution plan and click **View Records** to enter the execution log page.
2.  2. Find the specific execution log entry on the execution log page, such as the last execution log entry. Click the corresponding **Execution Cluster** to view the ID of the execution cluster.
3.  3. Then search for the cluster ID directory OSS://mybucket/emr/spark/under OSS://mybucket/emr/spark directory.
4.  4. There will be multiple directories under OSS://mybucket/emr/spark/cluster ID/jobs based on the execution ID of the job, and each directory stores the running log file of the job.

## Q: What is the timing policy of the cluster, execution plan, and running job? {#section_olm_s51_pfb .section}

A: Three following timing policies are available:

-   The timing policy of the cluster

    In the cluster list, you can see the running time of every cluster. The formula for calculating the running time is: Running time = Time when the cluster is released - Time when the cluster is established. Once a cluster has been created, the timing starts until the end of the life cycle of the cluster.

-   The timing policy of the execution plan

    In the running log list of the execution plan, you can see the running time of every execution plan, and the timing policy can be summarized into two situations:

    -   1. If the execution plan is executed on demand, the running process of every execution log involves cluster creation, job submission, job running, and cluster release. The formula for calculating the on-demand execution plan is: Running time = The time when the cluster is created + The total time used for running all the jobs in the execution plan + The time when the cluster is released.
    -   2. If the execution plan is associated with an existing cluster, the entire execution cycle does not involve the creation and release of a cluster. The running time is the total time used for running all the jobs in the execution plan.
-   The timing policy of the running job

    The specified job refers to the job assigned to the execution plan. Click the View Job List to the right of the running log of every execution plan to view the job. The formula for calculating the runtime of every job is: Running time = The actual time when the job stops running - The actual time when the job starts running. The specified time when the job starts or stops refers to the time when the job is scheduled to run or stopped running by the Spark or Hadoop cluster.


## Q: Why are there no security groups available the first time I run an execution plan? {#section_lzn_cv1_pfb .section}

A: For some security reasons, you cannot directly use an existing security group as an EMR security group. So you are not able to select an available security group if you have not created a security group in EMR We recommend that you create an on-demand cluster for testing. You can create a new EMR security group when creating the cluster. You can set up a scheduling cycle to schedule execution plans after the test is passed. The security groups that you have previously created are also available.

## Q: The error message "java.lang.RuntimeException.Parse responsed failed: ‘<!DOCTYPE html\>…’" is returned when reading and writing MaxCompute . {#section_kdm_dv1_pfb .section}

A: Check whether the odps tunnel endpoint is correct. This error occurs if it is wrong.

## Q: TPS conflict occurs when multiple consumer IDs consume the same topic. {#section_az5_2v1_pfb .section}

A: This topic may have been created in the beta test or other environments, causing the consumption data of certain consumer groups conflicted. Report the corresponding topic and consumer ID to ONS for processing by submitting a ticket.

## Q: Can I view job log entries on the worker nodes in EMR? {#section_lq2_kv1_pfb .section}

A: Yes. Prerequisite: Click **Save Log** when creating a cluster. How to view the log entries: Choose **Execution plan list** \> **More** \> **Running log** \> **Running log** \> **View job list** \> **Job list** \> **workers log**.

## Q: Why do the external tables created in Hive contain no data? {#section_ydh_cw1_pfb .section}

A: For example:

```
CREATE EXTERNAL TABLE storage_log(content STRING) PARTITIONED BY (ds STRING)
    ROW FORMAT DELIMITED
    FIELDS TERMINATED BY '\t'
    STORED AS TEXTFILE
    LOCATION 'oss://log-124531712/biz-logs/airtake/pro/storage'; 
    hive> select * from storage_log;
    OK
    Time taken: 0.3 seconds
    The external table that you have created contains no data.
```

Hive does not automatically bind itself with the partition directory of the specified directory. You need to bind them manually, for example:

```
alter table storage_log add partition(ds=123);                                                                                                                                             OK
    Time taken: 0.137 seconds
    hive> select * from storage_log;
    OK
    abcd    123
    efgh    123
```

## Q: Why does the Spark Streaming job stop with no specified reason? {#section_rqz_2w1_pfb .section}

A: First, check whether the current Spark version is earlier than Version 1.6. Spark Version 1.6 repaired a memory leak bug. This bug can cause container memory overuse, which terminates jobs. This error may be one of the causes and this does not mean that Spark 1.6 does not have any issues. In addition, you must check your code to optimize memory usage.

## Q: Why is the job still in the running status in the EMR console when the Spark Streaming job has ended? {#section_zj5_fw1_pfb .section}

A: Check whether the running mode of the Spark Streaming job is yarn-client. If yes, set it to yarn-cluster. Errors occur in EMR when it is monitoring the status of Spark Streaming jobs that are in the yarn-client mode. This issue will be repaired as soon as possible.

## **Q: Why does the error message "error: could not find or load main class" return?** {#section_wql_kw1_pfb .section}

A: Check whether the path protocol header of the Job Jar is `ossref` in the job configuration. If not, change it to `ossref`.

## Q: How are machines in a cluster responsible for different tasks? {#section_x13_nw1_pfb .section}

A: The EMR contains a master node and multiple slave or worker nodes. The master node does not perform data storage or computing tasks. The slave node is used for data storage and computing tasks. For example, in a cluster with three four-core 8 GB machines, one of the machines serves as the master node and the other two serve as the slave nodes. As a result, the available computing resources of the cluster are two four-core 8 GB machines.

## Q: How to use the local shared library in MR jobs? {#section_pcy_nw1_pfb .section}

A: You can use multiple methods, including the following example: Modify the mapred-site.xml file, for example:

```
<property>  
    <name>mapred.child.java.opts</name>  
    <value>-Xmx1024m -Djava.library.path=/usr/local/share/</value>  
  </property>  
  <property>  
    <name>mapreduce.admin.user.env</name>  
    <value>LD_LIBRARY_PATH=$HADOOP_COMMON_HOME/lib/native:/usr/local/lib</value>  
  </property>
```

Add the library file as needed and you can use the local shared library.

## Q: How can I specify the OSS data source file path in the MR or Spark job? {#section_zrk_qw1_pfb .section}

A: You can use the following OSS URL: `oss://[accessKeyId:accessKeySecret@]bucket[.endpoint]/object/path`.

This URL is used for specifying input/output data sources in the job, and is similar to `hdfs://`. Follow this procedure when you perform operations on OSS data:

-   \(Recommended\) EMR provides MetaService, which allows you to access OSS data without AssessKey, and directly write to the OSS path: // bucket/Object/path.
-   \(Not recommended\) You can configure AccessKeyId, AccessKeySecret, and endpoint to Configuration \(SparkConf in Spark jobs, Configuration in MR jobs\), or you can directly specify AccessKeyId, AccessKeySecret, and endpoint in the URL. For more information, see the [Development preparation](../../../../../intl.en-US/Developer Guide/Preparations/Configure the OSS URI to use E-MapReduce.md#) section.

## Q: Why does Spark SQL return an error message "Exception in thread "main" java.sql.SQLException: No suitable driver has been found for jdbc:mysql:xxx"? {#section_sy1_ww1_pfb .section}

A:

-   1. Earlier versions of mysql-connector-java may encounter such issues. Update mysql-connector-java to the latest version.
-   2. In the job parameters, use—driver-class-path ossref://bucket/…/mysql-connector-java-\[version\].jar to load mysql-connector-java package. This issue may also occur if you directly package mysql-connector-javainto the Job Jar.

## Q: Why is the error message "Invalid authorization specification, message from server: ip not in whitelist" returned when Spark SQL is connected with RDS? {#section_uw3_zw1_pfb .section}

A: Check the RDS whitelist settings and add the internal network IP addresses of the cluster machines to the RDS whitelist.

## Q: Notes on creating a cluster based on low-configuration machines. {#section_vgj_1x1_pfb .section}

A:

-   If you choose two-core 4 GB machines for the master node, the memory of the master node is heavily loaded. This may cause insufficient memory issues. We recommend that you expand the memory capacity of the master node.
-   If you choose two-core 4 GB machines for the slave nodes, adjust the parameters when you run MR or Hive jobs. For MR jobs, add the -D yarn.app.mapreduce.am.resource.mb=1024 parameter. For Hive jobs, add the set yarn.app.mapreduce.am.resource.mb=1024 parameter. This can prevent jobs to be suspended.

## Q: Why is the error message "Failed with exception java.io.IOException:org.apache.parquet.io.ParquetDecodingException: Can not read value at 0 in block -1 in file hdfs://…/…/part-00000-xxx.snappy.parquet" returned when Hive or Impala jobs reads Parquet tables that are imported by Spark SQL ? {#section_m52_hx1_pfb .section}

A: Hive and Spark SQL use different conversion methods on decimal types to write Parquet. This may cause Hive to fail to correctly read the data imported by Spark SQL. If Hive or Impala needs to use data that has been imported using Spark SQL, we recommend that you add the spark.sql.parquet.writeLegacyFormat=true parameter and then import data again.

## Q: How does Beeline access Kerberos security clusters? {#section_y2c_2gp_wfb .section}

A:

-   HA cluster \(discovery mode\)

    ```
    
    ! connect jdbc:hive2://emr-header-1:2181,emr-header-2:2181,emr-header-3:2181/;serviceDiscoveryMode=zooKeeper;zooKeeperNamespace=hiveserver2;principal=hive/_HOST@EMR.${clusterId). COM
    ```

-   HA cluster \(directly connected to a machine\)

    Connect to emr-header-1.

    ```
    ! connect jdbc:hive2://emr-header-1:10000/;principal=hive/emr-header-1@EMR.${clusterId}. COM
    ```

    Connect to emr-header-2.

    ```
    ! connect jdbc:hive2://emr-header-2:10000/;principal=hive/emr-header-2@EMR.${clusterId}. COM
    ```

-   Non-HA cluster

    ```
    ! connect jdbc:hive2://emr-header-1:10000/;principal=hive/emr-header-1@EMR.${clusterId}. COM
    ```


