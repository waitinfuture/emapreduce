# Jindo DistCp使用说明

本文介绍JindoFS的数据迁移工具Jindo DistCp的使用方法。

-   本地安装了Java JDK 8。
-   已创建EMR-3.28.0或后续版本的集群，详情请参见[创建集群](/cn.zh-CN/集群管理/集群配置/创建集群.md)。

## 使用Jindo Distcp

执行以下命令，获取帮助信息。

```
[root@emr-header-1 opt]# jindo distcp --help
```

返回信息如下。

```
     --help   -   Print help text
     --src=VALUE   -   Directory to copy files from
     --dest=VALUE   -   Directory to copy files to
     --parallelism=VALUE   -   Copy task parallelism
     --outputManifest=VALUE   -   The name of the manifest file
     --previousManifest=VALUE   -   The path to an existing manifest file
     --requirePreviousManifest=VALUE   -   Require that a previous manifest is present if specified
     --copyFromManifest   -   Copy from a manifest instead of listing a directory
     --srcPrefixesFile=VALUE   -   File containing a list of source URI prefixes
     --srcPattern=VALUE   -   Include only source files matching this pattern
     --deleteOnSuccess   -   Delete input files after a successful copy
     --outputCodec=VALUE   -   Compression codec for output files
     --groupBy=VALUE   -   Pattern to group input files by
     --targetSize=VALUE   -   Target size for output files
     --enableBalancePlan   -   Enable plan copy task to make balance
     --enableDynamicPlan   -   Enable plan copy task dynamically
     --enableTransaction   -   Enable transation on Job explicitly
     --diff   -   show the difference between src and dest filelist
     --ossKey=VALUE   -   Specify your oss key if needed
     --ossSecret=VALUE   -   Specify your oss secret if needed
     --ossEndPoint=VALUE   -   Specify your oss endPoint if needed
     --policy=VALUE   -   Specify your oss storage policy
     --cleanUpPending   -   clean up the incomplete upload when distcp job finish
     --queue=VALUE   -   Specify yarn queuename if needed
     --bandwidth=VALUE   -   Specify bandwidth per map/reduce in MB if needed
     --s3Key=VALUE   -   Specify your s3 key
     --s3Secret=VALUE   -   Specify your s3 Sercet
     --s3EndPoint=VALUE   -   Specify your s3 EndPoint
```

## --src和--dest

`--src`表示指定源文件的路径，`--dest`表示目标文件的路径。

Jindo DistCp默认将`--src`目录下的所有文件拷贝到指定的`--dest`路径下。您可以通过指定`--dest`路径来确定拷贝后的文件目录，如果不指定根目录，Jindo DistCp会自动创建根目录。

例如，如果您需要将/opt/tmp下的文件拷贝到OSS bucket，可以执行以下命令。

```
jindo distcp --src /opt/tmp --dest oss://yang-hhht/tmp
```

## --parallelism

`--parallelism`用于指定MapReduce作业里的mapreduce.job.reduces参数。该参数默认为7，您可以根据集群的资源情况，通过自定义`--parallelism`大小来控制DistCp任务的并发度。

例如，将HDFS上/opt/tmp目录拷贝到OSS bucket，可以执行以下命令。

```
jindo distcp --src /opt/tmp --dest oss://yang-hhht/tmp --parallelism 20
```

## --srcPattern

`--srcPattern`使用正则表达式，用于选择或者过滤需要复制的文件。您可以编写自定义的正则表达式来完成选择或者过滤操作，正则表达式必须为全路径正则匹配。

例如，如果您需要复制/data/incoming/hourly\_table/2017-02-01/03下所有log文件，您可以通过指定`--srcPattern`的正则表达式来过滤需要复制的文件。

执行以下命令，查看/data/incoming/hourly\_table/2017-02-01/03下的文件。

```
[root@emr-header-1 opt]# hdfs dfs -ls /data/incoming/hourly_table/2017-02-01/03
```

返回信息如下。

