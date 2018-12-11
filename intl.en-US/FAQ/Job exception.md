# Job exception {#concept_kd1_xyb_pfb .concept}

## Q: Why does a Spark job report "Container killed by YARN for exceeding memory limits" or a MapReduce job report "Container is running beyond physical memory limits"? {#section_pyz_xyb_pfb .section}

A: The amount of memory assigned is low when the application is submitted. The JVM consumes too much memory during startup, exceeding the assigned amount. This causes the job to be terminated by NodeManager. This also affects Spark jobs, which may consume more off-heap memory. For Spark jobs, increase the value of spark.yarn.driver.memoryOverhead or spark.yarn.executor.memoryOverhead. For MapReduce jobs, increase the value of mapreduce.map.memory.mb and mapreduce.reduce.memory.mb.

## Q: Why is "Error: Java heap space" returned when I submit a job? {#section_qyz_xyb_pfb .section}

A: The task has large amounts of data in the process but the JVM has insufficient memory. As a result, the OutOfMemoryError error is returned. For Tez jobs, increase the value of hive.tez.java.opts. For Spark jobs, increase the value of spark.executor.memory or park.driver.memory. For MapReduce jobs, increase the value of mapreduce.map.java.opts or mapreduce.reduce.java.opts.

## Q: Why is "No space left on device" returned when I submit a job? {#section_ryz_xyb_pfb .section}

A: Master or worker node has insufficient storage place, which causes a failure of submitting the job. If the disk is full, exceptions in local Hive meta databases such as MySQL Server, or Hive Metastore connection errors may occur. We recommend that you clear enough disk space of the master node, including the system disk and HDFS space.

## Q: Why is "ConnectTimeoutException" or "ConnectionException" returned when I use OSS or Log Service? {#section_syz_xyb_pfb .section}

A: The OSS endpoint is a public network address, but the EMR worker node does not have a public IP address. Therefore, you cannot access OSS or Log Service. For example, the statement `select * from tbl limit 10` can be successfully executed, but `Hive SQL: select count(1) from tbl` fails.

Set the OSS endpoint to an internal network address, such as oss-cn-hangzhou-internal.aliyuncs.com, or use MetaService provided by EMR. If you choose to use MetaService, you do not need to specify an endpoint.

```
alter table tbl set location "oss://bucket.oss-cn-hangzhou-internal.aliyuncs.com/xxx"
alter table tbl partition (pt = 'xxxx-xx-xx') set location "oss://bucket.oss-cn-hangzhou-internal.aliyuncs.com/xxx"
```

## Q: Why is "OutOfMemoryError" returned when I read a Snappy file? {#section_imy_rzb_pfb .section}

A: The format of standard Snappy files written by Log Service is different from that of the Hadoop Snappy files. By default, EMR processes Hadoop Snappy files.When it processes standard Snappy files, the OutOfMemoryError error is returned. You can set the value of the corresponding parameters to true for troubleshooting. For Hive jobs, configure `set io.compression.codec.snappy.native=true`. For MapReduce jobs, configure `Dio.compression.codec.snappy.native=true`. For Spark jobs, configure `spark.hadoop.io.compression.codec.snappy.native=true`.

## Q: Why is "Invalid authorization specification, message from server: "ip not in whitelist or in blacklist, client ip is xxx" returned when I connect the EMR cluster to an RDS instance? {#section_jmy_rzb_pfb .section}

A: You need to configure the whitelist on the RDS instance when you connect the EMR cluster to an RDS instance. If you do not add the IP addresses of the cluster nodes to the whitelist, especially after expanding the cluster, this error occurs.

## Q: Why is "Exception in thread “main” java.lang.RuntimeException: java.lang.ClassNotFoundException: Class com.aliyun.fs.oss.nat.NativeOssFileSystem not found" returned when reading or writing OSS data? {#section_kmy_rzb_pfb .section}

