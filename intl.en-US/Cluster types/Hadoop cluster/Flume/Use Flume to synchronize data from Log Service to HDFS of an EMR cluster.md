# Use Flume to synchronize data from Log Service to HDFS of an EMR cluster

This topic describes how to use the Flume service of an EMR cluster to synchronize data from Log Service to HDFS of the EMR cluster. The synchronized data is automatically saved in HDFS partitions based on timestamps.

## Background information

In EMR V3.20.0 and later, Log Service can be configured as a source of the Flume service of EMR clusters. You can use Logtail of Log Service to collect the data to be synchronized and upload the data to LogHub. Then, use the Flume service of an EMR cluster to synchronize the data from LogHub to HDFS of the EMR cluster.

For more information about how to upload data to LogHub, see [Log collection methods](/intl.en-US/Data Collection/Log collection methods.md).

## Prerequisites

An EMR Hadoop cluster is created, and Flume is selected from the optional services during the cluster creation. For more information, see [Create a cluster](/intl.en-US/Cluster Management/Configure clusters/Create a cluster.md).

## Configure Flume

-   Configure a source

    |Parameter|Description|
    |---------|-----------|
    |type|Set this parameter to org.apache.flume.source.loghub.LogHubSource.|
    |endpoint|The endpoint used to access LogHub. **Note:** If you use the endpoint of a VPC or the classic network, make sure that the VPC or classic network is deployed in the same region as the EMR cluster. If you use a public endpoint, make sure that the node on which a Flume agent runs is assigned with a public IP address. |
    |project|The name of the LogHub project.|
    |logstore|The name of the Logstore.|
    |accessKeyId|The AccessKey ID of your Alibaba Cloud account.|
    |accessKey|The AccessKey secret of your Alibaba Cloud account.|
    |useRecordTime|Set this parameter to true. Default value: false. If a header does not contain the timestamp property, the time when events are received is encoded as timestamps, and the timestamps are inserted into the header. When the Flume agent starts or stops or data synchronization is delayed, the data is placed in wrong partitions. To avoid this issue, set the value to true. In this way, the time when LogHub collects the data is used as the timestamp. |
    |consumerGroup|The name of the consumer group. Default value: consumer\_1.|

    The following table describes the other parameters.

    |Parameter|Description|
    |---------|-----------|
    |consumerPosition|The position where the consumer group consumes the LogHub data for the first time. Default value: end. The value end indicates that the consumption starts from the latest data.     -   begin: The consumption starts from the earliest data.
    -   special: The consumption starts from a specified point in time.

If you set this parameter to special, you must set startTime to a specific point in time. Unit: seconds.

 The LogHub server records the consumption position of the consumer group after the first data consumption. To change the consumerPosition value after the first data consumption, you can clear the status information of the consumer group or configure a new consumer group by changing the value of consumerGroup.|
    |heartbeatInterval|The interval at which the consumer group sends heartbeats to the server. Unit: milliseconds. Default value: 30000.|
    |fetchInOrder|Specifies whether the consumer group consumes data with the same key in sequence. Default value: false.|
    |batchSize|The maximum number of messages that can be written to a channel at a time. This is a common source batch configuration.|
    |batchDurationMillis|The maximum number of milliseconds to wait before messages are written to a channel at a time. This is a common source batch configuration.|
    |backoffSleepIncrement|The initial and incremental wait time that triggers sleep when LogHub does not have data. This is a common source sleep configuration.|
    |maxBackoffSleep|The maximum wait time that triggers sleep when LogHub does not have data. This is a common source sleep configuration.|

-   Configure a channel and a sink

    In this example, a memory channel and an HDFS sink are used.

    -   Configure an HDFS sink. The following table lists the parameters to be configured.

        |Parameter|Value|
        |---------|-----|
        |hdfs.path|/tmp/flume-data/loghub/datetime=%y%m%d/hour=%H|
        |hdfs.fileType|DataStream|
        |hdfs.rollInterval|3600|
        |hdfs.round|true|
        |hdfs.roundValue|60|
        |hdfs.roundUnit|minute|
        |hdfs.rollSize|0|
        |hdfs.rollCount|0|

    -   Configure a memory channel. The following table lists the parameters to be configured.

        |Parameter|Value|
        |---------|-----|
        |capacity|2000|
        |transactionCapacity|2000|


## Start the Flume agent

Start the Flume agent in the EMR console. For more information, see [Use Flume](/intl.en-US/Cluster types/Hadoop cluster/Flume/Use Flume.md). After the Flume agent is started, you can view the logs in the configured HDFS path based on timestamps.

![Flume agent_run](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4948159751/p46306.png)

