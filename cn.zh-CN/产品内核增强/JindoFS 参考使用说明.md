# JindoFS 参考使用说明 {#concept_473658 .concept}

JindoFS 是一种云原生的文件系统，结合 OSS 和本地存储，成为 EMR 产品的新一代存储系统，为上层计算提供了高效可靠的存储。本文主要说明 JindoFS 的配置使用方式，以及介绍一些典型的应用场景。

## 概述 {#section_ed7_ndi_45t .section}

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/381399/156015576648599_zh-CN.png)

JindoFS 采用了本地存储和 OSS 的异构多备份机制，Storage Service 提供了数据存储能力，首先使用 OSS 作为存储后端，保证数据的高可靠性，同时利用本地存储实现冗余备份，利用本地的备份，可以加速数据读取；另外，JindoFS 的元数据通过本地服务 Namespace Service 管理，从而保证了元数据操作的性能（和 HDFS 元数据操作性能相当）。EMR-3.20.0 及以上版本支持 Jindo FS，您可以在创建集群时勾选相关服务来使用 JindoFS。

## 环境准备 {#section_dny_u6x_m00 .section}

-   创建集群

    选择 EMR-3.20.0 及以上版本，勾选可选服务中的 **SmartData** 和 **Bigboot**，具体操作步骤请参见[创建集群](../../../../cn.zh-CN/集群规划与配置/集群配置/创建集群.md#)。Bigboot 服务提供了 EMR 平台上的基础的分布式数据管理交互服务以及一些组件管理监控和支持性服务，SmartData 服务基于 Bigboot 之上对应用层提供了 JindoFS 文件系统。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/381399/156015576648600_zh-CN.png)

-   配置集群

    SmartData 提供的 JindoFS 文件系统使用 OSS 作为存储后端，因此在使用 JindoFS 之前需配置一些 OSS 相关参数。下面提供两种配置方式，第一种是先创建好集群，修改 Bigboot 相关参数，需重启 SmartData 服务生效；第二种是创建集群过程中添加自定义配置，这样集群创建好后相关服务就能按照自定义参数启动：

    -   集群创建好后参数初始化

        所有 JindoFS 相关配置都在 Bigboot 组件中，配置如下图所示，红框中为必填的配置项，oss.access.bucket 为 OSS bucket 名字，oss.data-dir 为 JindoFS 在 OSS bucket 中所使用的目录（注：该目录为 JindoFS 后端存储目录，生成的数据不能人为破坏，并且保证该目录仅用于 JindoFS 后端存储，JindoFS 在写入数据时会自动创建用户所配置的目录，无需在 OSS 上事先创建）。oss.access.endpoint、oss.access.key、oss.access.secret 含义可参考页面注解，一般考虑到性能和稳定性，推荐使用同 region 下的 OSS bucket 作为存储后端，这种情况下，EMR 集群能够免密访问 OSS，无需配置这几项。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/381399/156015576648601_zh-CN.png)

        配置完成后保存并部署，然后在 SmartData 服务中重启所有组件，即开始使用 JindoFS。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/381399/156015576748602_zh-CN.png)

    -   创建集群时添加自定义配置

        EMR 集群在创建集群时支持添加自定义配置，以同 region 下免密访问 OSS 为例，如下图勾选**软件自定义配置**，添加如下配置，配置 oss.data-dir 和 oss.access.bucket。

        ``` {#codeblock_k7q_23k_z6p}
        [
            {         
            "ServiceName":"BIGBOOT",
            "FileName":"bigboot",
            "ConfigKey":"oss.data-dir",
            "ConfigValue":"jindoFS-1"
            },
            {
            "ServiceName":"BIGBOOT",
            "FileName":"bigboot",
            "ConfigKey":"oss.access.bucket",
            "ConfigValue":"oss-bucket-name"
            }
        ]
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/381399/156015576748604_zh-CN.png)


## 使用 JindoFS {#section_jca_e6y_zza .section}

JindoFS 使用上与 HDFS 类似，提供 jfs 前缀，将 jfs 替代 hdfs 即可使用。简单示例：

``` {#codeblock_d6a_6i7_cwk}
hadoop fs -ls jfs:/// hadoop fs -mkdir jfs:///test-dirhadoop fs -put test.log jfs:///test-dir/
```

目前，JindoFS 能够支持 EMR 集群上的 Hadoop、Hive、Spark 的作业进行访问，其余组件尚未完全支持。

## 应用场景 {#section_qrf_zrj_sf6 .section}

EMR 目前提供了三种大数据存储系统，EMR OssFileSystem、EMR HDFS 和 EMR JindoFS，OssFileSystem 和 JindoFS 都是云上存储的解决方案，下面比较了这三种存储系统各自的特点，同时也将开源 OSS 放入比较。

|特点|开源 OSS|EMR OssFileSystem|EMR HDFS|EMR JindoFS|
|--|------|-----------------|--------|-----------|
|存储空间|海量|海量|取决于集群规模|海量|
|可靠性|高|高|高|高|
|吞吐率因素|服务端|集群内磁盘缓存|集群内磁盘|集群内磁盘|
|元数据效率|慢|中|快|快|
|扩容操作|容易|容易|容易|容易|
|缩容操作|容易|容易|需 Decommission|容易|
|数据本地化|无|弱|强|较强|

JindoFS 具有以下几个特点：

-   海量弹性的存储空间，基于 OSS 作为存储后端，存储不受限于本地集群，而且本地集群能够自由弹性伸缩。
-   能够利用本地集群的存储资源加速数据读取，适合具有一定本地存储能力的集群，能够利用有限的本地存储提升吞吐率，特别对于一写多读的场景效果显著。
-   元数据操作效率高，能够与 HDFS 相当，能够有效规避 OSS 文件系统元数据操作耗时以及高频访问下可能引发不稳定的痛点。
-   能够最大限度保证执行作业时的数据本地化，减少网络传输的压力，进一步提升读取性能。

## 磁盘空间水位控制 {#section_r89_879_ml3 .section}

JindoFS 后端基于 OSS，可以提供海量的存储，但是本地盘的容量是有限的，因此 JindoFS 会自动淘汰本地较冷的数据备份。我们提供了node.data-dirs.watermark.high.ratio 和 node.data-dirs.watermark.low.ratio 这两个参数用来调节本地存储的使用容量，值均为 0～1 的小数表示使用比例，JindoFS 默认使用所有数据盘，每块盘的使用容量默即为数据盘大小。前者表示使用量上水位比例，每块数据盘的 JindoFS 占用的空间到达上水位即会开始清理淘汰；后者表示使用量下水位比例，触发清理后会将 JindoFS 的占用空间清理到下水位。用户可以通过设置上水位比例调节期望分给 JindoFS 的磁盘空间，下水位必须小于上水位，设置合理的值即可。

