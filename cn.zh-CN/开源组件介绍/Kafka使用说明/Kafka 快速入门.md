# Kafka 快速入门 {#concept_ot1_5bx_y2b .concept}

从 E-MapReduce 3.4.0 版本开始将支持 Kafka 服务。

## 创建Kafka集群 {#section_w3z_vbx_y2b .section}

在 E-MapReduce 控制台创建集群时，选择集群类型为 Kafka，则会创建一个默认只包含 Kafka 组件的集群，除了基础组件外包括 Zookeeper，Kafka 和 KafkaManager 三个组件。每个节点将只部署一个 Kafka broker。我们建议您的Kafka 集群是一个专用集群，不要和 Hadoop 相关服务混部在一起。

## 本地盘Kafka集群 {#section_x3z_vbx_y2b .section}

为了更好地降低单位成本以及应对更大的存储需求，E-MapReduce 将在 EMR-3.5.1 版本开始支持基于本地盘（D1类簇机型，详情请参见[实例规格族](../../../../intl.zh-CN/实例/实例规格族.md#)介绍文档）的 Kafka 集群。相比较云盘，本地盘 Kafka 集群有如下特点：

-   实例配备大容量、高吞吐 SATA HDD 本地盘，单磁盘 190 MB/s 顺序读写性能，单实例最大 5 GB/s 存储吞吐能力。
-   本地存储价格比 SSD 云盘降低 97%。
-   更高网络性能，最大 17 Gbit/s实例间网络带宽，满足业务高峰期实例间数据交互需求。

但本地盘机型也有特殊之处：

|操作|本地磁盘数据状态|说明|
|:-|:-------|:-|
|操作系统重启/控制台重启/强制重启|保留|本地磁盘存储卷保留，数据保留|
|操作系统关机/控制台停止/强制停止|保留|本地磁盘存储卷保留，数据保留|
|控制台上释放（实例）|擦除|本地磁盘存储卷擦除，数据不保留|

**说明：** 

-   当宿主机宕机或者磁盘损坏时，磁盘中的数据将会丢失。
-   请勿在本地磁盘上存储需要长期保存的业务数据，并及时做好数据备份和采用高可用架构。如需长期保存，建议将数据存储在云盘上。

为了能够在本地盘上部署 Kafka 服务，E-MapReduce 默认以下要求：

1.  default.replication.factor = 3，即要求 topic 的分区副本数至少为3。如果设置更小副本数，则会增加数据丢失风险。
2.  min.insync.replicas = 2，即要求当 producer 设定 acks 为 all\(-1\)时，每次至少写入两个副本才算写入成功。

当出现本地盘损坏时，E-MapReduce 会进行：

1.  将坏盘从 Broker 的配置中剔除，重启 Broker，在其他正常可用的本地盘上恢复坏盘丢失的数据。根据坏盘上已经写入的数据量不等，恢复的总时间也不等。
2.  当机器磁盘损坏数目过多（超过 20%）是，E-MapReduce 将主动进行机器迁移，恢复异常的磁盘。
3.  如果当前机器上可用剩余磁盘空间不足以恢复坏盘丢失数据时，Broker 将异常 Down 掉。这种情况，您可以选择清理一些数据，腾出磁盘空间并重启 Broker 服务；也可以联系 E-MapReduce 进行机器迁移，恢复异常的磁盘。

## 参数说明 {#section_fjz_vbx_y2b .section}

您可以在 E-MapReduce 的集群配置管理中查看 Kafka 的软件配置，当前主要有：

|配置项|说明|
|:--|:-|
|zookeeper.connect|Kafka 集群的 Zookeeper 连接地址|
|kafka.heap.opts|Kafka broker 的堆内存大小|
|num.io.threads|Kafka broker的 IO 线程数，默认为机器 CPU 核数目的 2 倍|
|num.network.threads|Kafka broker 的网络线程数，默认为机器的 CPU 核数目|