A: When reading or writing OSS data in Spark jobs, you need to package the EMR SDK into the job JAR. For more information, see [Prerequisites](../../../../intl.en-US/Developer Guide/Spark/Preparations.md#).

## Q: Why is the available memory of the Spark node exceeded when Spark is connected to Flume? {#section_lmy_rzb_pfb .section}

A: Check whether the data receiving mode is Push-based. If not, set the mode to Push-based. For more information, see [Documentation](http://spark.apache.org/docs/latest/streaming-flume-integration.html?spm=a2c4g.11186623.2.4.pFAJPG).

## Q: Why is "Caused by: java.io.IOException: Input stream cannot be reset as 5242880 bytes have been written, exceeding the available buffer size of 524288" returned when I connect OSS to the Internet? {#section_mmy_rzb_pfb .section}

A: This is a bug caused by insufficient space for caching during network connection retries. We recommend that you use the EMR SDK with a version later than V1.1.0.

## Q: Why is "Failed to access metastore. This class should not accessed in runtime.org.apache.hadoop.hive.ql.metadata.HiveException: java.lang.RuntimeException: Unable to instantiate org.apache.hadoop.hive.ql.metadata.SessionHiveMetaStoreClient” returned when Spark is running ? {#section_nmy_rzb_pfb .section}

A: When Spark processes Hive data, you must set the execution mode of Spark to yarn-client or local. Do not set the mode to yarn-cluste. Otherwise, this error occurs. If the JAR package of the job contains third-party files, this error may occur when Spark is running.

## Q: Why is "java.lang.NoSuchMethodError:org.apache.http.conn.ssl.SSLConnetionSocketFactory.init\(Ljavax/net/ssl/SSLContext;Ljavax/net/ssl/HostnameVerifier\)" returned when using the OSS SDK in Spark? {#section_omy_rzb_pfb .section}

A: The http-core and http-client packages that the OSS SDK is dependent on have version dependency conflicts with the running environments of Spark and Hadoop. We recommend that you do not use the OSS SDK in your code. Otherwise, you must manually resolve this issue. If you need to perform some basic operations to handle OSS files, such as listing objects, click [here](../../../../intl.en-US/Developer Guide/Spark/Preparations.md#) to view the detailed information about how to handle OSS files.

## Q: Why is "java.lang.IllegalArgumentException: Wrong FS: oss://xxxxx, expected: hdfs://ip:9000" returned when I use OSS? {#section_pmy_rzb_pfb .section}

A: The default filesystem of HDFS is used when you process OSS data. You must use the OSS path to initialize the filesystem so that it can be used to process data on OSS in the following steps.

```
Path outputPath = new Path(EMapReduceOSSUtil.buildOSSCompleteUri("oss://bucket/path", conf));        org.apache.hadoop.fs.FileSystem fs = org.apache.hadoop.fs.FileSystem.get(outputPath.toUri(), conf);        
if (fs.exists(outputPath)) {
  fs.delete(outputPath, true);        
}
```

## Q: Why does garbage collection take a long time and job execution become slower? {#section_g3j_k1c_pfb .section}

A: If the size of the heap memory on the JVM that executes the job is too small, garbage collection may take a longer time and the performance of the job is affected. We recommend that you expand the Java Heap Size. For Tez jobs, increase the value of the hive.tez.java.opts Hive parameter. For Spark jobs, increase the value of spark.executor.memory or spark.driver.memory. For MapReduce jobs, increase the value of mapreduce.map.java.opts or mapreduce.reduce.java.opts.

## Q: Why does AppMaster take a long time to start a task? {#section_p3j_k1c_pfb .section}

A: If there are too many job tasks or Spark executors, AppMaster may take a long time to start a task. The runtime of a single task is short, and the overhead for scheduling jobs becomes large. We recommend that you use CombinedInputFormat to reduce the number of tasks. You can also increase the block size \(dfs.blocksize\) of data that is produced by former jobs, or increase the value of mapreduce.input.fileinputformat.split.maxsize. For Spark jobs, you can reduce the number of executors \(spark.executor.instances\) or reduce the number of concurrent jobs \(spark.default.parallelism\).

## Q: Why does it take a long time to apply for resources, which causes a job pending issue? {#section_q3j_k1c_pfb .section}

A: After the job is submitted, AppMaster needs to apply for resources to start the task. The cluster is occupied during this period and it may take a long time to apply for resources, causing a job pending issue. We recommend that you check whether the configurations of resource groups are inappropriate, and whether the current resource group is occupied but the cluster still has available resources. If so, you can adjust the configurations of key resource groups or resize the cluster to make full use of the resources .

## Q: Why does a small number of tasks take a long time to execute, and the overall runtime of the job become longer \(data skew problem\)? {#section_r3j_k1c_pfb .section}

A: During a certain stage of the task, data is distributed unevenly. In this circumstance, most tasks are quickly executed, but a small number of tasks takes a long time to execute due to large amounts of data. This makes the overall runtime of the job become longer. We recommend that you use the mapjoin feature of Hive and `set hive.optimize.skewjoin = true`.

## Q: Why does a failed task attempt make the job runtime longer? {#section_s3j_k1c_pfb .section}

A: A job has a failed task attempt or failed job attempt. Although the job may end normally, the failed attempt may make the runtime of the job become longer. We recommend that you locate the cause of task failures from this section.

## Q: Why is "java.lang.IllegalArgumentException: Size exceeds Integer.MAX\_VALUE" returned when the Spark job is running? {#section_t3j_k1c_pfb .section}

A: The block size may become too large if the number of partitions is too small. The maximum value of Integer.MAX\_VALUE\(2 GB\) may then be exceeded when you perform data shuffling. We recommend that you increase the number of partitions, and increase the value of spark.default.parallelism, spark.sql.shuffle.partitions, or perform the repartition operation before you perform data shuffling.