```
Found 6 items
-rw-r-----   2 root hadoop       2252 2020-04-17 20:42 /data/incoming/hourly_table/2017-02-01/03/000151.sst
-rw-r-----   2 root hadoop       4891 2020-04-17 20:47 /data/incoming/hourly_table/2017-02-01/03/1.log
-rw-r-----   2 root hadoop       4891 2020-04-17 20:47 /data/incoming/hourly_table/2017-02-01/03/2.log
-rw-r-----   2 root hadoop       4891 2020-04-17 20:42 /data/incoming/hourly_table/2017-02-01/03/OPTIONS-000109
-rw-r-----   2 root hadoop       1016 2020-04-17 20:47 /data/incoming/hourly_table/2017-02-01/03/emp01.txt
-rw-r-----   2 root hadoop       1016 2020-04-17 20:47 /data/incoming/hourly_table/2017-02-01/03/emp06.txt
```

执行以下命令，复制以log结尾的文件。

```
jindo distcp --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --srcPattern .*\.log --parallelism 20
```

执行以下命令，查看目标bucket的内容。

```
[root@emr-header-1 opt]# hdfs dfs -ls oss://yang-hhht/hourly_table/2017-02-01/03
```

返回信息如下，显示只复制了以log结尾的文件。

```
Found 2 items
-rw-rw-rw-   1       4891 2020-04-17 20:52 oss://yang-hhht/hourly_table/2017-02-01/03/1.log
-rw-rw-rw-   1       4891 2020-04-17 20:52 oss://yang-hhht/hourly_table/2017-02-01/03/2.log
```

## --deleteOnSuccess

`--deleteOnSuccess`可以移动数据并从源位置删除文件。

例如，执行以下命令，您可以将/data/incoming/下的hourly\_table文件移动到OSS bucket中，并删除源位置文件。

```
jindo distcp --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --deleteOnSuccess --parallelism 20
```

## --outputCodec

`--outputCodec`可以在线高效地存储数据和压缩文件。

```
jindo distcp --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --outputCodec=gz --parallelism 20
```

目标文件夹中的文件已经使用gz编解码器压缩了。

```
[root@emr-header-1 opt]# hdfs dfs -ls oss://yang-hhht/hourly_table/2017-02-01/03
```

返回信息如下：

```
Found 6 items
-rw-rw-rw-   1        938 2020-04-17 20:58 oss://yang-hhht/hourly_table/2017-02-01/03/000151.sst.gz
-rw-rw-rw-   1       1956 2020-04-17 20:58 oss://yang-hhht/hourly_table/2017-02-01/03/1.log.gz
-rw-rw-rw-   1       1956 2020-04-17 20:58 oss://yang-hhht/hourly_table/2017-02-01/03/2.log.gz
-rw-rw-rw-   1       1956 2020-04-17 20:58 oss://yang-hhht/hourly_table/2017-02-01/03/OPTIONS-000109.gz
-rw-rw-rw-   1        506 2020-04-17 20:58 oss://yang-hhht/hourly_table/2017-02-01/03/emp01.txt.gz
-rw-rw-rw-   1        506 2020-04-17 20:58 oss://yang-hhht/hourly_table/2017-02-01/03/emp06.txt.gz
```

Jindo DistCp当前版本支持编解码器gzip、gz、lzo、lzop、snappy以及关键字none和keep（默认值）。关键字含义如下：

-   none表示保存为未压缩的文件。如果文件已压缩，则Jindo DistCp会将其解压缩。
-   keep表示不更改文件压缩形态，按原样复制。

**说明：** 如果您想在开源Hadoop集群环境中使用编解码器lzo，则需要安装gplcompression的native库和hadoop-lzo包。

## --outputManifest和--requirePreviousManifest

`--outputManifest`可以指定生成DistCp的清单文件，用来记录copy过程中的目标文件、源文件和数据量大小等信息。

如果您需要生成清单文件，则指定`--requirePreviousManifest`为`false`。当前outputManifest文件默认且必须为gz类型压缩文件。

```
jindo distcp --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --outputManifest=manifest-2020-04-17.gz --requirePreviousManifest=false --parallelism 20
```

查看outputManifest文件内容。

