# 使用Spark处理JindoFS上的数据

Spark处理JindoFS上的数据，主要有两种方式，一种是直接调用文件系统接口使用；一种是通过SparkSQL读取存在JindoFS的数据表。

## JindoFS配置

已创建名为emr-jfs的命名空间，示例如下：

-   jfs.namespaces=emr-jfs
-   jfs.namespaces.emr-jfs.uri=oss://oss-bucket/oss-dir
-   jfs.namespaces.emr-jfs.mode=block

## 处理JindoFS上的数据

-   调用文件系统

    Spark中读写JindoFS上的数据，与处理其他文件系统的数据类似，以RDD操作为例，直接使用jfs的路径即可：

    ```
    val a = sc.textFile("jfs://emr-jfs/README.md")
    ```

    ![rdd_data](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1357459951/p63481.png)

    写入数据：

    ```
    scala> a.collect().saveAsTextFile("jfs://emr-jfs/output")
    ```

-   SparkSQL

    创建数据库、数据表以及分区时指定Location到JindoFS即可，详情请参见[使用Hive查询JindoFS上的数据](/intl.zh-CN/SmartData/JindoFS 生态/使用Hive查询JindoFS上的数据.md)。对于已经创建好的存储在JindoFS上的数据表，直接查询即可。


