# SmartData版本说明

SmartData组件是EMR JindoFS引擎的存储部分，为EMR各个计算引擎提供统一的存储、缓存和计算优化以及功能扩展。SmartData组件主要包括JindoFS，JindoTable和相关工具集。本文介绍SmartData（3.0.0）版本的更新内容。

## JindoFS存储优化

-   改进JindoFS Namespace服务单机配置，单机情况下也可以更新并异步写入元数据至Tablestore。
-   移除JindoFS Namespace服务的Tablestore作为元数据后端的配置，不再支持基于Tablestore的HA方案。
-   支持归档存储，允许文件数据按照OSS归档类型进行存储，以节省成本。
-   提供JindoFS分层存储的Archive、Unarchive和Status命令，允许归档至指定目录，查看归档操作进度和相关状态。
-   提供JindoFS ls2命令，允许查看文件信息。
-   支持JindoFS存储系统fsimage的离线导出和分析查询。
-   支持跨集群访问JindoFS存储系统。

JindoFS分层存储命令详情请参见[JindoFS分层存储命令使用说明](/cn.zh-CN/JindoFS/JindoFS基础使用（EMR-3.30.x版本）/JindoFS分层存储命令使用说明.md)

## JindoFS缓存优化

-   改进缓存数据磁盘组织，解除对系统盘的依赖，实现数据盘之间完全独立，增强磁盘下线操作。
-   改进缓存服务，增强节点容错处理和节点下线操作。
-   改进缓存块写入磁盘的选择策略，默认支持轮询（Round Robin）。
-   改进读写流程，增强容错处理。
-   提供JindoFS分层存储的Cache、Uncache和Status命令，允许缓存至指定目录，支持数据预加载，查看缓存进度和相关状态。
-   优化小文件占用缓存空间的问题，准确地统计相关指标。

## JindoTable计算优化

-   提供JindoTable Optimize命令，支持优化Hive表操作，例如分区小文件合并。
-   提供JindoTable Archive、Unarchive和Status命令，允许归档至指定表和分区，查看归档操作进度和相关状态。
-   支持JindoTable Cache、Uncache和Status命令，允许缓存至指定表和分区，支持数据预加载，查看缓存进度和相关状态。
-   支持导出MaxCompute表至JindoFS缓存系统上，以实现机器学习训练前结构化数据的预加载机制。

JindoTable详情请参见[JindoTable使用说明](/cn.zh-CN/JindoFS/JindoFS基础使用（EMR-3.30.x版本）/JindoTable使用说明.md)

## JindoFS OSS扩展和支持

-   支持在客户端进行Ranger权限集成，获取OSS各种操作，通过JindoFS服务记录进行Ranger权限检查。
-   支持在客户端进行操作审计，获取OSS各种操作，通过JindoFS服务记录操作记录，作为审计用途。
-   支持Hadoop Credentials Provider框架，允许按照Hadoop常用方式指定OSS的Accesskey配置。
-   支持Flink Connector，允许Flink引擎将OSS作为source、sink和checkpoint存储。
-   提供JindoFS OSS SDK（Hadoop Connector）轻量版本（lite），主要适用于非标准环境，例如用户的IDC（Internet Data Center）集群环境。

## JindoManager系统管理

支持通过UI来查看JindoFS存储系统上的系统状态、文件统计和缓存系统上的缓存指标统计。

## JindoTools工具箱

改进JindoDistCp工具的分发机制，针对EMR集群内使用场景和非EMR集群环境使用场景，分别使用不同的发行包。

JindoDistCp提供轻量版本（lite），主要适用于非标准环境，例如用户的IDC集群环境。

