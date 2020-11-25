# Use JindoFS as the storage backend of HBase

This topic describes how to use JindoFS as the storage backend of HBase.

## Background information

In the Hadoop ecosystem, HBase is a database with high write performance and is suitable for real-time data query. HBase clusters of EMR V3.22.0 or later support JindoFS or Object Storage Service \(OSS\) as the storage backend. JindoFS and OSS are more flexible than Hadoop Distributed File System \(HDFS\).

## JindoFS configuration

For example, a namespace named emr-jfs is created with the following configuration:

-   jfs.namespaces=emr-jfs
-   jfs.namespaces.emr-jfs.uri=oss://oss-bucket/oss-dir
-   jfs.namespaces.emr-jfs.mode=block

## Specify the storage path for an HBase cluster

JindoFS and OSS do not support data synchronization in EMR V3.22.0. You can set the hbase.rootdir parameter in the hbase-site file to a directory in JindoFS or OSS, and the hbase.wal.dir parameter to a local directory in HDFS. Then, you can store write-ahead logging \(WAL\) files in the HDFS. To release an HBase cluster, disable the tables on this cluster and make sure that the updates in WAL files are written to HFile.

|Configuration file|Parameter|Description|Example|
|------------------|---------|-----------|-------|
|hbase-site|hbase.rootdir|The root directory of the HBase cluster in JindoFS.|jfs://emr-jfs/hbase-root-dir|
|hbase.wal.dir|The local directory in which the HBase cluster stores WAL files in HDFS.|hdfs://emr-cluster/hbase|

## Create an EMR cluster

For more information about how to create a cluster, see [Create a cluster](/intl.en-US/Cluster Management/Configure clusters/Create a cluster.md).

You can add custom software configurations when you create an EMR cluster, as shown in the following figure.

![1](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7907409951/p63663.png)

## Use JindoFS

For example, to use JindoFS as the storage backend of an HBase cluster, use the following custom configuration and replace the OSS bucket and relevant paths:

```
[   
  {
       "ServiceName":"BIGBOOT",
       "FileName":"bigboot",
       "ConfigKey":"jfs.namespaces",
       "ConfigValue":"emr-jfs"
  },
  {
       "ServiceName":"BIGBOOT",
       "FileName":"bigboot",
       "ConfigKey":"jfs.namespaces.emr-jfs.uri",
       "ConfigValue":"oss://oss-bucket/jindoFS"
  },
  {
       "ServiceName":"BIGBOOT",
       "FileName":"bigboot",
       "ConfigKey":"jfs.namespaces.emr-jfs.mode",
       "ConfigValue":"block"
  },
  {
       "ServiceName":"HBASE",
       "FileName":"hbase-site",
       "ConfigKey":"hbase.rootdir",
       "ConfigValue":"jfs://emr-jfs/hbase-root-dir"
  },
  {
       "ServiceName":"HBASE",
       "FileName":"hbase-site",
       "ConfigKey":"hbase.wal.dir",
       "ConfigValue":"hdfs://emr-cluster/hbase"
  }
]
```

