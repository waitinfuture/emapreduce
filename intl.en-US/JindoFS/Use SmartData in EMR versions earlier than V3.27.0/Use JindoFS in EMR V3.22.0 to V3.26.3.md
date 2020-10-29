# Use JindoFS in EMR V3.22.0 to V3.26.3

JindoFS is a cloud-native file system that combines the advantages of Object Storage Service \(OSS\) and local storage. JindoFS is also a next-generation storage system that provides efficient and reliable storage services for cloud computing in E-MapReduce \(EMR\). This topic describes how to configure and use JindoFS, and its use scenarios.

## Overview

JindoFS supports the block storage mode and cache mode.

JindoFS adopts a heterogeneous multi-backup mechanism. Storage Service provides the data storage capability. Data is stored in OSS to ensure high reliability. Redundant backups are stored in the local cluster to accelerate read operations. Namespace Service manages metadata of JindoFS. In this case, metadata is queried from Namespace Service instead of OSS, which improves query performance. This query method of JindoFS is similar to that of Hadoop Distributed File System \(HDFS\).

**Note:**

-   EMR V3.20.0 and later support JindoFS. To use JindoFS, select the related services when you create an EMR cluster.
-   This topic describes how to use JindoFS in EMR V3.22.0 or later. For more information about how to use JindoFS in EMR V3.20.0 to V3.22.0 \(V3.22.0 excluded\), see [Use JindoFS in EMR V3.20.0 to V3.22.0 \(V3.22.0 excluded\)](/intl.en-US/JindoFS/Use SmartData in EMR versions earlier than V3.27.0/Use JindoFS in EMR V3.20.0 to V3.22.0 (V3.22.0 excluded).md).

![signal_path](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2807409951/p48599.png)

## Prepare the environment

-   Create an EMR cluster

    Select EMR V3.22.0 or a later version. Select **SmartData** from the optional services. For more information about how to create an EMR cluster, see [Create a cluster](/intl.en-US/Cluster Management/Configure clusters/Create a cluster.md).

    ![create_cluster](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2807409951/p62928.png)

-   Configure JindoFS

    JindoFS provided by SmartData uses OSS as the storage backend. Therefore, you must configure parameters related to OSS before you use JindoFS. EMR provides the following two configuration methods: 1. Modify parameters related to Bigboot after you create an EMR cluster, and then restart SmartData for the configurations to take effect. 2. Add custom configurations when you create an EMR cluster. In this case, the related services are restarted based on custom parameters after the EMR cluster is created.

    -   Initialize parameters after the EMR cluster is created

        You can configure all parameters related to JindoFS in Bigboot, as shown in the following figures.

        1.  In the Service Configuration section, click the bigboot tab.

            ![server_config](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2807409951/p62932.png)

        2.  Click **Custom Configuration** in the upper-right corner.

            ![cong_sel](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8502693061/p62943.png)

        **Note:**

        -   The parameters framed in red in the preceding figures are required.
        -   JindoFS supports multiple namespaces. A namespace named test is used in this topic.
        |Parameter|Description|Example|
        |---------|-----------|-------|
        |jfs.namespaces|The namespace supported by JindoFS. Separate multiple namespaces with commas \(,\).|test|
        |jfs.namespaces.test.uri|The storage backend of the test namespace.|oss://oss-bucket/oss-dir**Note:** You can set the value to a directory in an OSS bucket. In this case, this directory serves as the root directory, in which the test namespace reads and writes data. |
        |jfs.namespaces.test.mode|The storage mode of the test namespace.|block **Note:** JindoFS supports the block storage mode and cache mode. |
        |jfs.namespaces.test.oss.access.key|The AccessKey ID used to access the OSS bucket that serves as the storage backend.|xxxx**Note:** We recommend that you store data in an OSS bucket that is in the same region and under the same account as your EMR cluster. This ensures high performance and stability. In this case, you do not need to configure the AccessKey ID and AccessKey secret because the OSS bucket allows password-free access from the EMR cluster. |
        |jfs.namespaces.test.oss.access.secret|The AccessKey secret used to access the OSS bucket that serves as the storage backend.|

        Save and deploy the JindoFS configuration. Restart all components in SmartData to use JindoFS.

        ![server](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/3807409951/p48602.png)

    -   Add custom configurations when you create an EMR cluster

        You can add custom configurations when you create an EMR cluster. Assume that you want to create an EMR cluster in the same region as an OSS bucket to access OSS without using an AccessKey pair. Turn on **Custom Software Settings** as shown in the following figure. Add the following configuration to the field in the Advanced Settings section to customize parameters for the test namespace:

        ```
        [
        {
        "ServiceName":"BIGBOOT",
        "FileName":"bigboot",
        "ConfigKey":"jfs.namespaces","ConfigValue":"test"
        },{
        "ServiceName":"BIGBOOT",
        "FileName":"bigboot",
        "ConfigKey":"jfs.namespaces.test.uri",
        "ConfigValue":"oss://oss-bucket/oss-dir"
        },{
        "ServiceName":"BIGBOOT",
        "FileName":"bigboot",
        "ConfigKey":"jfs.namespaces.test.mode",
        "ConfigValue":"block"
        }
        ]
        ```

        ![kerbernets](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/3807409951/p62954.png)


## Use JindoFS

