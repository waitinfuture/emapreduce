# JindoFS文件元数据离线分析

EMR-3.30.0及后续版本的Block模式，支持dump整个namespace的元数据信息至OSS中，并通过Jindo Sql工具直接分析元数信息。

在HDFS文件系统中，整个分布式文件的元数据存储在名为fsimage的快照文件中。文件中包含了整个文件系统的命名空间、文件、Block和文件系统配额等元数据信息。HDFS支持通过命令行下载整个fsimage文件（xml形式）到本地，以便离线分析元数据信息，而JindoFS无需下载元数据信息至本地。

## 上传文件系统元数据至OSS

使用Jindo命令行工具上传命名空间的元数据至OSS，命令格式如下。

```
jindo jfs -dumpMetadata <nsName>
```

`<nsName>`为Block模式对应的namespace名称。

例如，上传并离线分析test-block的元数据。

```
jindo jfs -dumpMetadata test-block
```

![test-block](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6155073061/p176259.png)

当提示如下信息时，表示上传成功并以JSON格式的文件存放在OSS中。

```
Sucessfully upload namespace metadata to OSS.
```

## 元数据上传路径

元数据信息上传的路径为JindoFS中配置的sysinfo的子目录下的metadataDump子目录。

例如，配置的`namespace.sysinfo.oss.uri`为`oss://abc/`，则上传的文件会在`oss://abc/metadataDump`子目录中。

|参数|说明|
|--|--|
|namespace.sysinfo.oss.uri|存储Bucket和路径。|
|namespace.sysinfo.oss.endpoint|对应Endpoint信息，支持跨Region。|
|namespace.sysinfo.oss.access.key|阿里云的AccessKey ID。|
|namespace.sysinfo.oss.access.secret|阿里云的AccessKey Secret。|

批次信息：因为分布式文件系统的元数据会跟随用户的使用发生变化，所以我们每次对元数据进行分析是基于命令执行当时的元数据信息的快照进行的。每次运行Jindo命令进行上传会在目录下，根据上传时间生成对应批次号作为本次上传文件的根目录，以保证每次上传的数据不会被覆盖，您可以根据需要删除历史数据。

![namespace](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6155073061/p176263.png)

-   ①表示OSS系统信息配置路径。
-   ②表示namespce。
-   ③表示批次号。

## 元数据Schema

上传至OSS的文件系统元信息以JSON文件格式存放。其Schema信息如下。

```
{
  "type":"string",          /*INode类型，FILE文件 DIRECTORY目录*/
  "id": "string",            /*INode id*/
  "parentId" :"string",         /*父节点id*/
  "name":"string",         /*INode名称*/
  "size": "int",         /*INode大小， bigint*/
  "permission":"int",         /*permmsion 以int格式存放*/
  "owner":"string",          /*owner名称*/
  "ownerGroup":"string",     /*owner组名称*/
  "mtime":"int",              /*inode修改时间， bigint*/
  "atime":"int",              /*inode最近访问时间， bigint*/
  "attributes":"string",       /*文件相关属性*/
  "state":"string",            /*INode状态*/
  "storagePolicy":"string",    /*存储策略*/
  "etag":"string"           /*etag*/
}
```

## 使用Jindo Sql分析元数据

1.  执行如下命令，启动Jindo Sql。

    ![start_sql](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0302263061/p176257.png)

2.  查询Jindo Sql可以分析的表格。

    -   使用`show tables`可以查看支持查询分析的表格。目前Jindo Sql内置了审计和元数据信息的分析功能，对应audit\_log和fs\_image。
    -   使用`show partitions fs_image`可以查看表的fs\_image分区信息。每一个分区对应于一次上传`jindo jfs -dumpMetadata`生成的数据。

        示例如下。

        ![show_partitions](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0302263061/p176258.png)

3.  查询分析元数据信息。

    Jindo Sql使用Spark-SQL语法。您可以使用sql进行分析和查询fs\_image表。

    示例如下。

    ![check fs_image](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6155073061/p176289.png)

    namespace和datatime为jindo sql增加的两列，分别对应于namespace名称和上传元数据的时间戳。

    例如：根据某次dump的元数据信息统计该namespace下的目录个数。

    ![Number of queries](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6155073061/p176292.png)


## 使用Hive分析元数据

1.  在Hive中创建Table Schema。

    在Hive中创建对应的元信息以供查询，您可以参考下面的格式在Hive中创建文件系统元信息对应表的Schema。

    ```
    CREATE EXTERNAL TABLE `table_name` 
    (`type` string,
     `id` string,
     `parentId` string,
     `name` string,
     `size` bigint, 
     `permission` int,
     `owner` string,
     `ownerGroup` string,
     `mtime` bigint, 
     `atime` bigint,
     `attr` string,
     `state` string,
     `storagePolicy` string,
     `etag` string) 
     ROW FORMAT SERDE 'org.apache.hive.hcatalog.data.JsonSerDe' 
     STORED AS TEXTFILE 
     LOCATION '文件上传的OSS路径';
    ```

2.  使用Hive进行离线分析。

    创建完Hive表后，您可以使用Hive SQL分析元数据。

    ```
    select * from table_name limit 200;
    ```

    示例如下。

    ![Offline analysis](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6155073061/p176278.png)


