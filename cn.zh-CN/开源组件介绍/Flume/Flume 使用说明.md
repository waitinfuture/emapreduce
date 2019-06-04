# Flume 使用说明 {#concept_186399 .concept}

本文以 E-MapReduce-Flume 实时同步 HDFS audit 日志至 HDFS 为例，介绍 Flume 的使用。

## 背景信息 {#section_dxg_nk6_5mw .section}

E-MapReduce 从 3.19.0 版本开始对 EMR-Flume 提供集群管理的功能。通过集群管理功能，可以在 Web 页面方便的配置和管理 Flume Agent。示例中，在 master 实例启动 Flume agent，收集本地磁盘中的 audit 日志通过 Avro 协议发送数据至 core 实例，在 core 实例配置并启动 failover sink processor，接收 master 实例发送的数据并 sink 到 HDFS 中。 Flume 其他使用场景的配置见 [Flume 配置说明](intl.zh-CN/开源组件介绍/Flume/Flume 配置说明.md#)。

## 准备工作 {#section_9eq_o48_6x5 .section}

创建 E-MapReduce Hadoop 集群，在可选服务中选择 Flume。具体操作请参见[创建集群](../../../../intl.zh-CN/集群规划与配置/集群配置/创建集群.md#)。

## 操作步骤 {#section_xb2_y7i_unv .section}

-   Core 实例配置并启动 Flume Agent

    例如在 emr-worker-1 节点进行操作，选择核心实例组进行配置，如下图所示：

    1.  在配置页面设置如下 ：

        |default-agent.sinks.default-sink.type|hdfs|
        |default-agent.channels.default-channel.type|file|
        |default-agent.sources.default-source.type|avro|
        |deploy\_node\_hostname|emr-worker-1|

    2.  在配置页面通过自定义配置添加如下配置：

        |default-agent.sinks.default-sink.hdfs.path|对于高可用集群，使用 hdfs://emr-cluster/path 形式的地址|
        |default-agent.sinks.default-sink.hdfs.fileType|DataStream|
        |default-agent.sinks.default-sink.hdfs.rollSize|0|
        |default-agent.sinks.default-sink.hdfs.rollCount|0|
        |default-agent.sinks.default-sink.hdfs.rollInterval|86400|
        |default-agent.sinks.default-sink.hdfs.batchSize|51200|
        |default-agent.sources.default-source.bind|0.0.0.0|
        |default-agent.sources.default-source.port|根据实际设置|
        |default-agent.channels.default-channel.transactionCapacity|51200|
        |default-agent.channels.default-channel.dataDirs|channel 存储 event 数据的路径|
        |default-agent.channels.default-channel.checkpointDir|存储 checkpoint 的路径|
        |default-agent.channels.default-channel.capacity|根据 hdfs roll 进行设置|

    3.  保存配置后启动 Flume agent
    4.  单击**查看操作历史**，显示操作成功后，部署拓扑页面可以看到 emr-worker-1 节点的 flume 已经是 started 状态。emr-worker-1 节点启动成功后，开始启动第二个 worker 节点。 同样的方式，例如在 worker-2 节点启动 flume，修改配置项：

        |deploy\_node\_hostname|节点的 hostname|
        |default-agent.sinks.default-sink.hdfs.path|对于高可用集群，使用 hdfs://emr-cluster/path 形式的地址|

    5.  保存配置后，启动 All Components，指定机器为 emr-worker-2。
-   Master 实例配置并启动 Flume Agent

    例如在 emr-header-1 节点进行操作，选择服务配置。

    配置 agent 如下：

    |additional\_sinks|k1|
    |deploy\_node\_hostname|emr-header-1|
    |default-agent.sources.default-source.type|taildir|
    |default-agent.sinks.default-sink.type|avro|
    |default-agent.channels.default-channel.type|file|

    新增配置如下：

    |配置项|值|
    |---|--|
    |default-agent.sources.default-source.filegroups|f1|
    |default-agent.sources.default-source.filegroups.f1|/mnt/disk1/log/hadoop-hdfs/hdfs-audit.log.\*|
    |default-agent.sources.default-source.positionFile|存储 position file 的路径|
    |default-agent.channels.default-channel.checkpointDir|存储 checkpoint 的路径|
    |default-agent.channels.default-channel.dataDirs|存储 event 数据的路径|
    |default-agent.channels.default-channel.capacity|根据实际情况设置|
    |default-agent.sources.default-source.batchSize|2000|
    |default-agent.channels.default-channel.transactionCapacity|2000|
    |default-agent.sources.default-source.ignoreRenameWhenMultiMatching|true|
    |default-agent.sinkgroups|g1|
    |default-agent.sinkgroups.g1.sinks|default-sink k1|
    |default-agent.sinkgroups.g1.processor.type|failover|
    |default-agent.sinkgroups.g1.processor.priority.default-sink|10|
    |default-agent.sinkgroups.g1.processor.priority.k1|5|
    |default-agent.sinks.default-sink.hostname|emr-worker-1 节点的IP|
    |default-agent.sinks.default-sink.port|emr-worker-1 节点 Flume Agent 的 port|
    |default-agent.sinks.k1.hostname|emr-worker-2 节点的 IP|
    |default-agent.sinks.k1.port|emr-worker-2 节点 Flume Agent 的 port|
    |default-agent.sinks.default-sink.batch-size|2000|
    |default-agent.sinks.k1.batch-size|2000|
    |default-agent.sinks.k1.type|avro|
    |default-agent.sinks.k1.channel|default-channel|


## 查看同步结果 {#section_7ak_5sw_re7 .section}

使用 HDFS 命令，可以看到同步的数据被写入 FlumeData.$\{timestamp\} 形式的文件中，其中 timestamp 为文件创建的时间戳。

## 查看日志 {#section_0co_aop_gzu .section}

Flume agent 日志的存放路径为 /mnt/disk1/log/flume/default-agent/flume.log。

## 查看监控信息 {#section_tmt_fwe_84j .section}

集群与服务管理页面提供了 Flume agent 的监控信息。通过在集群与服务管理页面单击 **Flume** 服务进行访问，如下图所示：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/160292/155963146547819_zh-CN.png)

