# Use LogHub Source to move data from non-EMR clusters to HDFS of EMR clusters {#concept_227958 .concept}

This topic describes how to use EMR-Flume to move data in Log Service to HDFS of an EMR cluster and store the data in partitions based on record timestamps.

## Background {#section_lan_bd8_e9r .section}

EMR features EMR-Flume with Log Service Source since V3.20.0. By using tools of Log Service such as Logtail, you can move data to LogHub and use EMR-Flume to move the data to HDFS of an EMR cluster. For more information, see [Collection methods](../../../../reseller.en-US/User Guide/Data Collection/Collection methods.md#).

## Preparations {#section_x38_xa6_m39 .section}

Create a Hadoop cluster and select Flume from optional services. For more information, see [Create a cluster](../../../../reseller.en-US/Cluster Planning and Configurations/Configure clusters/Create a cluster.md#).

## Configure Flume {#section_06y_uth_kbg .section}

-   Configure Source

    |Configuration item|Value|Description|
    |------------------|-----|-----------|
    |type|org.apache.flume.source.loghub.LogHubSource| |
    |endpoint|The endpoint of LogHub|If you use a VPC or classic network endpoint, make sure that the VPC or classic network is deployed in the same region as the EMR cluster. If you use a public network endpoint, make sure that the node on which the Flume agent runs is assigned with a public IP address.|
    |project|The project of LogHub| |
    |logstore|The logstore of LogHub| |
    |accessKeyId|The AccessKey ID| |
    |accessKey|The AccessKey Secret| |
    |useRecordTime|true|Default value: false. If the timestamp property is not in the header, the time when events are received is encoded as timestamps, which are inserted into the header. When Flume Agent starts or stops or data synchronization is delayed, the data is placed in the wrong partitions. Set the value to true. A true value indicates using the time when LogHub collects the data as the timestamp.|
    |consumerGroup|consumer\_1|The name of the consumer group. Default value: consumer\_1.|

    The other configuration items are described as follows.

    -   consumerPosition 

        The position where the consumer group consumes the LogHub data for the first time. Default value: end \(indicates consuming the latest data\). Valid values: begin, special, and end. "begin" indicates that the consumer group starts consuming from the earliest data. "special" indicates that the consumer group starts data consuming at a specified offset. When the value is set to special, you need to specify the offset by using the startTime configuration item. Unit: seconds. The LogHub server records the consumer position of the consumer group after first data consumption. To modify the consumerPosition value, clear the status of the consumer group that consumes LogHub data. For more information, see [Status of a consumer group](../../../../reseller.en-US/User Guide/Real-time subscription and consumption/Consumption by consumer groups/View consumer group status.md#). You can also modify the value of consumerGroup to assign another consumer group.

    -   heartbeatInterval and fetchInOrder 

        "heartbeatInterval" indicates the interval at which the consumer group sends heartbeats to the server. Unit: milliseconds. Default value: 30000. "fetchInOrder" indicates whether the consumer group consumes data with the same key in sequence. Default value: false.

    -   batchSize and batchDurationMillis

        Common configuration items for source batch. Indicate the thresholds that trigger the events to be written to the channel.

    -   backoffSleepIncrement and maxBackoffSleep

        Common configuration items for source sleep. Indicate the increment for time delay and the maximum time delay before retrieving LogHub data again when no data is found in LogHub.

-   Configure the channel and sink

    In this example, the memory channel and the HDFS sink are used.

    -   Configure the HDFS sink as follows.

        |Configuration item|Value|
        |------------------|-----|
        |hdfs.path|/tmp/flume-data/loghub/datetime=%y%m%d/hour=%H|
        |hdfs.fileType|DataStream|
        |hdfs.rollInterval|3600|
        |hdfs.round|true|
        |hdfs.roundValue|60|
        |hdfs.roundUnit|minute|
        |hdfs.rollSize|0|
        |hdfs.rollCount|0|

    -   Configure the memory channel as follows.

        |Configuration item|Value|
        |------------------|-----|
        |capacity|2000|
        |transactionCapacity|2000|


## Run Flume Agent {#section_bh6_l36_4xh .section}

For more information, see [How to use Flume](reseller.en-US/Open Source Components /Flume/Use Flume.md#). After Flume Agent is started, on the configured HDFS path, you can see the logs that are stored in the partitions based on the record timestamps.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/190546/155929393946306_en-US.png)

For information about the status of consumer groups on Log Service, see [View consumer group status](../../../../reseller.en-US/User Guide/Real-time subscription and consumption/Consumption by consumer groups/View consumer group status.md#)

