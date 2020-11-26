# SmartData 3.1.0版本简介

SmartData组件是EMR Jindo引擎的存储部分，为EMR各个计算引擎提供统一的存储、缓存、计算优化以及功能扩展。SmartData组件主要包括JindoFS，JindoTable和相关工具集。本文介绍SmartData（3.1.0）版本的更新内容。

## 背景信息

SmartData 3.1.0版本使用时，限制信息如下：

-   JindoFS Cache模式支持元数据缓存，修改meta-cache开关，即可启用缓存模式，但仅建议在训练场景下打开使用，不建议在分析场景下使用（避免因配置使用不当导致跟其他写入路径出现不同步的情况）。
-   JindoFS Namespace名称，仅可使用字母、数字和中划线（-）。
-   JindoDistCp目前支持的大文件最大不能超过78 GB。
-   JindoDistCp block模式虽然支持checksum功能，但JindoDistCp暂不支持checksum功能。

## 版本变更

SmartData（3.1.0）版本与SmartData（3.0.0）版本的内容差异点如下：

-   新增内容
    -   [改写Jindo HDFS客户端路径](/cn.zh-CN/SmartData/SmartData基础使用（EMR-3.32.x版本）/改写Jindo HDFS客户端路径.md)
    -   [JindoFS数据管理策略](/cn.zh-CN/SmartData/SmartData基础使用（EMR-3.32.x版本）/JindoFS数据管理策略.md)
    -   [JindoTable表或分区的访问热度收集](/cn.zh-CN/SmartData/SmartData基础使用（EMR-3.32.x版本）/JindoTable表或分区的访问热度收集.md)
-   优化内容
    -   [Credential Provider使用说明](/cn.zh-CN/SmartData/SmartData基础使用（EMR-3.32.x版本）/Credential Provider使用说明.md)
    -   [支持Flink可恢复性写入JindoFS或OSS（SmartData 3.1.0版本）](/cn.zh-CN/SmartData/JindoFS 生态/支持Flink可恢复性写入JindoFS或OSS（SmartData 3.1.0版本）.md)

## 功能变更

-   [JindoFS存储优化](#section_zng_sm7_8ei)
-   [JindoFS缓存优化](#section_1gw_b3d_zcp)
-   [JindoTable计算优化](#section_r35_47p_s0h)
-   [JindoManager系统管理](#section_589_2zh_zj7)
-   [JindoTools工具箱](#section_e1m_heo_e48)
-   [JindoFS生态支持](#section_twl_lb9_865)

## JindoFS存储优化

-   支持文件的checksum功能，对齐开源HDFS checksum相关接口，支持MD5MD5CRC和COMPOSITE\_CRC两种算法；并且针对MD5MD5CRC算法实现了可传入磁盘的块大小（Block Size）的扩展接口以及支持相应的Shell命令，从而能够更好地支持和HDFS文件的比对。
-   文件透明压缩功能，支持对目录设置压缩策略，对目录下新写入的文件数据块进行压缩后存储到OSS后端存储上，对于一些高压缩比的数据，可大幅节省存储空间以及读写数据量。
-   支持写文件flush语义，调用flush接口后能够保证文件数据持久化到当前位置，并且可读。
-   解决了`hadoop fs -ls -R`命令在文件目录层级深，目录很多的情况下，出现由于线程处于等待状态致使命令无法执行的问题。
-   增强了`hadoop fs -stat`命令，支持显示atime和privilege等。
-   增加了Jindo HDFS客户端路径改写功能，以减少集群迁移时修改路径的工作量。

    详情请参见[改写Jindo HDFS客户端路径](/cn.zh-CN/SmartData/SmartData基础使用（EMR-3.32.x版本）/改写Jindo HDFS客户端路径.md)。


## JindoFS缓存优化

-   针对机器学习训练场景提供小文件缓存优化，大幅提升海量小文件的缓存效率和读取性能。
-   提供小文件目录预加载`cache`命令，大幅提升预加载效率。
-   支持数据缓存自动触发功能，您可以通过设置需要跟踪的目标目录以及时间间隔，每隔相应的时间间隔，系统自动发现用户目录下的新增文件，并自动触发Cache操作。

## JindoTable计算优化

-   JindoTable Dump TF格式支持二维数组。
-   Jindo mc dump支持Gzip压缩，可以使用`-c`参数。

## JindoManager系统管理

增加了JindoManager服务，集中负责Jindo系统的运维管理以及状态监控等附加功能，提供了Web UI服务，以及查看各项Jindo系统状态。

## JindoTools工具箱

-   Jindo DistCp工具针对小文件优化了Job Commiter的逻辑，大幅减少OSS的请求次数，提升大量小文件情况下DistCp的性能。
-   Jindo DistCp工具优化了文件分批，实现了更加合理的分批策略，提升了整体性能。

## JindoFS生态支持

-   Flink流式作业可恢复性地写入JindoFS，支持Block与Cache两种模式。结合可重发的数据源（例如Kafka），可以实现Exactly\_Once语义。
-   Flink实现熵注入功能。流式作业写入OSS或JindoFS时（Block与Cache两种模式均可），支持写入路径的熵注入（entropy injection）功能，即可以使用随机字符串匹配替换路径中的特定部分。该功能有利于提高写入效率。

    详情请参见[支持Flink可恢复性写入JindoFS或OSS（SmartData 3.1.0版本）](/cn.zh-CN/SmartData/JindoFS 生态/支持Flink可恢复性写入JindoFS或OSS（SmartData 3.1.0版本）.md)。

-   支持JindoFS Tensorflow Connector，通过实现Tensorflow Filesystem，支持原生的Tensorflow IO接口。支持Tensorflow 1.15及后续版本和Tensorflow 2.3后续版本。

