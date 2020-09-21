# 使用LogHub Source将非E-MapReduce集群的数据同步至E-MapReduce集群的HDFS

本文介绍如何使用E-MapReduce的Flume实时同步Log Service的数据至E-MapReduce集群的HDFS，并根据数据记录的时间戳将数据存入HDFS相应的分区中。

## 背景信息

从EMR-3.20.0版本开始将E-MapReduce的Flume加入了Log Service Source。您可以借助Log Service的Logtail工具，将需要同步的数据实时采集并上传到LogHub，再使用E-MapReduce的Flume将LogHub的数据同步至EMR集群的HDFS。

采集数据到Log Service的LogHub的详细步骤参见[采集方式](/intl.zh-CN/数据采集/采集方式.md)。

## 前提条件

创建Hadoop集群，在可选服务中选择Flume，详细步骤请参见[创建集群](/intl.zh-CN/集群管理/集群配置/创建集群.md)。

## 配置Flume

-   配置Source

    |参数|说明|
    |--|--|
    |type|设置为org.apache.flume.source.loghub.LogHubSource。|
    |endpoint|LogHub的Endpoint。 **说明：** 如果使用VPC或经典网络的Endpoint，需要保证与EMR集群在同一个地区；如果使用公网Endpoint，需要保证运行Flume agent的节点有公网IP。 |
    |project|LogHub的项目名。|
    |logstore|LogStore名称。|
    |accessKeyId|阿里云的AccessKey ID。|
    |accessKey|阿里云的AccessKey Secret。|
    |useRecordTime|设置为true。 默认值为alse。如果Header中没有Timestamp属性，接收Event的时间戳会被加入到Header中。 但是在Flume Agent启停或者同步滞后等情况下，会将数据放入错误的时间分区中。为避免这种情况，可以将该值设置为true，使用数据收集到LogHub的时间作为Timestamp。 |
    |consumerGroup|消费组名称，默认值为consumer\_1。|

    其他参数说明如下：

    |参数|说明|
    |--|--|
    |consumerPosition|消费组在第一次消费LogHub数据时的位置，默认值为end，即从最近的数据开始消费。     -   begin：表示从最早的数据开始消费。
    -   special：表示从指定的时间点开始消费。

在配置为special时，需要配置startTime为开始消费的时间点，单位为秒。

 首次运行后LogHub服务端会记录消费组的消费点，此时如果想更改 consumerPosition，可以清除LogHub的消费组状态，或者更改配置consumerGroup为新的消费组。|
    |heartbeatInterval|消费组与服务端维持心跳的间隔，单位是毫秒，默认为30000毫秒。|
    |fetchInOrder|相同Key的数据是否按序消费，默认值为false。|
    |batchSize|通用的source batch配置，在一个批处理中写入通道的最大消息数。|
    |batchDurationMillis|通用的source batch配置，在将批处理写入通道之前的最大时间。|
    |backoffSleepIncrement|通用的source sleep配置，表示LogHub没有数据时触发Sleep的初始和增量等待时间。|
    |maxBackoffSleep|通用的source sleep配置，表示LogHub没有数据时触发Sleep的最大等待时间。|

-   配置Channel和Sink

    此处使用Memory Channel和HDFS Sink。

    -   HDFS Sink配置如下。

        |参数|值|
        |--|--|
        |hdfs.path|/tmp/flume-data/loghub/datetime=%y%m%d/hour=%H|
        |hdfs.fileType|DataStream|
        |hdfs.rollInterval|3600|
        |hdfs.round|true|
        |hdfs.roundValue|60|
        |hdfs.roundUnit|minute|
        |hdfs.rollSize|0|
        |hdfs.rollCount|0|

    -   Memory Channel配置如下。

        |参数|值|
        |--|--|
        |capacity|2000|
        |transactionCapacity|2000|


## 运行Flume agent

在阿里云E-Mapreduce控制台页面启动Flume agent的具体操作参见 [使用说明](/intl.zh-CN/集群类型/Hadoop集群/Flume/Flume 使用说明.md)。成功启动后，可以看到配置的HDFS路径下按照Record Timestamp存储的日志数据。

![Flume agent_run](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/6032659951/p46306.png)