```
[root@emr-header-1 opt]# hadoop fs -text oss://yang-hhht/hourly_table/manifest-2020-04-17.gz > before.lst
[root@emr-header-1 opt]# cat before.lst 
```

返回信息如下。

```
{"path":"oss://yang-hhht/hourly_table/2017-02-01/03/000151.sst","baseName":"2017-02-01/03/000151.sst","srcDir":"oss://yang-hhht/hourly_table","size":2252}
{"path":"oss://yang-hhht/hourly_table/2017-02-01/03/1.log","baseName":"2017-02-01/03/1.log","srcDir":"oss://yang-hhht/hourly_table","size":4891}
{"path":"oss://yang-hhht/hourly_table/2017-02-01/03/2.log","baseName":"2017-02-01/03/2.log","srcDir":"oss://yang-hhht/hourly_table","size":4891}
{"path":"oss://yang-hhht/hourly_table/2017-02-01/03/OPTIONS-000109","baseName":"2017-02-01/03/OPTIONS-000109","srcDir":"oss://yang-hhht/hourly_table","size":4891}
{"path":"oss://yang-hhht/hourly_table/2017-02-01/03/emp01.txt","baseName":"2017-02-01/03/emp01.txt","srcDir":"oss://yang-hhht/hourly_table","size":1016}
{"path":"oss://yang-hhht/hourly_table/2017-02-01/03/emp06.txt","baseName":"2017-02-01/03/emp06.txt","srcDir":"oss://yang-hhht/hourly_table","size":1016}
```

## --outputManifest和--previousManifest

`--outputManifest`表示包含所有已复制文件（旧文件和新文件）的列表，`--previousManifest`表示只包含之前复制文件的列表。您可以使用`--outputManifest`和`--previousManifest`重新创建完整的操作历史记录，查看运行期间复制的文件。

例如，在源文件夹中新增加了两个文件，命令如下所示。

```
jindo distcp --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --outputManifest=manifest-2020-04-18.gz --previousManifest=oss://yang-hhht/hourly_table/manifest-2020-04-17.gz --parallelism 20
```

执行以下命令，查看运行期间复制的文件。

```
[root@emr-header-1 opt]# hadoop fs -text oss://yang-hhht/hourly_table/manifest-2020-04-18.gz > current.lst
[root@emr-header-1 opt]# diff before.lst current.lst 
```

返回信息如下。

```
3a4,5
> {"path":"oss://yang-hhht/hourly_table/2017-02-01/03/5.log","baseName":"2017-02-01/03/5.log","srcDir":"oss://yang-hhht/hourly_table","size":4891}
> {"path":"oss://yang-hhht/hourly_table/2017-02-01/03/6.log","baseName":"2017-02-01/03/6.log","srcDir":"oss://yang-hhht/hourly_table","size":4891}
```

## --copyFromManifest

使用`--outputManifest`生成清单文件后，您可以使用`--copyFromManifest`指定`--outputManifest`生成的清单文件，并将dest目录生成的清单文件中包含的文件信息拷贝到新的目录下。

```
jindo distcp --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --previousManifest=oss://yang-hhht/hourly_table/manifest-2020-04-17.gz --copyFromManifest --parallelism 20
```

## --srcPrefixesFile

`--srcPrefixesFile`可以一次性完成多个文件夹的复制。

示例如下，查看hourly\_table下文件。

```
[root@emr-header-1 opt]# hdfs dfs -ls oss://yang-hhht/hourly_table
```

返回信息如下。

```
Found 4 items
drwxrwxrwx   -          0 1970-01-01 08:00 oss://yang-hhht/hourly_table/2017-02-01
drwxrwxrwx   -          0 1970-01-01 08:00 oss://yang-hhht/hourly_table/2017-02-02
```

执行以下命令，复制hourly\_table下文件到folders.txt。

```
jindo distcp --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --srcPrefixesFile file:///opt/folders.txt --parallelism 20
```

查看folders.txt文件的内容。

```
[root@emr-header-1 opt]# cat folders.txt 
```

返回信息如下。

