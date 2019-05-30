# 使用 LogHub Source 将非 E-MapReduce 集群的数据同步至 E-MapReduce 集群的 HDFS {#concept_227958 .concept}

本文介绍使用 EMR-Flume 实时同步 Log Service 的数据至 EMR 集群的 HDFS，并根据 record timestamp 将数据存入 HDFS 相应的 partition 中。

## 背景信息 {#section_lan_bd8_e9r .section}

E-MapReduce 从 3.20.0 版本开始对 EMR-Flume 加入了 Log Service Source。借助 Log Service 的 Logtail 等工具，可以将需要同步的数据实时采集并上传到 LogHub，再使用 EMR-Flume 将 LogHub 的数据同步至 EMR 集群的 HDFS。 有关采集数据到 Log Service 的 LogHub 的详细方法及步骤参见[采集方式](../../../../intl.zh-CN/用户指南/数据采集/采集方式.md#)。

## 准备工作 {#section_x38_xa6_m39 .section}

创建 Hadoop 集群，在可选软件中选择 Flume，详细步骤请参见[创建集群](../../../../intl.zh-CN/集群规划与配置/集群配置/创建集群.md#)。

## 配置 Flume {#section_06y_uth_kbg .section}

-   配置 Source

    |配置项|值|说明|
    |---|--|--|
    |type|org.apache.flume.source.loghub.LogHubSource| |
    |endpoint|LogHub 的 Endpoint|如果使用 VPC/经典网络的 endpoint，要保证与 EMR 集群在同一个地区；如果使用公网 endpoint，要保证运行 Flume agent 的节点有公网 IP。|
    |project|LogHub 的 project| |
    |logstore|LogHub 的 logstore| |
    |accessKeyId|阿里云的 AccessKey ID| |
    |accessKey|阿里云的 AccessKey| |
    |useRecordTime|true|默认值为 false。如果 header 中没有 timestamp 属性，接收 event 的时间戳会被加入到 header 中； 但是在Flume Agent 启停或者同步滞后等情况下，会将数据放入错误的时间分区中。为避免这种情况，可以将该值设置为true，使用数据收集到 LogHub 的时间作为 timestamp。|
    |consumerGroup|consumer\_1|消费组名称，默认值为consumer\_1|

    其他配置项说明如下：

    -   consumerPosition 

        消费组在第一次消费 LogHub 数据时的位置，默认值为 end，即从最近的数据开始消费； 可以设置的其他值为 begin 或 special。begin 表示从最早的数据开始消费；special 表示从指定的时间点开始消费。在配置为 special 时，需要配置 startTime 为开始消费的时间点，单位为 秒。 首次运行后 LogHub 服务端会记录消费组的消费点，此时如果想更改 consumerPosition，可以清除 LogHub 的消费组状态，参见[消费组状态](../../../../intl.zh-CN/用户指南/实时消费/消费组消费/消费组状态.md#)；或者更改配置consumerGroup 为新的消费组。

    -   heartbeatInterval 和 fetchInOrder 

        heartbeatInterval 表示消费组与服务端维持心跳的间隔，单位是毫秒，默认为 30 秒；fetchInOrder 表示相同 key 的数据是否按序消费，默认值为 false。

    -   batchSize 和 batchDurationMillis

        通用的 source batch 配置，表示触发 event 写入 channel 的阈值。

    -   backoffSleepIncrement 和 maxBackoffSleep

        通用的 source sleep 配置，表示 LogHub 没有数据时触发 sleep 的时间和增量。

-   配置 channel 和 sink

    此处使用 memory channel 和 HDFS sink。

    -   HDFS Sink 配置如下：

        |配置项|值|
        |---|--|
        |hdfs.path|/tmp/flume-data/loghub/datetime=%y%m%d/hour=%H|
        |hdfs.fileType|DataStream|
        |hdfs.rollInterval|3600|
        |hdfs.round|true|
        |hdfs.roundValue|60|
        |hdfs.roundUnit|minute|
        |hdfs.rollSize|0|
        |hdfs.rollCount|0|

    -   Memory channel 配置如下：

        |配置项|值|
        |---|--|
        |capacity|2000|
        |transactionCapacity|2000|


## 运行 Flume agent {#section_bh6_l36_4xh .section}

在 Console 页面启动 Flume agent 的具体操作参见 [Flume 使用说明](intl.zh-CN/开源组件介绍/Flume/Flume 使用说明.md#)。成功启动后，可以看到配置的 HDFS 路径下按照 record timestamp 存储的日志数据。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/190546/155918490746306_zh-CN.png)

查看 Log Service 上的消费组状态：

