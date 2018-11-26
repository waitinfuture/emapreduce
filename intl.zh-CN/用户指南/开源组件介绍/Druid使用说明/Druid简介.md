# Druid简介 {#concept_csl_jkd_z2b .concept}

Druid是Metamarkets公司（一家为在线媒体或广告公司提供数据分析服务的公司）推出的一个分布式内存实时分析系统，用于解决如何在大规模数据集下进行快速的、交互式的查询和分析。

## 基本特点 {#section_jcb_mkd_z2b .section}

Druid具有以下特点：

-   亚秒级OLAP查询，包括多维过滤、ad-hoc的属性分组、快速聚合数据等等。
-   实时的数据消费，真正做到数据摄入实时、查询结果实时。
-   高效的多租户能力，最高可以做到几千用户同时在线查询。
-   扩展性强，支持PB级数据、千亿级事件快速处理，支持每秒数千查询并发。
-   极高的高可用保障，支持滚动升级。

## 应用场景 {#section_lcb_mkd_z2b .section}

实时数据分析是Druid最典型的使用场景。该场景涵盖的面很广，例如：

-   实时指标监控
-   推荐模型
-   广告平台
-   搜索模型

这些场景的特点都是拥有大量的数据，且对数据查询的时延要求非常高。在实时指标监控中，系统问题需要在出现的一刻被检测到并被及时给出报警。在推荐模型中，用户行为数据需要实时采集，并及时反馈到推荐系统中。用户几次点击之后系统就能够识别其搜索意图，并在之后的搜索中推荐更合理的结果。

## Druid架构 {#section_ncb_mkd_z2b .section}

Druid拥有优秀的架构设计，多个组件协同工作，共同完成数据从摄取到索引、存储、查询等一系列流程。

下图是Druid工作层（数据索引以及查询）包含的组件。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17905/153829585110852_zh-CN.png)

-   Realtime 组件负责数据的实时摄入。
-   Broker 阶段负责查询任务的分发以及查询结果的汇总，并将结果返回给用户。
-   Historical节点负责索引后的历史数据的存储，数据存储在deep storage。deep storage可以是本地，也可以是HDFS等分布式文件系统。
-   Indexing service 包含两个组件（图中未画出）。
    -   Overlord 组件负责索引任务的管理、分发。
    -   MiddleManager 负责索引任务的具体执行。

下图是 Druid segments（Druid 索引文件）管理层所涉及的组件。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17905/153829585110853_zh-CN.png)

-   Zookeeper 负责存储集群的状态以及作为服务发现组件，例如集群的拓扑信息，overlord leader 的选举，indexing task 的管理等等。
-   Coordinator 负责 segments 的管理，如 segments 下载、删除以及如何在 historical 之间做均衡等等。
-   Metadata storage 负责存储 segments 的元信息，以及管理集群各种各样的持久化或临时性数据，比如配置信息、审计信息等等。

## 产品优势 {#section_o4g_wkd_z2b .section}

E-MapReduce Druid 基于开源 Druid 做了大量的改进，包括与E-MapReduce、阿里云周边生态的集成、方便的监控与运维支持、易用的产品接口等等，真正做到了即买即用和7\*24免运维。

EMR Druid 目前支持的特性如下所示：

-   支持以OSS作为deep storage
-   支持将OSS文件作为批量索引的数据来源
-   支持将元数据存储到 RDS
-   集成了Superset工具
-   方便地扩容、缩容（缩容针对 task 节点）
-   丰富的监控指标和告警规则
-   坏节点迁移
-   支持高安全
-   支持HA

