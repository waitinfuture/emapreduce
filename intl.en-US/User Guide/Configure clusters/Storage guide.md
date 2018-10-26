# Storage guide {#concept_ift_2n3_y2b .concept}

There are two types of disks on a node: one is the system disk which is used to install operating systems, the other is the data disk which is used for data storage. A node generally has one system disk by default which must be a cloud disk. However, you can have more than one data disk \(currently, up to sixteen on a single node\). Each piece of data disk can have different configurations, including different disk types and capacities. SSD cloud disks are defaulted in EMR as the cluster’s system disks. Four cloud disks are used in EMR by default. Considering current intranet bandwidth, the default configuration is reasonable.

## Cloud and ephemeral disks {#section_flt_fn3_y2b .section}

Two types of disks are available for data storage.

-   Cloud disks

    Include SSD, ultra, and basic cloud disks.

    Rather than being directly attached to a local computing node, cloud disks have access to a remote storage node by the network. Each piece of data has two real-time backups in the backend, thus three identical copies in total. When one of the copies is corrupted \(due to disk damage, rather than damages arising from business\), your backup data is automatically used for recovery.

-   Ephemeral disks

    Include ephemeral SATA disks in the big data type and ephemeral SSD disks used in the ephemeral SSD type.

    Ephemeral disks are attached directly to the computing node and have better performance than cloud disks. You cannot select the number of ephemeral disks and must keep the default configurations. Similar to offline physical hosts, no data backup is in the backend, and upper-level software to guarantee data reliability is required.


## Applicable use cases {#section_srg_3n3_y2b .section}

In EMR, when the hosting node is released, data in all cloud and ephemeral disks is cleared. The disks cannot be kept independently or re-used. Hadoop HDFS uses all data disks for data storage. Hadoop YARN also uses all data disks as on-demand data storage for computing.

When your business does not involve large data volume \(below TB level\), cloud disks can be used as the IOPS and throughput are smaller than local disks. In case of large data volumes, we recommend that you use local disks whose data reliability is guaranteed by EMR. If you encounter apparently insufficient throughput, you can switch to ephemeral disks.

## OSS {#section_dpg_jn3_y2b .section}

OSS can be used as HDFS in EMR. You can have easy read and write access to OSS. All codes using HDFS can also be simply edited to access data on OSS.

For example:

Reading data from spark

```
sc.Textfile("hdfs://user/path")
```

Replace storage type HDFS-\> OSS

```
sc.Textfile("oss://user/path")
```

The same is true for Mr or hive jobs.

HDFS commands directly handle OSS data

```
hadoop fs -ls oss://bucket/path
hadoop fs -cp hdfs://user/path  oss://bucket/path
```

In this process, you do not need to enter AK and endpoint, EMR will automatically complete the user's information using the current cluster owner.

However, as OSS does not have high IOPS, it is not suitable for use cases that require high IOPS, such as Spark Streaming or HBase.