The use of JindoFS is similar to that of HDFS. JindoFS also provides a prefix. To use JindoFS, you only need to replace the hdfs prefix with the jfs prefix.

JindoFS supports most of the computing components in the EMR cluster, including Hadoop, Hive, Spark, Flink, Presto, and Impala.

Examples:

-   Run shell commands

    ```
    hadoop fs -ls jfs://your-namespace/ 
    hadoop fs -mkdir jfs://your-namespace/test-dir
    hadoop fs -put test.log jfs://your-namespace/test-dir/
    hadoop fs -get jfs://your-namespace/test-dir/test.log . /
    ```

-   Run a MapReduce job

    ```
    hadoop jar /usr/lib/hadoop-current/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.8.5.jar teragen -Dmapred.map.tasks=1000 10737418240 jfs://your-namespace/terasort/input
    hadoop jar /usr/lib/hadoop-current/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.8.5.jar terasort -Dmapred.reduce.tasks=1000 jfs://your-namespace/terasort/input jfs://your-namespace/terasort/output
    ```

-   Run a Spark SQL test

    ```
    CREATE EXTERNAL TABLE IF NOT EXISTS src_jfs (key INT, value STRING) location 'jfs://your-namespace/Spark_sql_test/';
    ```


## Control disk space usage

JindoFS uses OSS as the data storage backend, which allows you to store large volumes of data. However, the capacity of local disks is limited. JindoFS automatically deletes cold data in local disks. Alibaba Cloud uses the node.data-dirs.watermark.high.ratio and node.data-dirs.watermark.low.ratio parameters to adjust the space usage of local disks. The values of both parameters are in the range of 0 to 1 to indicate the percentage of space usage. JindoFS uses the total storage capacity of all data disks by default. The node.data-dirs.watermark.high.ratio parameter specifies the upper limit of space usage on each disk. Less frequently accessed data stored on a disk is released if the space used by JindoFS reaches the upper limit. The node.data-dirs.watermark.low.ratio parameter specifies the lower limit of space usage on each disk. After the space usage of a disk reaches the upper limit, less frequently accessed data is released until the space usage of the disk reaches the lower limit. You can configure the upper limit and lower limit to adjust and assign disk space to JindoFS. Make sure that the upper limit is greater than the lower limit.

## Configure the storage policy

JindoFS provides multiple storage policies to meet different storage needs. The following table lists four available storage policies for a directory.

|Policy|Description|
|------|-----------|
|COLD|Data has only a backup in OSS but no backups in the local cluster. This policy is suitable for storing cold data.|
|WARM|The default storage policy.

Data has a backup in OSS and a backup in the local cluster. The local backup can accelerate read operations. |
|HOT|Data has a backup in OSS and multiple backups in the local cluster. Local backups can accelerate read operations on hot data.|
|TEMP|Data has only a backup in the local cluster. This policy is suitable for storing temporary data. The local backup can accelerate read and write operations on the temporary data. However, this may lower data reliability.|

JindoFS provides a command-line tool Admin to configure the storage policy of a directory. The default storage policy is WARM. New files are stored based on the storage policy configured for the parent directory. Run the following command to configure the storage policy:

```
jindo dfsadmin -R -setStoragePolicy [path] [policy]
```

Run the following command to obtain the storage policy configured for a directory:

```
jindo dfsadmin -getStoragePolicy [path]
```

**Note:** The \[path\] parameter specifies the directory. The -R option specifies that a recursive operation is performed to configure the same storage policy for all sub-directories of the directory.

## Use the Admin tool

JindoFS supports the archive and jindo commands of the Admin tool.

-   The Admin tool provides the archive command to archive cold data.

    This command allows you to explicitly evict local blocks. Assume that Hive partitions a table by day. If the data generated a week ago in partitioned tables is infrequently accessed, you can run the archive command on the directory that stores such data on a regular basis. Then, the backups stored in the local cluster are evicted, whereas the backups in OSS are retained.

    Run the following archive command:

    ```
    jindo dfsadmin -archive [path]
    ```

    **Note:** The \[path\] parameter specifies the directory in which the data is to be archived.

-   The Admin tool provides the jindo command to manage metadata of JindoFS for Namespace Service.

    ```
    jindo dfsadmin [-options]
    ```

    **Note:** You can run the `jindo dfsadmin --help` command to obtain help information.


The Admin tool provides the diff and sync commands for the cache mode.

-   The diff command is used to display the difference between the data stored in the local cluster and that in OSS.

    ```
    jindo dfsadmin -R -diff [path]
    ```

    **Note:** By default, you can use the diff command to display the difference between the metadata stored in the local cluster and that in the sub-directories of the directory specified by the `[path]` parameter. The `-R` option specifies that a recursive operation is performed to compare the metadata stored in the local cluster with that stored in all sub-directories of the directory specified by the `[path]` parameter.

-   The sync command is used to synchronize the metadata between the local cluster and OSS.

    ```
    jindo dfsadmin -R -sync [path]
    ```

    **Note:** The `[path]` parameter specifies the directory in which the metadata is to be synchronized. By default, you can use the sync command to synchronize the metadata in the sub-directories of the directory specified by the `[path]` parameter to the local cluster. The `-R` option specifies that a recursive operation is performed to synchronize the metadata in all sub-directories of the directory specified by the `[path]` parameter.


