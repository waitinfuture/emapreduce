# Kafka 快速入门 {#concept_ot1_5bx_y2b .concept}

从EMR-3.4.0版本开始将支持Kafka服务。

## 创建Kafka集群 {#section_w3z_vbx_y2b .section}

在E-MapReduce控制台创建集群时，选择集群类型为Kafka，则会创建一个默认只包含Kafka组件的集群，除了基础组件外包括Zookeeper，Kafka和KafkaManager三个组件。每个节点将只部署一个Kafka broker。我们建议您的Kafka集群是一个专用集群，不要和Hadoop相关服务混部在一起。

## 本地盘Kafka集群 {#section_x3z_vbx_y2b .section}

为了更好地降低单位成本以及应对更大的存储需求，E-MapReduce将在EMR-3.5.1版本开始支持基于本地盘（D1类簇机型，详情参考[ECS机型](https://help.aliyun.com/document_detail/25378.html)介绍文档）的Kafka集群。相比较云盘，本地盘Kafka集群有如下特点：

-   实例配备大容量、高吞吐SATA HDD本地盘，单磁盘 190 MB/s 顺序读写性能，单实例最大 5 GB/s 存储吞吐能力。
-   本地存储价格比 SSD 云盘降低 97%。
-   更高网络性能，最大17 Gbit/s实例间网络带宽，满足业务高峰期实例间数据交互需求。

但本地盘机型也有特殊之处：

|操作|本地磁盘数据状态|说明|
|:-|:-------|:-|
|操作系统重启/控制台重启/强制重启|保留|本地磁盘存储卷保留，数据保留|
|操作系统关机/控制台停止/强制停止|保留|本地磁盘存储卷保留，数据保留|
|控制台上释放（实例）|擦除|本地磁盘存储卷擦除，数据不保留|

**说明：** 

-   当宿主机宕机或者磁盘损坏时，磁盘中的数据将会丢失。
-   请勿在本地磁盘上存储需要长期保存的业务数据，并及时做好数据备份和采用高可用架构。如需长期保存，建议将数据存储在云盘上。

为了能够在本地盘上部署Kafka服务，E-MapReduce默认要求：

1.  default.replication.factor = 3，即要求topic的分区副本数至少为3。如果设置更小副本数，则会增加数据丢失风险。
2.  min.insync.replicas = 2，即要求当producer设定acks为all\(-1\)时，每次至少写入两个副本才算写入成功。

当出现本地盘损坏时，E-MapReduce会进行：

1.  将坏盘从Broker的配置中剔除，重启Broker，在其他正常可用的本地盘上恢复坏盘丢失的数据。根据坏盘上已经写入的数据量不等，恢复的总时间也不等。
2.  当机器磁盘损坏数目过多（超过20%）是，E-MapReduce将主动进行机器迁移，恢复异常的磁盘。
3.  如果当前机器上可用剩余磁盘空间不足以恢复坏盘丢失数据时，Broker将异常Down掉。这种情况，您可以选择清理一些数据，腾出磁盘空间并重启Broker服务；也可以联系E-MapReduce进行机器迁移，恢复异常的磁盘。

## 参数说明 {#section_fjz_vbx_y2b .section}

您可以在E-MapReduce的集群配置管理中查看Kafka的软件配置，当前主要有：

|配置项|说明|
|:--|:-|
|zookeeper.connect|Kafka集群的Zookeeper连接地址|
|kafka.heap.opts|Kafka broker的堆内存大小|
|num.io.threads|Kafka broker的IO线程数，默认为机器CPU核数目的2倍|
|num.network.threads|Kafka broker的网络线程数，默认为机器的CPU核数目|

