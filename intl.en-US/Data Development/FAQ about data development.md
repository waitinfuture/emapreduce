# FAQ about data development

This topic provides answers to some commonly asked questions about data development.

Questions about jobs:

-   [What are the differences between jobs and workflows?](#section_2l7_z5g_ick)
-   [What do I do if a TPS conflict occurs when multiple consumer IDs consume the same topic?](#section_75h_m1v_03k)
-   [Why does an external table created in Hive contain no data?](#section_wd2_vqs_sf8)
-   [Why does a Spark Streaming job stop running unexpectedly?](#section_3fs_d2r_p5z)
-   [Why is a Spark Streaming job still in the Running state in the EMR console after the job has been stopped?](#section_dgg_qry_5j5)
-   [How do I include local shared libraries in a MapReduce job?](#section_dki_3fc_y6w)
-   [How do I specify the file path of an OSS data source for a MapReduce or Spark job?](#section_3xs_fs7_l9f)
-   [How do I use Beeline to connect to Kerberos-authenticated clusters?](#section_c24_9y1_ryi)
-   [What do I do if an out-of-memory error is reported when Spark receives Flume data?](#section_v51_ttd_35g)
-   [Why does a job run slowly?](#section_otz_ijt_nuo)
-   [Why does AppMaster take a long time to start a task?](#section_8fu_qzc_ufj)
-   [What do I do if the timestamp field shows a delay of eight hours when I import data from ApsaraDB for RDS into EMR?](#section_ltj_c3f_d1c)

Questions about logs:

-   [How do I view job logs?](#section_gvz_h23_mvz)
-   [How do I view logs on OSS?](#section_902_cv6_jek)
-   [Can I view the job logs stored in core nodes?](#section_7ua_xld_ugc)
-   [How do I view the logs of a service deployed in an EMR cluster?](#section_80h_78b_58u)
-   [How do I clear the log data of a completed job?](#section_msg_i48_dc5)

Questions about exception diagnosis:

-   [How do I fix the error "Error: Could not find or load main class"?](#section_ykc_q6d_50d)
-   [What do I do if the error "Invalid authorization specification, message from server: ip not in whitelist" is reported when I connect Spark SQL to ApsaraDB for RDS?](#section_2vz_guj_luo)
-   [What do I do if the following error is reported when I read or write data from or to MaxCompute tables: "java.lang.RuntimeException.Parse response failed: '<! DOCTYPE html\>…'"?](#section_gqw_ros_mtx)
-   [What do I do if the error "Exception in thread "main" java.sql.SQLException: No suitable driver found for jdbc:mysql:xxx" is reported when a Spark SQL job runs?](#section_tpq_it1_72p)
-   [What do I do if the following error is reported when I run a Hive or Impala job to read Parquet data \(including columns of the DECIMAL type\) written by Spark SQL: "Failed with exception java.io.IOException:org.apache.parquet.io.ParquetDecodingException: Can not read value at 0 in block -1 in file hdfs://… /…/part-00000-xxx.snappy.parquet"?](#section_anw_uga_77a)
-   [What do I do if the Thrift Server process properly runs but the "Connection refused telnet emr-header-1 10001" error is reported?](#section_ey6_aza_3ww)
-   [How do I fix the Spark job error "Container killed by YARN for exceeding memory limits" or the MapReduce job error "Container is running beyond physical memory limits"?](#section_ez3_s66_8mb)
-   [How do I fix the error "Error: Java heap space"?](#section_icx_5u6_nyj)
-   [How do I fix the error "No space left on device"?](#section_c7s_txd_vq8)
-   [What do I do if the error "ConnectTimeoutException" or "ConnectionException" is reported when I access OSS or Log Service?](#section_t4w_13t_rxh)
-   [What do I do if an out-of-memory error is reported when I run a job to read a Snappy file?](#section_rpg_rvi_i7q)
-   [How do I fix the error "Exception in thread main java.lang.RuntimeException: java.lang.ClassNotFoundException: Class com.aliyun.fs.oss.nat.NativeOssFileSystem not found"?](#section_dk1_w5t_8nl)
-   [What do I do if the error "java.lang.NoSuchMethodError:org.apache.http.conn.ssl.SSLConnetionSocketFactory.init\(Ljavax/net/ssl/SSLContext;Ljavax/net/ssl/HostnameVerifier\)" is reported when OSS SDK is used in Spark?](#section_d0o_0z9_ya2)
-   [How do I fix the error "java.lang.IllegalArgumentException: Wrong FS: oss://xxxxx, expected: hdfs://ip:9000"?](#section_h1b_r2r_rld)
-   [What do I do if the error "java.lang.IllegalArgumentException: Size exceeds Integer.MAX\_VALUE" is reported when a Spark job runs?](#section_086_xfa_gvy)

Questions about feature usage:

-   [Does EMR support real-time computing?](#section_b1n_wbd_bh3)
-   [What do I do if the timestamp field shows a delay of eight hours when I import data from ApsaraDB for RDS into EMR?](#section_ltj_c3f_d1c)
-   [How do I modify the spark-env configurations of the Spark service?](#section_0p6_0oc_xqe)
-   [How do I pass job parameters to a script?](#section_4v6_bmd_vzq)
-   [How do I set the authentication method of HiveServer2 to LDAP?](#section_3v5_qbo_zpx)
-   [How do I enable the HDFS balancer in an EMR cluster and optimize the performance of the HDFS balancer?](#section_qz6_nea_1k7)
-   [How do I submit a Spark job in standalone mode?](#section_c5g_07n_fzr)
-   [What do I do if the Custom Configuration button is not displayed for a service on the EMR console?](#section_b9j_1wh_n1c)

## What are the differences between jobs and workflows?

-   Job

    When you create a job on EMR, you configure only the JAR package, data input and output addresses, and some runtime parameters. These configurations determine how the job runs. After you complete the configurations and specify a job name, the job is created.

-   Workflow

    A workflow associates a job with a cluster.

    -   You can use a workflow to run a sequence of jobs.
    -   You can specify an existing cluster for a workflow to run jobs. If you do not specify a cluster, a temporary cluster is automatically created for the workflow.
    -   You can also schedule the execution of your workflow. After all jobs of the workflow are completed, the temporary cluster is automatically released.
    -   You can view the status and logs of each execution in the records of each workflow.

## How do I view job logs?

You can view job logs in the EMR console. If you log on to the master node to submit jobs and run scripts, you can plan job logs based on the scripts.

## How do I view logs on OSS?

1.  Log on to the EMR console and click the Data Platform tab. Find the project, and click Workflows in the Action column. In the left-side navigation pane of the page that appears, click the workflow whose logs you want to view. Click the **Records** tab in the lower part of the page.
2.  On the **Records** tab, find the workflow instance that you want to view, and click **Details** in the Action column. On the **Job Instance Info** tab of the page that appears, obtain the ID of the cluster where the job is run.
3.  Go to the OSS://mybucket/emr/spark directory and find the folder named after the cluster ID.
4.  Go to the OSS://mybucket/emr/spark/clusterID/jobs directory, which contains folders named after job IDs. Each directory stores the operational logs of a job.

## What do I do if the following error is reported when I read or write data from or to MaxCompute tables: "java.lang.RuntimeException.Parse response failed: '<! DOCTYPE html\>…'"?

Cause: The MaxCompute Tunnel endpoint may be invalid.

Solution: Enter a valid MaxCompute Tunnel endpoint.

## What do I do if a TPS conflict occurs when multiple consumer IDs consume the same topic?

Cause: This topic may have been created during public preview or in other environments. This causes consumption data inconsistency among multiple consumer groups. Solution: [submit a ticket](https://workorder-intl.console.aliyun.com/#/ticket/createIndex). Specify the topic and consumer IDs in the ticket.

## Can I view the job logs stored in core nodes?

Yes, you can view the job logs stored in core nodes.

## Why does an external table created in Hive contain no data?

Problem description: After an external table is created, the table is queried but no data is returned.

CREATE EXTERNAL TABLE statement:

```
CREATE EXTERNAL TABLE storage_log(content STRING) PARTITIONED BY (ds STRING)
    ROW FORMAT DELIMITED
    FIELDS TERMINATED BY '\t'
    STORED AS TEXTFILE
    LOCATION 'oss://log-12453****/your-logs/airtake/pro/storage';
```

Command that is used to query data:

```
hive> select * from storage_log;
```

Cause: Hive does not automatically associate a Partitions directory.

Solution: Manually specify a Partitions directory:

```
alter table storage_log add partition(ds=123);                                                                                                                                             OK
    Time taken: 0.137 seconds
    hive> select * from storage_log;   
```

The following data is returned:

```
     OK
    abcd    123
    efgh    123
```

## Why does a Spark Streaming job stop running unexpectedly?

-   Check whether the Spark version is earlier than 1.6. If it is, update it.

    Spark versions earlier than 1.6 have a memory leak bug. This bug can cause the container to stop running unexpectedly.

-   Check whether your code has been optimized in terms of memory usage.

## Why is a Spark Streaming job still in the Running state in the EMR console after the job has been stopped?

Cause: EMR cannot effectively monitor the status of Spark Streaming jobs that run in yarn-client mode.

Solution: Change the running mode of the job to yarn-cluster.

## How do I fix the error "Error: Could not find or load main class"?

Check whether the path protocol header of the JAR file for the job is `ossref`. If it is not, change it to `ossref`.

## How do I include local shared libraries in a MapReduce job?

Add the library files to the mapred-site.xml file. Example:

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

## How do I specify the file path of an OSS data source for a MapReduce or Spark job?

You can specify the input and output data sources of a job in the `oss://[accessKeyId:accessKeySecret@]bucket[.endpoint]/object/path` format, which is similar to `hdfs://` URLs.

You can access OSS data with or without an AccessKey pair:

-   \(Recommended\) EMR provides MetaService, which allows you to access OSS data without an AccessKey pair. You can specify a path in the oss://bucket/object/path format.
-   \(Not recommended\) You can configure the AccessKey ID, AccessKey secret, and endpoint parameters on the Configuration object for a MapReduce job or the SparkConf object for a Spark job. You can also directly specify the AccessKey ID, AccessKey secret, and endpoint in a URI. For more information, see [Development preparations](/intl.en-US/Developer Guide/Preparations/Development preparations.md).

## What do I do if the error "Exception in thread "main" java.sql.SQLException: No suitable driver found for jdbc:mysql:xxx" is reported when a Spark SQL job runs?

Cause: The version of mysql-connector-java is not supported.

Solution: Update mysql-connector-java to the latest version.

## What do I do if the error "Invalid authorization specification, message from server: ip not in whitelist" is reported when I connect Spark SQL to ApsaraDB for RDS?

Add the internal IP addresses of the cluster nodes into a whitelist of ApsaraDB for RDS.

## What do I do if the following error is reported when I run a Hive or Impala job to read Parquet data \(including columns of the DECIMAL type\) written by Spark SQL: "Failed with exception java.io.IOException:org.apache.parquet.io.ParquetDecodingException: Can not read value at 0 in block -1 in file hdfs://… /…/part-00000-xxx.snappy.parquet"?

Cause: The DECIMAL data type has different representations in the different Parquet conventions used in Hive and Spark SQL. Parquet data \(including columns of the DECIMAL type\) written by Spark SQL cannot be read properly by using Hive or Impala. Solution: To solve this issue, we recommend that you set the spark.sql.parquet.writeLegacyFormat parameter to true before you import the Parquet data written by Spark SQL to Hive or Impala. This setting makes Spark use the same convention as Hive or Impala for writing the Parquet data.

## How do I use Beeline to connect to Kerberos-authenticated clusters?

-   HA cluster \(service discovery mode\)

    ```
    ! connect jdbc:hive2://emr-header-1:2181,emr-header-2:2181,emr-header-3:2181/;serviceDiscoveryMode=zooKeeper;zooKeeperNamespace=hiveserver2;principal=hive/_HOST@EMR.${clusterId).COM
    ```

-   HA cluster
    -   Connect to emr-header-1

        ```
        ! connect jdbc:hive2://emr-header-1:10000/;principal=hive/emr-header-1@EMR.${clusterId}.COM
        ```

    -   Connect to emr-header-2

        ```
        ! connect jdbc:hive2://emr-header-2:10000/;principal=hive/emr-header-2@EMR.${clusterId}.COM
        ```

-   Non-HA cluster

    ```
    ! connect jdbc:hive2://emr-header-1:10000/;principal=hive/emr-header-1@EMR.${clusterId}.COM
    ```


## What do I do if the Thrift Server process properly runs but the "Connection refused telnet emr-header-1 10001" error is reported?

Check the logs in the /mnt/disk1/log/spark directory. This issue is caused by the Thrift Server running out of memory \(OOM\). You can increase memory by setting the spark.driver.memory parameter to a larger value.

## How do I view the logs of a service deployed in an EMR cluster?

Log on to the master node of the cluster. View the logs of the service in the /mnt/disk1/log directory.

## How do I fix the Spark job error "Container killed by YARN for exceeding memory limits" or the MapReduce job error "Container is running beyond physical memory limits"?

Cause: The amount of memory requested when you submit an application is too low. The JVM occupies more memory than the allocated amount during startup. As a result, the job is abnormally terminated by NodeManager. In particular, Spark jobs may consume a large amount of off-heap memory and are more likely to be abnormally terminated.

Solution:

-   For a Spark job, set spark.yarn.driver.memoryOverhead or spark.yarn.executor.memoryOverhead to a larger value.
-   For a MapReduce job, set mapreduce.map.memory.mb or mapreduce.reduce.memory.mb to a larger value.

## How do I fix the error "Error: Java heap space"?

Cause: The task has large amounts of data to process but the JVM has insufficient memory. As a result, an out-of-memory error is returned.

Solution:

-   For a Spark job, set spark.executor.memory or spark.driver.memory to a larger value.
-   For a MapReduce job, set mapreduce.map.java.opts or mapreduce.reduce.java.opts to a larger value.

## How do I fix the error "No space left on device"?

Possible causes:

-   A master or core node has insufficient storage space, which causes a job submission failure.
-   If a disk is full, an exception may occur in local Hive metadatabases such as MySQL Server, or a Hive metastore connection error may occur.

Solution: Clear enough disk space on the master node, including the system disk and HDFS space.

## What do I do if the error "ConnectTimeoutException" or "ConnectionException" is reported when I access OSS or Log Service?

Cause: The OSS endpoint is a public endpoint, but your EMR core node does not have a public IP address. As a result, you cannot access OSS. The error is reported when you access Log Service for the same reason.

Solution:

-   Change the OSS endpoint to an internal endpoint.
-   You can also use MetaService provided by EMR to access OSS or Log Service. If you use this method, you do not need to specify an endpoint.

For example, the `select * from tbl limit 10` command can be successfully executed, but `Hive SQL: select count(1) from tbl` fails. Change the OSS endpoint to an internal network endpoint.

```
alter table tbl set location "oss://bucket.oss-cn-hangzhou-internal.aliyuncs.com/xxx"
alter table tbl partition (pt = 'xxxx-xx-xx') set location "oss://bucket.oss-cn-hangzhou-internal.aliyuncs.com/xxx"
```

## What do I do if an out-of-memory error is reported when I run a job to read a Snappy file?

Cause: The format of standard Snappy files written by Log Service is different from that of Hadoop Snappy files. EMR processes Hadoop Snappy files, so an out-of-memory error is reported when it processes standard Snappy files.

Solution:

-   For a Hive job, set `io.compression.codec.snappy.native` to true.
-   For a MapReduce job, set `Dio.compression.codec.snappy.native` to true.
-   For a Spark job, set `spark.hadoop.io.compression.codec.snappy.native` to true.

## How do I fix the error "Exception in thread main java.lang.RuntimeException: java.lang.ClassNotFoundException: Class com.aliyun.fs.oss.nat.NativeOssFileSystem not found"?

If you want to use Spark jobs to read or write OSS data, you must install EMR SDK. For more information, see [Preparations](/intl.en-US/Developer Guide/Spark/Preparations.md).

## What do I do if an out-of-memory error is reported when Spark receives Flume data?

Check whether the data receiving method is push-based. If it is not, change the data receiving method to push-based. For more information, see [Spark Streaming and Flume integration guide](http://spark.apache.org/docs/latest/streaming-flume-integration.html?spm=a2c4g.11186623.2.4.pFAJPG).

## How do I fix the error "Caused by: java.io.IOException: Input stream cannot be reset as 5242880 bytes have been written, exceeding the available buffer size of 524288"?

The cache for network connection retries is insufficient. We recommend that you use aliyun-java-sdk-emr later than V1.1.0.

## What do I do if the error "java.lang.NoSuchMethodError:org.apache.http.conn.ssl.SSLConnetionSocketFactory.init\(Ljavax/net/ssl/SSLContext;Ljavax/net/ssl/HostnameVerifier\)" is reported when OSS SDK is used in Spark?

OSS SDK and the Spark and Hadoop running environments have version dependency conflicts. We recommend that you do not use OSS SDK in code.

## How do I fix the error "java.lang.IllegalArgumentException: Wrong FS: oss://xxxxx, expected: hdfs://ip:9000"?

The default file system of HDFS is used when you process OSS data. You must use the OSS path to initialize the file system so that it can be used to process OSS data.

```
Path outputPath = new Path(EMapReduceOSSUtil.buildOSSCompleteUri("oss://bucket/path", conf));
org.apache.hadoop.fs.FileSystem fs = org.apache.hadoop.fs.FileSystem.get(outputPath.toUri(), conf);        
if (fs.exists(outputPath)) {
  fs.delete(outputPath, true);        
}
```

## How do I clear the log data of a completed job?

Problem description: The HDFS space of a cluster is full. A large volume of data is stored in the /spark-history directory.

Solution:

1.  Go to the **Configure** tab for the Spark service. Check whether the **spark\_history\_fs\_cleaner\_enabled** parameter is specified in the **Service Configuration** section.
    -   If it is, change its value to true to periodically clear the logs of completed jobs.
    -   If it is not, click the **spark-defaults** tab. Click **Custom Configuration** in the upper-right corner. In the dialog box that appears, add the **spark\_history\_fs\_cleaner\_enabled** parameter and set it to true.
2.  Select **Restart All Components** from the **Actions** drop-down list in the upper-right corner.
3.  In the **Cluster Activities** dialog box, specify the related parameters and click **OK**.
4.  In the **Confirm** message, click **OK**.

## Why does a job run slowly?

Cause: If the size of the heap memory on the JVM where the job runs is too small, garbage collection may take a long time. As a result, the performance of the job is affected.

Solution:

-   For a Tez job, set the hive.tez.java.opts parameter to a larger value.
-   For a Spark job, set spark.executor.memory or spark.driver.memory to a larger value.
-   For a MapReduce job, set mapreduce.map.java.opts or mapreduce.reduce.java.opts to a larger value.

## Why does AppMaster take a long time to start a task?

Cause: If the number of tasks or Spark executors is large, AppMaster may take a long time to start a task. The running duration of a single task is short, but the overhead for scheduling jobs is large.

Solution:

-   Use CombinedInputFormat to reduce the number of tasks.
-   Increase the block size \(dfs.blocksize\) of the data generated by former jobs.
-   Set mapreduce.input.fileinputformat.split.maxsize to a larger value.
-   For Spark jobs, reduce the number of executors \(spark.executor.instances\) or reduce the number of concurrent jobs \(spark.default.parallelism\).

## What do I do if the error "java.lang.IllegalArgumentException: Size exceeds Integer.MAX\_VALUE" is reported when a Spark job runs?

During data shuffling, the number of partitions is smaller than the value of the Integer.MAX\_VALUE parameter. We recommend that you increase the number of partitions by setting the spark.default.parallelism and spark.sql.shuffle.partitions parameters to larger values. You can also perform the repartition operation before you perform data shuffling.

## Does EMR support real-time computing?

EMR provides three types of real-time computing services: Spark Streaming, Storm, and Flink.

## What do I do if the timestamp field shows a delay of eight hours when I import data from ApsaraDB for RDS into EMR?

Problem description:

1.  The Test\_Table table in ApsaraDB for RDS contains a timestamp field.

    ![data source](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/3021860061/p62051.png)

2.  The following command is used to import data in the Test\_Table table to EMR HDFS:

    ```
    sqoop import \
    --connect jdbc:mysql://rm-2ze****341.mysql.rds.aliyuncs.com:3306/s***o_sqoopp_db \
    --username s***o \
    --password ****** \
    --table play_evolutions \
    --target-dir /user/hadoop/output \
    --delete-target-dir \
    --direct \
    --split-by id \
    --fields-terminated-by '|' \
    -m 1
    ```

3.  The import result is queried.

    In the query result, the timestamp field shows a delay of eight hours.


Solution: When you import the data, delete the --direct parameter from the import command.

```
sqoop import \
--connect jdbc:mysql://rm-2ze****341.mysql.rds.aliyuncs.com:3306/s***o_sqoopp_db \
--username s***o \
--password ****** \
--table play_evolutions \
--target-dir /user/hadoop/output \
--delete-target-dir \
--split-by id \
--fields-terminated-by '|' \
-m 1
```

The query results are normal.

![Result](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/3021860061/p62069.png)

## How do I modify the spark-env configurations of the Spark service?

Log on to the master node of the cluster and modify the configurations in the /etc/ecm/spark-conf/spark-env.sh and /var/lib/ecm-agent/cache/ecm/service/SPARK/<Version Number\>/package/templates/spark-env.sh files.

**Note:** If you submit a task on a core node, you also need to modify the configurations on the core node.

## How do I pass job parameters to a script?

You can run a script in a Hive job and use the `-hivevar` option to pass job parameters to the script.

1.  Prepare a script.

    In a script, you can reference a variable in the format of `${varname}`, for example, `${rating}`. Example:

    -   Script name: hivesql.hive
    -   Path of the script in OSS: oss://bucket\_name/path/to/hivesql.hive
    -   Script content:

        ```
        use default;
         drop table demo;
         create table demo (userid int, username string, rating int);
         insert into demo values(100,"john",3),(200,"tom",4);
         select * from demo where rating=${rating};
        ```

2.  Log on to the [EMR console](https://emr.console.aliyun.com/).
3.  In the top navigation bar, select the region where your cluster residesand select a resource group based on your business requirements.
4.  Click the Data Platform tab.
5.  In the **Projects** section, click **Edit Job** in the Actions column of a project.
6.  In the **Edit Job** pane on the left, right-click the folder on which you want to perform operations and select **Create Job**.
7.  In the dialog box that appears, specify the **Name** and **Description** parameters, select Hive from the Job Type drop-down list, and then click OK.
8.  Configure the job.
    1.  Click Job Settings in the upper-right corner of the page. On the **Basic Settings** tab, specify **Key** and **Value** in the Configuration Parameters section. Set **Key** to a variable specified in the script, for example, **rating**.

        ![config_parameter](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6888060061/p60021.png)

    2.  Enter the following code in the Content field of the job to use the `-hivevar` option to pass the parameters configured in the job to the variables in the script:

        ```
        -hivevar rating=${rating} -f ossref://bucket_name/path/to/hivesql.hive
        ```

9.  Run the job.

    The following figure shows the result of the job.

    ![submit_log](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6888060061/p60123.png)


## How do I set the authentication method of HiveServer2 to LDAP?

1.  Log on to the [EMR console](https://emr.console.aliyun.com/).
2.  In the top navigation bar, select the region where your cluster residesand select a resource group based on your business requirements.
3.  Click the **Cluster Management** tab.
4.  Find the cluster whose HiveServer2 authentication method you want to change and click **Details** in the **Actions** column.
5.  In the left-side navigation pane, choose **Cluster Service** \> **Hive**.
6.  Set the authentication method of HiveServer2 to LDAP and restart HiveServer2.
    1.  Click the **Configure** tab. In the **Service Configuration** section, click the **hiveserver2-site** tab.

        ![hiveserver2-site configuration](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/7888060061/p60649.png)

    2.  Click **Custom Configuration** in the upper-right corner.

        To set the authentication method of HiveServer2 to **LDAP**, configure the parameters described in the following table.

        |Parameter|Value|Description|
        |---------|-----|-----------|
        |**hive.server2.authentication**|LDAP|The authentication method.|
        |**hive.server2.authentication.ldap.url**|Format: ldap://$\{emr-header-1-hostname\}:10389|Replace $\{emr-header-1-hostname\} with your hostname. You can run the `hostname` command on the emr-header-1 node of your cluster to obtain the hostname. For more information about how to log on to the emr-header-1 node, see [Connect to the master node of an EMR cluster in SSH mode](/intl.en-US/Cluster Management/Configure clusters/Connect to a cluster/Connect to the master node of an EMR cluster in SSH mode.md).|
        |**hive.server2.authentication.ldap.baseDN**|**ou=people,o=emr**|None.|

    3.  Click **Save** in the upper-right corner.
    4.  In the **Confirm Changes** dialog box, configure the parameters and click **OK**.
    5.  In the upper-right corner of the page, select **Restart HiveServer2** from the **Actions** drop-down list.
    6.  In the **Cluster Activities** dialog box, configure the parameters and click **OK**.
    7.  In the **Confirm** message, click **OK**.
7.  Add an account to the LDAP service.

    In an EMR cluster, OpenLDAP is a component of the LDAP service. OpenLDAP is used to manage Knox accounts by default. HiveServer2 can reuse the Knox accounts for LDAP authentication. For more information about how to add an account, see [Manage users](/intl.en-US/Cluster Management/Cluster planning/Manage user accounts.md).

    In this example, the **emr-guest** account is added.

8.  Check whether you can use the new account to log on to HiveServer2.

    Use /usr/lib/hive-current/bin/beeline to log on to HiveServer2.

    ```
    beeline> ! connect jdbc:hive2://emr-header-1:10000/
    Enter username for jdbc:hive2://emr-header-1:10000/: emr-guest
    Enter password for jdbc:hive2://emr-header-1:10000/: emr-guest-pwd
    Transaction isolation: TRANSACTION_REPEATABLE_READ
    ```

    If the account or password is invalid, the following error message appears:

    ```
    Error: Could not open client transport with JDBC Uri: jdbc:hive2://emr-header-1:10000/: Peer indicated failure: Error validating the login (state=08S01,code=0)
    ```


## How do I enable the HDFS balancer in an EMR cluster and optimize the performance of the HDFS balancer?

1.  Log on to a node of your cluster.
2.  Run the following commands to switch to the hdfs user and run the HDFS balancer:

    ```
    su hdfs
    /usr/lib/hadoop-current/sbin/start-balancer.sh -threshold 10
    ```

3.  Check the running status of the HDFS balancer:

    -   Method 1:

        ```
        less /var/log/hadoop-hdfs/hadoop-hdfs-balancer-emr-header-xx.cluster-xxx.log
        ```

    -   Method 2:

        ```
        tailf /var/log/hadoop-hdfs/hadoop-hdfs-balancer-emr-header-xx.cluster-xxx.log
        ```

    **Note:** If the command output includes `Successfully`, the HDFS balancer is running.

    The following table describes the HDFS balancer parameters.

    |Parameter|Description|
    |---------|-----------|
    |Threshold|Default value: 10%. This value ensures that the disk usage on each DataNode differs from the overall usage in the cluster by no more than 10%.

When the overall usage is high, set this parameter to a smaller value.

When a number of new nodes are added to the cluster, you can set this parameter to a larger value to move data from the high-usage nodes to the low-usage nodes. |
    |dfs.datanode.balance.max.concurrent.moves|Default value: 5.

This parameter specifies the maximum number of concurrent block moves that are allowed in a DataNode. Set this parameter based on the number of disks. We recommend that you set this parameter to `4 × Number of disks` as the upper limit on the DataNode.

For example, if a DataNode has 28 disks, set this parameter to 28 on the HDFS balancer and 112 on the DataNode.`` Adjust the value based on the cluster load. Increase the value when the cluster load is low, and decrease the value when the cluster load is high.

**Note:** After you set this parameter for a DataNode, restart the DataNode for the parameter setting to take effect. |
    |dfs.balancer.dispatcherThreads|The number of dispatcher threads used by the HDFS balancer to decide which blocks to move. Before the HDFS balancer moves a certain amount of data between two DataNodes, it repeatedly retrieves block lists for moving blocks until the required amount of data is scheduled.**Note:** Default value: 200. |
    |dfs.balancer.rpc.per.sec|The number of remote procedure calls \(RPCs\) sent by dispatcher threads per second. Default value: 20.Before the HDFS balancer moves data between two DataNodes, it uses dispatcher threads to repeatedly send the getBlocks\(\) RPC to the NameNode. This results in a heavy load on the NameNode. To avoid this issue and balance the cluster load, we recommend that you set this parameter to limit the number of RPCs sent per second.

For example, you can decrease the value of the parameter by 10 or 5 for a cluster with a high load to minimize the impact on the overall moving progress. |
    |dfs.balancer.getBlocks.size|The total data size of the blocks moved each time. Before the HDFS balancer moves data between two DataNodes, it repeatedly gets block lists for moving blocks until the required amount of data is scheduled. By default, the size of blocks in each block list is 2 GB. When the NameNode receives a getBlocks\(\) RPC, the NameNode is locked. If an RPC queries a large amount of blocks, the NameNode is locked for a long time, which slows down data writing. To avoid this issue, we recommend that you set this parameter based on the NameNode load.|
    |dfs.balancer.moverThreads|Default value: 1000.Each block move requires a thread. This parameter limits the total number of concurrent moves. |
    |dfs.namenode.balancer.request.standby|Default value: false.Specifies whether the HDFS balancer queries the list of blocks to be moved on the standby NameNode. When a NameNode receives a getBlocks\(\) RPC, the NameNode is locked. If an RPC queries a large number of blocks, the NameNode is locked for a long time, which slows down data writing. If you use an HA cluster, the HDFS balancer sends RPCs only to the standby NameNode. |
    |dfs.balancer.getBlocks.min-block-size|The minimum size of blocks to be queried by the getBlocks\(\) RPC. After you set this parameter, the getBlocks\(\) RPC skips blocks smaller than the minimum size. This improves the query efficiency. Default value: 10485760 \(10 MB\).|
    |dfs.balancer.max-iteration-time|The maximum duration of each iteration for moving blocks between two DataNodes. Default value: 1200000. Unit: milliseconds.When the duration of an iteration exceeds this limit, the HDFS balancer automatically enters the next iteration. |
    |dfs.balancer.block-move.timeout|Default value: 0. Unit: milliseconds.When the HDFS balancer moves blocks, an iteration may last for a long time because some block moves are not completed. You can set this parameter to avoid this issue. |

    The following table describes the DataNode parameters.

    |Parameter|Description|
    |---------|-----------|
    |dfs.datanode.balance.bandwidthPerSec|Default value: 1048576 \(1 MB/s\).Specifies the bandwidth for each DataNode to balance the cluster. We recommend that you set the bandwidth to 100 MB/s. You can also set the dfsadmin -setBalancerBandwidth parameter to adjust the bandwidth. You do not need to restart DataNodes.

For example, you can increase the bandwidth when the cluster load is low and decrease the bandwidth when the cluster load is high. |
    |dfs.datanode.balance.max.concurrent.moves|The maximum number of concurrent threads on a DataNode used by the HDFS balancer to move blocks.|


## How do I submit a Spark job in standalone mode?

You can submit a Spark job only in Spark on YARN mode. The standalone mode is not supported.

## What do I do if the Custom Configuration button is not displayed for a service on the EMR console?

1.  Log on to the master node of your cluster.
2.  Go to the following configuration template directory:

    ```
    cd /var/lib/ecm-agent/cache/ecm/service/HUE/4.4.0.3.1/package/templates/
    ```

    ![hue templates](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4021860061/p133413.png)

    Use the `HUE` service as an example.

    -   `HUE` is the name of the service directory.
    -   `4.4.0.3.1` is the Hue version.
    -   `hue.ini` is the configuration file.
3.  Run the following command to add the required custom configuration:

    ```
    vim hue.ini
    ```

    If the configuration item already exists, you can change the value based on the time.

4.  In the EMR console, restart the service for the configuration to take effect.

