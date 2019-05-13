# Q&A about E-MapReduce {#concept_bw2_5s1_pfb .concept}

## Q: What is the difference between a job and an execution plan? {#section_ybr_zs1_pfb .section}

A: You need to perform two steps to run an EMR job:

-   Create a job

    An EMR job is essentially a set of configurations for running a job. An EMR job cannot be run directly. Instead, you need to specify the job JAR file, data input and output paths, and some run parameters in the configurations for running an EMR job. Provide a name for the set of configurations to complete the creation of a job. When you need to debug the job, an execution plan is required.

-   Create an execution plan

    An execution plan associates a job with a cluster. You can use an execution plan to run a sequence of jobs manually or schedule an execution plan to run the jobs periodically. You can choose a cluster for jobs to run using an execution plan. The cluster can be an on-demand cluster or an existing cluster. An on-demand cluster is released automatically after the execution of all jobs is completed. You can view the running status of each execution of an execution plan on the corresponding running log page.


## Q: How do I view the logs of jobs? {#section_cgv_jt1_pfb .section}

A: The EMR system uploads the logs of jobs to the OSS log path that you set when creating the cluster based on the job IDs. You can view logs of jobs in the EMR console. If you submit and run jobs on the master node, you need to go to the log path you set to view the logs.

## Q: How do I connect to a core node? {#section_bf4_nt1_pfb .section}

A: You need to perform the following steps.

1.  On the master node, switch to the hadoop user using the su command

    ```
    su hadoop
    ```

2.  Then you can connect to a core node using SSH without entering a password.

    ```
    ssh emr-worker-1
    ```

3.  Gain root privileges using the sudo command.

    ```
    sudo vi /etc/hosts
    ```


## Q: Can I view logs in the OSS console? {#section_nvk_xt1_pfb .section}

A: You can search for logs in the OSS console and download them. Viewing logs in the OSS console is not supported. The following example describes how to locate the logs of a job, assuming that you have already enabled the running logs feature and specified the log path in OSS. Assume that the log path is set to OSS://mybucket/emr/spark.

1.  Go to the execution plan page, click **Running log** for the execution plan, which contains the logs you want to view.
2.  On the running log page, find the execution record that you want. Click the corresponding cluster name in the **Execute cluster** column to view the cluster ID on the Details page.
3.  Locate OSS://mybucket/emr/spark/cluserID under the OSS://mybucket/emr/spark directory.
4.  Log files of jobs are stored in the corresponding folders that are created based on the job execution order ID under the OSS://mybucket/emr/spark/clusterID/jobs directory.

## Q: How is the running time calculated for a cluster, an execution plan, and a job? {#section_olm_s51_pfb .section}

A: The corresponding running time calculation policies are as follows.

-   For a cluster

    You can view the running time of each cluster on the cluster list. Formula: running time = completion time of cluster release - start time of cluster creation. Calculation of the running time starts when a cluster has been created and ends when the cluster has been released.

-   For an execution plan

    You can view the running time of each execution plan on the running log page. The running time calculation policy is based on the cluster that the execution plan runs on.

    -   If you choose a create-as-needed cluster for your execution plan, then each run of the execution plan involves creating a cluster, submitting jobs, and releasing the cluster. Formula: running time = consumption time for cluster creation + consumption time for running all jobs + consumption time for cluster release.
    -   If you choose an existing cluster for your execution plan, then cluster creation and cluster release will not be involved in the run cycle. Therefore, running time = consumption time for running all jobs.
-   For a job

    Specifically, we are referring to jobs that are included in an execution plan. For each running log of an execution plan, you can click View job list in the Operation column to see all jobs that are included in the corresponding execution plan. The running time calculation formula for each job is: running time = job execution completion time - start time of job execution. Job execution refers to jobs being scheduled to run on a Spark or Hadoop cluster.


## Q: Why are there no security groups available when running an execution plan for the first time? {#section_lzn_cv1_pfb .section}

