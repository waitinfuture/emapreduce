# Storage

This topic describes data storage in E-MapReduce \(EMR\) clusters.

## Disk roles

Two types of disks are available for a node in an EMR cluster: the system disk that is used to install the operating system, and data disks that are used to store data. One system disk is deployed by default, while many data disks can be deployed. A maximum of 16 data disks can be attached to each node. The data disks can vary in configurations, types, and capacities.

By default, Enhanced SSD \(ESSD\) is used as the system disk, one cloud disk is used as the data disk for the master node, and four cloud disks are used as the data disks for each core node.

## Cloud disks and local disks

The following two types of disks are available for data storage:

-   Cloud disks

    Include standard SSDs, ultra disks, and ESSDs. Disks are not attached directly to local compute nodes. Instead, they access a remote storage node over the network. Each piece of data has two real-time replicas at the backend. If the data is corrupted due to disk damage, a replica is used automatically for recovery.

-   Local disks

    Include local SATA disks and local SSDs of the big data type. Local disks are attached directly to compute nodes and have better performance than cloud disks. You cannot change the number of local disks. No data backup mechanism is deployed at the backend. Upper-layer software is required to ensure data reliability.


When nodes in an EMR cluster are released, data on all cloud disks and local disks is cleared. The disks cannot be kept independently and used again. Hadoop HDFS uses all data disks for data storage. Hadoop YARN also uses all data disks as temporary storage for computing.

If your business volume is below terabytes, you can use cloud disks that provide lower IOPS and throughput. If you have terabytes of data, we recommend that you use local disks. The data reliability of local disks is ensured by EMR. If the throughput of cloud disks is insufficient, use local disks instead.

## OSS

OSS can be used as HDFS in an EMR cluster. You can read and write data from and to OSS simply by modifying code that was originally used by HDFS. Example:

-   Use Spark to read data from HDFS:

    ```
    sc.textfile("hdfs://user/path")
    ```

    Change the storage type from HDFS to OSS:

    ```
    sc.textfile("oss://user/path")
    ```

-   You can directly run HDFS commands in OSS for MapReduce or Hive jobs.

    ```
    hadoop fs -ls oss://bucket/path
    hadoop fs -cp hdfs://user/path  oss://bucket/path
    ```

    In this process, you do not need to enter the AccessKey pair or endpoint of OSS. EMR automatically completes this information by using the data of the cluster owner. However, OSS is not suitable for scenarios that require high IOPS, such as Spark Streaming or HBase.


