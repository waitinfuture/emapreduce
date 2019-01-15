# Parameter description {#concept_sf4_nzg_hfb .concept}

The description of the parameters in Spark code

The following parameters can be configured in Spark code:

|Property|Default|Description|
|--------|-------|-----------|
|spark.hadoop.fs.oss.accessKeyId|None.|\(Optional\) The AccessKey ID required for accessing OSS.|
|spark.hadoop.fs.oss.accessKeySecret|None.|\(Optional\) The AccessKey Secret required for accessing OSS.|
|spark.hadoop.fs.oss.securityToken|None.|\(Optional\) The STS token required for accessing OSS.|
|spark.hadoop.fs.oss.endpoint|None.|\(Optional\) The endpoint used for accessing OSS.|
|spark.hadoop.fs.oss.multipart.thread.number|5|The number of threads \(concurrency\) used by OSS for the upload part - copy operation.|
|spark.hadoop.fs.oss.copy.simple.max.byte|134217728|The file size limit for copying files between buckets in OSS using common APIs.|
|spark.hadoop.fs.oss.multipart.split.max.byte|67108864|The file multipart limit for copying files between buckets in OSS using common APIs.|
|spark.hadoop.fs.oss.multipart.split.number|5|The number of multipart files for copying files between buckets in OSS using common APIs. By default, the number is the same as the number of threads \(concurrency\) used.|
|spark.hadoop.fs.oss.impl|com.aliyun.fs.oss.nat.NativeOssFileSystem|The implementation class for the native file system of OSS.|
|spark.hadoop.fs.oss.buffer.dirs|/mnt/disk1,/mnt/disk2,…|The OSS local temporary directory. It uses a data disk in the cluster by default.|
|spark.hadoop.fs.oss.buffer.dirs.exists|false|Whether the OSS temporary directory exists.|
|spark.hadoop.fs.oss.client.connection.timeout|50000|Timeout period for OSS client connections \(unit: milliseconds\).|
|spark.hadoop.fs.oss.client.socket.timeout|50000|Timeout period for OSS Client sockets \(unit: milliseconds\).|
|spark.hadoop.fs.oss.client.connection.ttl|-1|The value of time-to-live for clients|
|spark.hadoop.fs.oss.connection.max|1024|The maximum connections allowed.|
|spark.hadoop.job.runlocal|false|If the data source is OSS and you need to run and debug Spark code locally, set this parameter to true. Otherwise, set this parameter to false.|
|spark.logservice.fetch.interval.millis|200|The interval for receivers to retrieve data from LogHub.|
|spark.logservice.fetch.inOrder|true|Whether to consume the Shard data in order after the data has been parted.|
|spark.logservice.heartbeat.interval.millis|30000|The heartbeat interval for the data consumption processes \(unit: milliseconds\).|
|spark.mns.batchMsg.size|16|The number of MNS messages to fetch in bulk, with a maximum of 16.|
|spark.mns.pollingWait.seconds|30|The polling wait time if the MNS queue is empty.|
|spark.hadoop.io.compression.codec.snappy.native|false|Whether the Snappy files are in the standard Snappy format. By default, Hadoop recognizes Snappy files edited in Hadoop.|

## Smart Shuffle optimized configurations {#section_ofg_cbr_bgb .section}

Smart Shuffle is a shuffle implementation provided by Spark SQL in EMR version 3.16.0. It processes queries that have a large amount of shuffle data to improve the execution efficiency of Spark SQL. After Smart Shuffle is started, shuffle output data is not stored on ephemeral disks. Instead, data is transmitted to a remote node through a network in advance. Data in the same partition is transmitted to the same node and stored in the same file. Currently, Smart Shuffle has the following limitations:

-   Smart Shuffle cannot guarantee the atomictiy of task execution. When the execution of a task fails, you need to restart all the Spark jobs.
-   Smart Shuffle currently does not provide the External Smart Shuffle implementation, nor support Dynamic Resource Allocation.
-   Speculative tasks are not supported.
-   Smart Shuffle is incompatible with Adaptive Execution.

Smart Shuffle related configurations are as follows. You can enable Smart Shuffle by using Spark configuration files or the spark-submit parameters.

|Parameter|Default|Description|
|---------|-------|-----------|
|spark.shuffle.manager|sort|A shuffle method that allows you to enable the Smart Shuffle feature with Spark Session by configurating spark.shuffle.manager=org.apache.spark.shuffle.sort.SmartShuffleManager or spark.shuffle.manager=smart.|
|spark.shuffle.smart.spill.memorySizeForceSpillThreshold|128m|When Smart Shuffle is enabled, the memory used for each shuffle task has a threshold. When the threshold is reached, shuffle data is sent to the corresponding remote node through the network according to partitions.|
|spark.shuffle.smart.transfer.blockSize|1m|The size of the cache used for network transfers when Smart Shuffle is enabled.|

