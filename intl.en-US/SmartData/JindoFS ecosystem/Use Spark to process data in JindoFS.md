# Use Spark to process data in JindoFS

Spark processes data in JindoFileSystem \(JindoFS\) by using one of the following methods: call methods and use Spark SQL to read data from tables stored in JindoFS.

## JindoFS configuration

For example, a namespace named emr-jfs is created with the following configuration:

-   jfs.namespaces=emr-jfs
-   jfs.namespaces.emr-jfs.uri=oss://oss-bucket/oss-dir
-   jfs.namespaces.emr-jfs.mode=block

## Process data in JindoFS

-   Call methods

    The read and write operations performed by Spark in JindoFS are similar to those in other file systems. For example, to access data in JindoFS, use a directory with the jfs prefix in the following Resilient Distributed Dataset \(RDD\) operation:

    ```
    val a = sc.textFile("jfs://emr-jfs/README.md")
    ```

    ![rdd_data](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6907409951/p63481.png)

    To write data to JindoFS, call the following method:

    ```
    scala> a.collect().saveAsTextFile("jfs://emr-jfs/output")
    ```

-   Use Spark SQL

    Configure the parameter that sets the storage location to a directory in JindoFS when you create databases, tables, or partitions. For more information, see [Use Hive to query data in JindoFS](/intl.en-US/SmartData/JindoFS ecosystem/Use Hive to query data in JindoFS.md). Then, you can query data from tables stored in JindoFS.