```
hdfs://emr-header-1.cluster-50466:9000/data/incoming/hourly_table/2017-02-01
hdfs://emr-header-1.cluster-50466:9000/data/incoming/hourly_table/2017-02-02
```

## --groupBy和-targetSize

因为Hadoop可以从HDFS中读取少量的大文件，而不再读取大量的小文件，所以在大量小文件的场景下，您可以使用Jindo DistCp将小文件聚合为指定大小的大文件，以便于优化分析性能和降低成本。

例如，执行以下命令，查看如下文件夹中的数据。

```
[root@emr-header-1 opt]# hdfs dfs -ls /data/incoming/hourly_table/2017-02-01/03
```

返回信息如下。

```
Found 8 items
-rw-r-----   2 root hadoop       2252 2020-04-17 20:42 /data/incoming/hourly_table/2017-02-01/03/000151.sst
-rw-r-----   2 root hadoop       4891 2020-04-17 20:47 /data/incoming/hourly_table/2017-02-01/03/1.log
-rw-r-----   2 root hadoop       4891 2020-04-17 20:47 /data/incoming/hourly_table/2017-02-01/03/2.log
-rw-r-----   2 root hadoop       4891 2020-04-17 21:08 /data/incoming/hourly_table/2017-02-01/03/5.log
-rw-r-----   2 root hadoop       4891 2020-04-17 21:08 /data/incoming/hourly_table/2017-02-01/03/6.log
-rw-r-----   2 root hadoop       4891 2020-04-17 20:42 /data/incoming/hourly_table/2017-02-01/03/OPTIONS-000109
-rw-r-----   2 root hadoop       1016 2020-04-17 20:47 /data/incoming/hourly_table/2017-02-01/03/emp01.txt
-rw-r-----   2 root hadoop       1016 2020-04-17 20:47 /data/incoming/hourly_table/2017-02-01/03/emp06.txt
```

执行以下命令，将如下文件夹中的TXT文件合并为不超过10M的文件。

```
jindo distcp --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --targetSize=10 --groupBy='.*/([a-z]+).*.txt' --parallelism 20
```

经过合并后，可以看到两个TXT文件被合并成了一个文件。

```
[root@emr-header-1 opt]# hdfs dfs -ls oss://yang-hhht/hourly_table/2017-02-01/03/
Found 1 items
-rw-rw-rw-   1       2032 2020-04-17 21:18 oss://yang-hhht/hourly_table/2017-02-01/03/emp2
```

## --enableBalancePlan

在您要拷贝的数据大小均衡、小文件和大文件混合的场景下，因为DistCp默认的执行计划是随机进行文件分配的，所以您可以指定`--enableBalancePlan`来更改Jindo DistCp的作业分配计划，以达到更好的DistCp性能。

```
jindo distcp --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --enableBalancePlan --parallelism 20
```

**说明：** 该参数不支持和`--groupby`或`--targetSize`同时使用。

## --enableDynamicPlan

当您要拷贝的数据大小分化严重、小文件数据较多的场景下，您可以指定`--enableDynamicPlan`来更改Jindo DistCp的作业分配计划，以达到更好的DistCp性能。

```
jindo distcp --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --enableDynamicPlan --parallelism 20
```

**说明：** 该参数不支持和`--groupby`或`--targetSize`参数一起使用。

## --enableTransaction

`--enableTransaction`可以保证Job级别的完整性以及保证Job之间的事务支持。示例如下。

```
jindo distcp --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --enableTransaction --parallelism 20
```

## --diff

DistCp任务完成后，您可以使用`--diff`查看当前DistCp的文件差异。

例如，执行以下命令，查看/data/incoming/。

```
jindo distcp --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --diff
```

如果全部任务完成则会提示如下信息。

```
INFO distcp.JindoDistCp: distcp has been done completely
```

如果src的文件未能同步到dest上，则会在当前目录下生成manifest文件，您可以使用`--copyFromManifest`和`--previousManifest`拷贝剩余文件，从而完成数据大小和文件个数的校验。如果您的DistCp任务包含压缩或者解压缩，则`--diff`不能显示正确的文件差异，因为压缩或者解压缩会改变文件的大小。

