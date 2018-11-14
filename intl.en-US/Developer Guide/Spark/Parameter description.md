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