A: For security reasons, you cannot use an existing ECS security group as an EMR security group. Therefore, if you have not created any EMR security groups, no security groups are available for an execution plan. We recommend that you manually create an on-demand cluster for job testing. Create an EMR security group when you manually create a cluster. After testing all jobs, create an execution plan to schedule jobs. At this point, existing ECS security groups are available for your execution plan to choose from.

## Q: Why "java.lang.RuntimeException.Parse responsed failed: ‘<!DOCTYPE html\>…’" is returned when I upload data to or download data from MaxCompute using Tunnel ? {#section_kdm_dv1_pfb .section}

A: Check whether the tunnel endpoint is correct. This error occurs when the tunnel endpoint is incorrect.

## \[DO NOT TRANSLATE\] {#section_az5_2v1_pfb .section}

\[DO NOT TRANSLATE\] \[DO NOT TRANSLATE\]

## Q: Can I view the logs of jobs, which are stored in worker nodes, in the EMR console? {#section_lq2_kv1_pfb .section}

A: Yes. Prerequisites: You have enabled the **Running log** feature when creating the cluster. Path to job logs: **Execution plan list** \> **More** \> **Running log** \> **Running record** \> **View job list** \> **job list** \> **job instance**

## Q: Why data cannot be retrieved using the external table created by Hive? {#section_ydh_cw1_pfb .section}

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
    No data has been retrieved using the external table.
```

This issue occurs because no partition directory is available for Hive to locate. To solve this problem, you can use ALTER TABLE ADD PARTITION to add partitions to the table. For example:

```
alter table storage_log add partition(ds=123);                                                                                                                                             OK
    Time taken: 0.137 seconds
    hive> select * from storage_log;
    OK
    abcd    123
    efgh    123
```

## Q: Why does a Spark Streaming job stop running unexpectedly? {#section_rqz_2w1_pfb .section}

A: First, check whether the Spark version is earlier than v 1.6. Spark v 1.6 has fixed a memory leak bug. This bug may cause a container to be terminated for exceeding memory limits, which is one probable cause of a Spark Streaming job terminating unexpectedly. Second, check whether your code has been optimized for effective memory usage.

## Q: Why does the EMR console show that a Sparking Streaming job is running when the job has already stopped? {#section_zj5_fw1_pfb .section}

A: We recommend that you change the running mode of the Spark Streaming job from yarn-client to yarn-cluster. EMR has problems monitoring the status of a Sparking Streaming job that runs in yarn-client mode. We will fix the problem as soon as we can.

## **Q: Why does the error message "error: could not find or load main class" appear?** {#section_wql_kw1_pfb .section}

A: Check whether the Class-Path header of the job JAR file is `ossref`. If not, modify it to `ossref`.

## Q: How do the master node and slave nodes work together? {#section_x13_nw1_pfb .section}

A: An EMR cluster consists of a single master node and multiple slave \(worker\) nodes. Only slave \(worker\) nodes store and process data. For example, a cluster consists of three instances. Each instance has four vCPUs and 8 GB of memory. One instance serves as the master node and the other two serve as slave nodes. Therefore, the available computing resources of this cluster are two instances \(the two slave nodes\), each with four vCPUs and 8 GB of memory.

## Q: How do I include local shared libraries in a MapReduce job? {#section_pcy_nw1_pfb .section}

A: You have multiple ways to achieve this. The following example describes one way. Modify the mapred-site.xml file. For example:

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

You only need to specify the path of the library that you want.

## Q: How can I specify the OSS data source file path for a MapReduce or Spark job? {#section_zrk_qw1_pfb .section}

A: The OSS data source path format is shown as follows: `oss://[accessKeyId:accessKeySecret@]bucket[.endpoint]/object/path`

You can use the URI format to specify input and output OSS data sources for a job. Similarly, when the data sources are in HDFS, the corresponding URI starts with `hdfs://`. You can access OSS data with or without the AccessKey.

