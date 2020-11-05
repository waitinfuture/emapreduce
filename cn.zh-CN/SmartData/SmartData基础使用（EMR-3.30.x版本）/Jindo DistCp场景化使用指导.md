# Jindo DistCp场景化使用指导

本文通过场景化为您介绍如何使用Jindo DistCp。

-   已创建EMR-3.30.0版本的集群，详情请参见[创建集群](/cn.zh-CN/集群管理/集群配置/创建集群.md)。
-   已安装JDK 1.8。
-   根据您使用的Hadoop版本，下载jindo-distcp-<version\>.jar。
    -   Hadoop 2.7及后续版本，请下载[jindo-distcp-3.0.0.jar](https://smartdata-binary.oss-cn-shanghai.aliyuncs.com/Jindo-distcp-3.0/Hadoop2.7New/jindo-distcp-3.0.0.jar)。
    -   Hadoop 3.x系列版本，请下载[jindo-distcp-3.0.0.jar](https://smartdata-binary.oss-cn-shanghai.aliyuncs.com/Jindo-distcp-3.0/Hadoop3.x/jindo-distcp-3.0.0.jar)。

## 场景预览

Jindo DistCp常用使用场景如下所示：

-   [场景一：导入HDFS数据至OSS，需要使用哪些参数？如果数据量很大、文件很多（百万千万级别）时，该使用哪些参数优化？](#section_740_dme_sck)
-   [场景二：使用JindoDistCp成功导完数据后，如何验证数据完整性？](#section_sgm_ydo_etj)
-   [场景三：导入HDFS数据至OSS时，DistCp任务存在随时失败的情况，该使用哪些参数支持断点续传？](#section_vvv_0hg_97i)
-   [场景四：成功导入HDFS数据至OSS，数据不断增量增加，在Distcp过程中可能已经产生了新文件，该使用哪些参数处理？](#section_e41_h64_8vu)
-   [场景五：如果需要指定JindoDistCp作业在Yarn上的队列以及可用带宽，该使用哪些参数？](#section_ap9_haz_24g)
-   [场景五：如果需要指定JindoDistCp作业在Yarn上的队列以及可用带宽，该使用哪些参数？](#section_ap9_haz_24g)
-   [场景六：当通过低频或者归档形式写入OSS，该使用哪些参数？](#section_6ur_r7j_j8r)
-   [场景七：针对小文件比例和文件大小情况，该使用哪些参数来优化传输速度？](#section_fxa_emv_e5o)
-   [场景八：如果需要使用S3作为数据源，该使用哪些参数？](#section_0wf_8x1_8cl)
-   [场景九：如果需要写入文件至OSS上并压缩（LZO和GZ格式等）时，该使用哪些参数？](#section_8gv_uy0_h6h)
-   [场景十：如果需要把本次Copy中符合特定规则或者同一个父目录下的部分子目录作为Copy对象，该使用哪些参数？](#section_7u3_adw_k5b)
-   [场景十一：如果想合并符合一定规则的文件，以减少文件个数，该使用哪些参数？](#section_5si_qbd_a4j)
-   [场景十二：如果Copy完文件，需要删除原文件，只保留目标文件时，该使用哪些参数？](#section_qsc_qdv_ann)
-   [场景十三：如果不想将OSS AccessKey这种参数写在命令行里，该如何处理？](#section_qdv_gjx_73g)

## 场景一：导入HDFS数据至OSS，需要使用哪些参数？如果数据量很大、文件很多（百万千万级别）时，该使用哪些参数优化？

如果您使用的不是EMR环境，当从HDFS上往OSS传输数据时，需要满足以下几点：

-   HDFS可访问，有读数据权限。
-   需要提供OSS的AccessKey（AccessKey ID和AccessKey Secret），以及Endpoint信息，且该AccessKey具有写目标Bucket的权限。
-   OSS Bucket不能为归档类型。
-   环境可以提交MapReduce任务。
-   已下载JindoDistCp JAR包

本场景示例如下。

```
hadoop jar jindo-distcp-<version>.jar --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --ossKey yourkey --ossSecret yoursecret --ossEndPoint oss-cn-hangzhou.aliyuncs.com --parallelism 10
```

**说明：** 各参数含义请参见[Jindo DistCp使用说明](/cn.zh-CN/SmartData/SmartData基础使用（EMR-3.29.x版本）/Jindo DistCp使用说明.md)。

当您的数量很大，文件数量很多，例如百万千万级别时，您可以增大parallelism，以增加并发度，还可以开启--enableBatch参数来进行优化。优化命令如下。

```
hadoop jar jindo-distcp-<version>.jar --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --ossKey yourkey --ossSecret yoursecret --ossEndPoint oss-cn-hangzhou.aliyuncs.com --parallelism 500 --enableBatch
```

## 场景二：使用JindoDistCp成功导完数据后，如何验证数据完整性？

您可以通过以下两种方式验证数据完整性：

-   JindoDistCp Counters

    您可以在MapReduce任务结束的Counter信息中，获取Distcp Counters的信息。

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

    参数含义如下：

    -   Bytes Destination Copied：表示目标端写文件的字节数大小。
    -   Bytes Source Read：表示源端读文件的字节数大小。
    -   Files Copied：表示成功Copy的文件数。
-   JindoDistCp --diff

    您可以使用`--diff`命令，进行源端和目标端的文件比较，该命令会对文件名和文件大小进行比较，记录遗漏或者未成功传输的文件，存储在提交命令的当前目录下，生成manifest文件。

    在[场景一](#section_740_dme_sck)的基础上增加`--diff`参数即可，示例如下。

    ```
    hadoop jar jindo-distcp-<version>.jar --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --ossKey yourkey --ossSecret yoursecret --ossEndPoint oss-cn-hangzhou.aliyuncs.com --diff
    ```

    当全部文件传输成功时，系统返回如下信息。

    ```
    INFO distcp.JindoDistCp: distcp has been done completely
    ```


## 场景三：导入HDFS数据至OSS时，DistCp任务存在随时失败的情况，该使用哪些参数支持断点续传？

在[场景一](#section_740_dme_sck)的基础上，如果您的Distcp任务因为各种原因中间失败了，而此时您想支持断点续传，只Copy剩下未Copy成功的文件，此时需要您在进行上一次Distcp任务完成后进行如下操作：

1.  增加一个`--diff`命令，查看所有文件是否都传输完成。

    ```
    hadoop jar jindo-distcp-<version>.jar --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --ossKey yourkey --ossSecret yoursecret --ossEndPoint oss-cn-hangzhou.aliyuncs.com --diff
    ```

    当所有文件都传输完成，则会提示如下信息。否则，执行。

    ```
    INFO distcp.JindoDistCp: distcp has been done completely.
    ```

2.  文件没有传输完成时会生成manifest文件，您可以使用`--copyFromManifest`和`--previousManifest`命令进行剩余文件的Copy。示例如下。

    ```
    hadoop jar jindo-distcp-<version>.jar --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --dest oss://yang-hhht/hourly_table --previousManifest=file:///opt/manifest-2020-04-17.gz --copyFromManifest --parallelism 20
    ```

    `file:///opt/manifest-2020-04-17.gz`为您当前执行命令的本地路径。


## 场景四：成功导入HDFS数据至OSS，数据不断增量增加，在Distcp过程中可能已经产生了新文件，该使用哪些参数处理？

1.  未产生上一次Copy的文件信息，需要指定生成manifest文件，记录已完成的文件信息。

    在[场景一](#section_740_dme_sck)的基础上增加`--outputManifest=manifest-2020-04-17.gz`和`--requirePreviousManifest=false`两个信息，示例如下。

    ```
    hadoop jar jindo-distcp-<version>.jar --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --ossKey yourkey --ossSecret yoursecret --ossEndPoint oss-cn-hangzhou.aliyuncs.com --outputManifest=manifest-2020-04-17.gz --requirePreviousManifest=false --parallelism 20
    ```

    参数含义如下：

    -   `--outputManifest`：指定生成的manifest文件，文件名称自定义但必须以gz结尾，例如manifest-2020-04-17.gz，该文件会存放在`--dest`指定的目录下。
    -   `--requirePreviousManifest`：无已生成的历史manifest文件信息。
2.  当前一次Distcp任务结束后，源目录可能已经产生了新文件，这时候需要增量同步新文件。

    在[场景一](#section_740_dme_sck)的基础上增加`--outputManifest=manifest-2020-04-17.gz`和`--previousManifest=oss://yang-hhht/hourly_table/manifest-2020-04-17.gz`两个信息，示例如下。

    ```
    hadoop jar jindo-distcp-<version>.jar --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --ossKey yourkey --ossSecret yoursecret --ossEndPoint oss-cn-hangzhou.aliyuncs.com --outputManifest=manifest-2020-04-17.gz --requirePreviousManifest=false --parallelism 20
    ```

    ```
    hadoop jar jindo-distcp-2.7.3.jar --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --ossKey yourkey --ossSecret yoursecret --ossEndPoint oss-cn-hangzhou.aliyuncs.com --outputManifest=manifest-2020-04-18.gz --previousManifest=oss://yang-hhht/hourly_table/manifest-2020-04-17.gz --parallelism 10
    ```

3.  重复执行[步骤2](#li_0y6_txl_zhm)，不断同步增量文件。

## 场景五：如果需要指定JindoDistCp作业在Yarn上的队列以及可用带宽，该使用哪些参数？

在[场景一](#section_740_dme_sck)的基础上需要增加两个参数。两个参数可以配合使用，也可以单独使用。

-   `--queue`：指定Yarn队列的名称。
-   `--bandwidth`：指定带宽的大小，单位为MB。

示例如下。

```
hadoop jar jindo-distcp-<version>.jar --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --ossKey yourkey --ossSecret yoursecret --ossEndPoint oss-cn-hangzhou.aliyuncs.com --queue yarnqueue --bandwidth 6 --parallelism 10
```

## 场景六：当通过低频或者归档形式写入OSS，该使用哪些参数？

-   当通过归档形式写入OSS时，需要在在场景一的基础上增加`--archive`参数，示例如下。

    ```
    hadoop jar jindo-distcp-<version>.jar --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --ossKey yourkey --ossSecret yoursecret --ossEndPoint oss-cn-hangzhou.aliyuncs.com --archive --parallelism 20
    ```

-   当通过低频形式写入OSS时，需要在在场景一的基础上增加`--ia`参数，示例如下。

    ```
    hadoop jar jindo-distcp-<version>.jar --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --ossKey yourkey --ossSecret yoursecret --ossEndPoint oss-cn-hangzhou.aliyuncs.com --ia --parallelism 20
    ```


## 场景七：针对小文件比例和文件大小情况，该使用哪些参数来优化传输速度？

-   小文件较多，大文件较大情况。

    如果要Copy的所有文件中小文件的占比较高，大文件较少，但是单个文件数据较大，在正常流程中是按照随机方式来进行Copy文件分配，此时如果不做优化很可能造成一个Copy进程分配到大文件的同时也分配到很多小文件，不能发挥最好的性能。

    在[场景一](#section_740_dme_sck)的基础上增加`--enableDynamicPlan`开启优化选项，但不能和`--enableBalancePlan`一起使用。示例如下。

    ```
    hadoop jar jindo-distcp-<version>.jar --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --ossKey yourkey --ossSecret yoursecret --ossEndPoint oss-cn-hangzhou.aliyuncs.com --enableDynamicPlan --parallelism 10
    ```

    优化对比如下。

    ![优化](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/2413073061/p176261.png)

-   文件总体均衡，大小差不多情况。

    如果您要Copy的数据里文件大小总体差不多，比较均衡，您可以使用`--enableBalancePlan`优化。示例如下。

    ```
    hadoop jar jindo-distcp-<version>.jar --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --ossKey yourkey --ossSecret yoursecret --ossEndPoint oss-cn-hangzhou.aliyuncs.com --enableBalancePlan --parallelism 10
    ```

    优化对比如下。

    ![优化2](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/2413073061/p176262.png)


## 场景八：如果需要使用S3作为数据源，该使用哪些参数？

需要在场景一的基础上替换OSS的AccessKey和endPoint信息转换成S3参数：

-   `--s3Key`：连接S3的AccessKey ID。
-   `--s3Secret`：连接S3的AccessKey Secret。
-   `--s3EndPoint`：连接S3的endPoint信息。

示例如下。

```
hadoop jar jindo-distcp-<version>.jar --src s3a://yourbucket/ --dest oss://yang-hhht/hourly_table --s3Key yourkey --s3Secret yoursecret --s3EndPoint s3-us-west-1.amazonaws.com --parallelism 10
```

## 场景九：如果需要写入文件至OSS上并压缩（LZO和GZ格式等）时，该使用哪些参数？

如果您想压缩写入的目标文件，例如LZO和GZ等格式，以降低目标文件的存储空间，您可以使用`--outputCodec`参数来完成。

需要在[场景一](#section_740_dme_sck)的基础上增加`--outputCodec`参数，示例如下。

```
hadoop jar jindo-distcp-<version>.jar --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --ossKey yourkey --ossSecret yoursecret --ossEndPoint oss-cn-hangzhou.aliyuncs.com --outputCodec=gz --parallelism 10
```

JindoDistCp支持编解码器GZIP、GZ、LZO、LZOP和SNAPPY以及关键字none和keep（默认值）。这些关键字含义如下：

-   none表示保存为未压缩的文件。如果文件已压缩，则Jindo DistCp会将其解压缩。
-   keep表示不更改文件压缩形态，按原样复制。

**说明：** 如您在开源Hadoop集群环境中使用LZO压缩功能，则您需要安装gplcompression的native库和hadoop-lzo包，

## 场景十：如果需要把本次Copy中符合特定规则或者同一个父目录下的部分子目录作为Copy对象，该使用哪些参数？

-   如果您需要将Copy列表中符合一定规则的文件进行Copy，需要在[场景一](#section_740_dme_sck)的基础上增加`--srcPattern`参数，示例如下。

    ```
    hadoop jar jindo-distcp-<version>.jar --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --ossKey yourkey --ossSecret yoursecret --ossEndPoint oss-cn-hangzhou.aliyuncs.com --srcPattern .*\.log --parallelism 10
    ```

    `--srcPattern`：进行过滤的正则表达式，符合规则进行Copy，否则抛弃。

-   如果您需要Copy同一个父目录下的部分子目录，需要在[场景一](#section_740_dme_sck)的基础上增加`--srcPrefixesFile`参数。

    ```
    hadoop jar jindo-distcp-<version>.jar --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --ossKey yourkey --ossSecret yoursecret --ossEndPoint oss-cn-hangzhou.aliyuncs.com --srcPrefixesFile file:///opt/folders.txt --parallelism 20
    ```

    `--srcPrefixesFile`：存储需要Copy的同父目录的文件夹列表的文件。

    示例中的folders.txt内容如下。

    ```
    hdfs://emr-header-1.cluster-50466:9000/data/incoming/hourly_table/2017-02-01
    hdfs://emr-header-1.cluster-50466:9000/data/incoming/hourly_table/2017-02-02
    ```


## 场景十一：如果想合并符合一定规则的文件，以减少文件个数，该使用哪些参数？

需要在[场景一](#section_740_dme_sck)的基础上增加如下参数：

-   `--targetSize`：合并文件的最大大小，单位MB。
-   `--groupBy`：合并规则，正则表达式。

示例如下。

```
hadoop jar jindo-distcp-<version>.jar --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --ossKey yourkey --ossSecret yoursecret --ossEndPoint oss-cn-hangzhou.aliyuncs.com --targetSize=10 --groupBy='.*/([a-z]+).*.txt' --parallelism 20
```

## 场景十二：如果Copy完文件，需要删除原文件，只保留目标文件时，该使用哪些参数？

需要在[场景一](#section_740_dme_sck)的基础上，增加`--deleteOnSuccess`参数，示例如下。

```
hadoop jar jindo-distcp-<version>.jar --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --ossKey yourkey --ossSecret yoursecret --ossEndPoint oss-cn-hangzhou.aliyuncs.com --deleteOnSuccess --parallelism 10
```

## 场景十三：如果不想将OSS AccessKey这种参数写在命令行里，该如何处理？

通常您需要将OSS AccessKey和endPoint信息写在参数里，但是JindoDistcp可以将OSS的AccessKey ID、AccessKey Secret和endpoint预先写在Hadoop的core-site.xml文件里 ，以避免使用时多次填写的问题。

-   如果您需要保存OSS的Accesskey相关信息，您需要将以下信息保存在core-site.xml中。

    ```
    <configuration>
        <property>
            <name>fs.jfs.cache.oss-accessKeyId</name>
            <value>xxx</value>
        </property>
    
        <property>
            <name>fs.jfs.cache.oss-accessKeySecret</name>
            <value>xxx</value>
        </property>
    
        <property>
            <name>fs.jfs.cache.oss-endpoint</name>
            <value>oss-cn-xxx.aliyuncs.com</value>
        </property>
    </configuration>
    ```

-   如果您需要保存S3的AccessKey相关信息，您需要将以下信息保存在core-site.xml中。

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


