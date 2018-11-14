# Parameter description {#concept_dr3_dgm_hfb .concept}

Description of parameters in Hadoop code

The following parameters can be used in Hadoop code:

|Property|Default|Description|
|--------|-------|-----------|
|fs.oss.accessKeyId|None|\(Optional\) The required AccessKey ID used to access OSS.|
|fs.oss.accessKeySecret|None|\(Optional\) The required AccessKey Secret used to access OSS.|
|fs.oss.securityToken|None|\(Optional\) The required STS token used to access OSS.|
|fs.oss.endpoint|None|\(Optional\) The endpoint used to access OSS.|
|fs.oss.multipart.thread.number|5|The number of threads \(concurrency\) used by OSS for upload part copy.|
|fs.oss.copy.simple.max.byte|134217728|The file limit of the internal copy in OSS using common APIs.|
|fs.oss.multipart.split.max.byte|67108864|The file multipart limit of copying files between buckets in OSS using common APIs.|
|fs.oss.multipart.split.number|5|The file multipart limit of copying files between buckets in OSS that is using common APIs. By default, the limit is equal to the number of threads used in the copy.|
|fs.oss.impl|com.aliyun.fs.oss.nat.NativeOssFileSystem|The implementation class for the native OSS file system.|
|fs.oss.buffer.dirs|/mnt/disk1,/mnt/disk2,…|OSS local temporary directory. It uses a cluster data disk by default.|
|fs.oss.buffer.dirs.exists|false|Indicates whether the OSS temporary directory exists.|
|fs.oss.client.connection.timeout|50000|The connection timeout for the OSS client \(ms\).|
|fs.oss.client.socket.timeout|50,000|The socket timeout for the OSS client \(ms\).|
|fs.oss.client.connection.ttl|-1|The connection alive time|
|fs.oss.connection.max|1024|The maximum connections allowed.|
|io.compression.codec.snappy.native|false|Indicates whether to mark Snappy files as standard. By default, Hadoop recognizes Snappy files modified by Hadoop.|