```
jindo distcp --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --dest oss://yang-hhht/hourly_table --previousManifest=file:///opt/manifest-2020-04-17.gz --copyFromManifest --parallelism 20
```

**说明：** 如果您的`--dest`为HDFS路径，目前仅支持/path、hdfs://hostname:ip/path和hdfs://headerIp:ip/path的写法，暂不支持hdfs:///path、hdfs:/path和其他自定义写法。

## --queue

您可以使用--queue来指定本次DistCp任务所在Yarn队列的名称。

命令示例如下。

```
jindo distcp --src /data/incoming/hourly_table --dest oss://<your_bucket>/hourly_table --queue yarnqueue
```

## --bandwidth

您可以使用--bandwidth来指定本次DistCp任务所用的带宽（以MB为单位），避免占用过大带宽。

## 使用OSS Accesskey

在E-MapReduce外或者免密服务出现问题的情况下，您可以通过指定Accesskey来获得访问OSS的权限。您可以在命令中使用--key、--secret、--endPoint选项来指定Accesskey。

命令示例如下。

```
jindo distcp --src /data/incoming/hourly_table --dest oss://<your_bucket>/hourly_table --key yourkey --secret yoursecret --endPoint oss-cn-hangzhou.aliyuncs.com --parallelism 20
```

## 使用归档或低频写入OSS

在您的Distcp任务写入OSS时，您可以通过如下模式写入OSS，数据存储：

-   使用归档（--archive）示例命令如下。

    ```
    jindo distcp --src /data/incoming/hourly_table --dest oss://<your_bucket>/hourly_table --policy archive --parallelism 20
    ```

-   使用低频（--ia）示例命令如下。

    ```
    jindo distcp --src /data/incoming/hourly_table --dest oss://<your_bucket>/hourly_table --policy ia --parallelism 20
    ```


## 清理残留文件

在您的DistCp任务过程中，由于某种原因在您的目标目录下，产生未正确上传的文件，这部分文件通过uploadId的方式由OSS管理，并且对用户不可见时，您可以通过指定--cleanUpPending选项，指定任务结束时清理残留文件，或者您也可以通过OSS控制台进行清理。

命令示例如下。

```
jindo distcp --src /data/incoming/hourly_table --dest oss://<your_bucket>/hourly_table --cleanUpPending --parallelism 20
```

## 使用s3作为数据源

您可以在命令中使用--s3Key、--s3Secret、--s3EndPoint选项来指定连接s3的相关信息。

代码示例如下。

```
jindo distcp jindo-distcp-2.7.3.jar --src s3a://yourbucket/ --dest oss://<your_bucket>/hourly_table --s3Key yourkey --s3Secret yoursecret --s3EndPoint s3-us-west-1.amazonaws.com 
```

您可以配置s3Key、s3Secret、s3EndPoint在Hadoop的core-site.xml文件里 ，避免每次使用时填写Accesskey。

```
<configuration>
    <property>
        <name>fs.s3a.access.key</name>
        <value>xxx</value>
    </property>

    <property>
        <name>fs.s3a.secret.key</name>
        <value>xxx</value>
    </property>

    <property>
        <name>fs.s3.endpoint</name>
        <value>s3-us-west-1.amazonaws.com</value>
    </property>
</configuration>
```

此时代码示例如下。

```
jindo distcp /tmp/jindo-distcp-2.7.3.jar --src s3://smartdata1/ --dest s3://smartdata1/tmp --s3EndPoint  s3-us-west-1.amazonaws.com
```

## 查看Distcp Counters

执行以下命令，在MapReduce的Counter信息中查找Distcp Counters的信息。

```
Distcp Counters
        Bytes Destination Copied=11010048000
        Bytes Source Read=11010048000
        Files Copied=1001

Shuffle Errors
        BAD_ID=0
        CONNECTION=0
        IO_ERROR=0
        WRONG_LENGTH=0
        WRONG_MAP=0
        WRONG_REDUCE=0
```

**说明：** 如果您的DistCp操作中包含压缩或者解压缩文件，则`Bytes Destination Copied`和`Bytes Source Read`的大小可能不相等。

