---
keyword: [阿里云, EMR]
---

# 什么是E-MapReduce

阿里云E-MapReduce（简称EMR），是运行在阿里云平台上的一种大数据处理的系统解决方案。

## 简介

EMR构建于云服务器ECS上，基于开源的Apache Hadoop和Apache Spark，让您可以方便地使用Hadoop和Spark生态系统中的其他周边系统分析和处理数据。EMR还可以与阿里云其他的云数据存储系统和数据库系统（例如，阿里云OSS和RDS等）进行数据传输。

EMR的SmartData组件是EMR Jindo引擎的主要存储部分，为EMR各个计算引擎提供统一的存储优化、缓存优化、计算缓存加速优化和多个存储功能扩展。

**说明：**

-   关于Apache Hadoop的更多介绍，请参见[Apache Hadoop官网](http://hadoop.apache.org/)。
-   关于Apache Spark的更多介绍，请参见[Apache Spark官网](http://spark.apache.org/)。
-   关于Apache Hive的更多介绍，请参见[Apache Hive官网](http://hive.apache.org/)。
-   关于Apache HBase的更多介绍，请参见[Apache HBase官网](http://hbase.apache.org/book.html#architecture.client)。
-   关于SmartData的更多介绍，请参见[SmartData](/cn.zh-CN/集群类型/Hadoop集群/SmartData.md)。



## E-MapReduce的用途

以往在使用Hadoop和Spark等分布式处理系统时，您通常需要执行如下步骤。

![EMR使用流程](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7830482061/p62182.png)

在上述使用流程中，真正跟用户的应用逻辑相关的是步骤8~10，而步骤1~7都是前期准备工作，但这些前期准备工作都非常冗长繁琐。E-MapReduce提供了集群管理工具的集成解决方案，例如，主机选型、环境部署、集群搭建、集群配置、集群运行、作业配置、作业运行、集群管理和性能监控等。通过E-MapReduce，您可以从繁琐的集群构建相关的采购、准备和运维等工作中解放出来，只关心自己应用程序的处理逻辑即可。

此外，E-MapReduce还为您提供了灵活的搭配组合方式，您可以根据自己的业务特点选择不同的集群服务。例如，如果您的需求是对数据进行日常统计和简单的批量运算，则可以只选择在E-MapReduce中运行Hadoop服务；如果您有流式计算和实时计算的需求，则可以在Hadoop服务基础上再加入Spark服务。

## E-MapReduce的组成

E-MapReduce最核心也是用户直接面对的组件是集群。E-MapReduce集群是由一个或多个阿里云ECS实例组成的Hadoop和Spark集群。以Hadoop为例，每个ECS Instance上通常都运行了一些daemon进程（例如，NameNode、DataNode、ResouceManager和NodeManager），这些daemon 进程共同组成了Hadoop集群。其中运行NameNode和ResourceManager的节点称为Master节点，而运行DataNode和NodeManager的节点称为Slave节点。

例如，下图是一个包含一个Master节点和三个Slave节点的E-MapReduce集群。

![Master_Slave](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7830482061/p174277.png)

