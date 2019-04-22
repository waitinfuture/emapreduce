# 什么是 E-MapReduce {#concept_hcj_lgy_w2b .concept}

阿里云 Elastic MapReduce（E-MapReduce）是运行在阿里云平台上的一种大数据处理的系统解决方案。

E-MapReduce 构建于阿里云云服务器 ECS 上，基于开源的 Apache Hadoop 和 Apache Spark，让您可以方便地使用 Hadoop 和 Spark 生态系统中的其他周边系统（如 Apache Hive、Apache Pig、HBase 等）来分析和处理自己的数据。不仅如此，E-MapReduce 还可以方便的与阿里云其他的云数据存储系统和数据库系统（如阿里云 OSS、阿里云 RDS 等。）进行数据传输。

## E-MapReduce 的用途 {#section_tqz_mhy_w2b .section}

当您想要使用 Hadoop、Spark 等分布式处理系统的时候，通常需要经历如下的步骤：

1.  评估业务特点
2.  选择机器类型
3.  采购机器
4.  准备硬件环境
5.  安装操作系统
6.  部署 Hadoop 和 Spark 等 App
7.  启动集群
8.  编写应用程序
9.  运行作业
10. 获取数据等一系列操作

在这些流程中，真正跟用户的应用逻辑相关的是从第 8 步才开始，第 1-7 步的各项工作都是前期的准备工作，通常这个前期工作都非常冗长繁琐。而 E-MapReduce 提供了集群管理工具的集成解决方案，如主机选型、环境部署、集群搭建、集群配置、集群运行、作业配置、作业运行、集群管理、性能监控等。

通过使用 E-MapReduce，您可以从集群构建各种繁琐的采购、准备、运维等工作中解放出来，只关心自己应用程序的处理逻辑即可。此外，E-MapReduce 还给用户提供了灵活的搭配组合方式，您可以根据自己的业务特点选择不同的集群服务。例如，如果您的需求是对数据进行日常统计和简单的批量运算，则可以只选择在 E-MapReduce 中运行 Hadoop 服务；而如果您有流式计算和实时计算的需求，则可以在 Hadoop 服务基础上再加入 Spark 服务。

## E-MapReduce 的组成 {#section_xgt_23y_w2b .section}

E-MapReduce 最核心也是用户直接面对的组件是集群。E-MapReduce 集群是由一个或多个阿里云 ECS Instance 组成的 Hadoop 和 Spark 集群。以 Hadoop 为例，每个 ECS Instance 上，通常都运行了一些 daemon 进程（如 namenode、datanode、resoucemanager 和 nodemanager），这些 daemon 进程就组成了 Hadoop 集群。运行 namenode 和 resourcemanager 的节点被称为 master 节点，而运行 datanode 和 nodemanager 的节点称为 slave 节点。

例如，下图表示了一个包含 1 个 master 节点和 3 个 slave 节点的 E-MapReduce 集群：

![1个 master 节点和3个 slave 节点的 E-MapReduce 集群架构](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17824/15559147179988_zh-CN.jpg)