-   \(Recommended\) EMR provides MetaService, which allows you to access OSS data without an AccessKey so that you can specify the data source using the oss://bucket/object/path path format.
-   \(Not recommended\) You can set the AccessKeyId, AccessKeySecret, and endpoint parameters on the Configuration object for a MapReduce job \(for a Spark job, set these parameters on the SparkConf object\). Or you can include the AccessKeyId, the AccessKeySecret, and the endpoint in a URI directly. For more information, see the [Development preparation](../../../../reseller.en-US/Developer Guide/Preparations/Configure the OSS URI to use E-MapReduce.md#) section.

## Q: Why does Spark SQL return an error message "Exception in thread "main" java.sql.SQLException: No suitable driver has been found for jdbc:mysql:xxx"? {#section_sy1_ww1_pfb .section}

A:

-   This error may occur when you use earlier versions of mysql-connector-java. Use the latest version of mysql-connector-java.
-   In the job parameters, use —driver-class-path ossref://bucket/…/mysql-connector-java-\[version\].jar to load mysql-connector-java package. This issue may also occur if you directly package mysql-connector-javainto the Job JAR file.

## Q: Why is the error message "Invalid authorization specification, message from server: ip not in whitelist" returned when Spark SQL connects to ApsaraDB for RDS? {#section_uw3_zw1_pfb .section}

A: Include the internal IP addresses of the cluster nodes in the whitelist of ApsaraDB for RDS.

## Q: What do I need to consider when creating a cluster of low-specification nodes? {#section_vgj_1x1_pfb .section}

A:

-   If you choose an instance with two vCPUs and 4 GB of memory as the master node, then the master node is prone to running out of memory. We recommend that you increase the memory of the master node.
-   If you choose an instance with two vCPUs and 4 GB of memory as a slave \(worker\) node, set the parameters as follows when running a MapReduce or Hive job. For a MapReduce job, set the yarn.app.mapreduce.am.resource.mb parameter to 1024. For a Hive job, set the yarn.app.mapreduce.am.resource.mb parameter to 1024. This step is to prevent a job from being suspended due to an OOM error.

## Q: Why is the error message "Failed with exception java.io.IOException:org.apache.parquet.io.ParquetDecodingException: Can not read value at 0 in block -1 in file hdfs://…/…/part-00000-xxx.snappy.parquet" returned when reading Parquet data \(including columns of the decimal type\) written by Spark SQL using Hive or Impala ? {#section_m52_hx1_pfb .section}

A: The decimal type has different representations in the different Parquet conventions used in Hive/Impala and Spark SQL. Therefore, Parquet data \(including columns of the decimal type\) written by Spark SQL cannot be read properly using Hive or Impala. To solve this issue, we recommend that you set the spark.sql.parquet.writeLegacyFormat parameter to true \(this setting makes Spark use the same convention as Hive/Impala for writing the Parquet data\) before importing the Parquet data written by Spark SQL to Hive or Impala.

## Q: How do I connect to Kerberos-authenticated clusters using Beeline? {#section_y2c_2gp_wfb .section}

A:

-   High-availability cluster \(service discovery mode\)

    ```
    
    ! connect jdbc:hive2://emr-header-1:2181,emr-header-2:2181,emr-header-3:2181/;serviceDiscoveryMode=zooKeeper;zooKeeperNamespace=hiveserver2;principal=hive/_HOST@EMR.${clusterId). COM
    ```

-   High-availability cluster \(directly connecting to a node\)

    Connect to the emr-header-1 node.

    ```
    ! connect jdbc:hive2://emr-header-1:10000/;principal=hive/emr-header-1@EMR.${clusterId}. COM
    ```

    Connect to the emr-header-2 node.

    ```
    ! connect jdbc:hive2://emr-header-2:10000/;principal=hive/emr-header-2@EMR.${clusterId}. COM
    ```

-   Non-HA cluster

    ```
    ! connect jdbc:hive2://emr-header-1:10000/;principal=hive/emr-header-1@EMR.${clusterId}. COM
    ```


## Q: Why do I receive a "Connection refused telnet emr-header-1 10001" error message? {#section_o55_2yd_ggb .section}

A:

You can view logs in the /mnt/disk1/log/spark directory.

This issue is caused by the Thrift Server running out of memory \(OOM\). You need to increase memory by raising the value of the spark.driver.memory parameter

.

