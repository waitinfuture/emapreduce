# Use Flume {#concept_186399 .concept}

This topic takes E-MapReduce-Flume moving audit log to HDFS as an example to describe how to use Flume.

## Background {#section_dxg_nk6_5mw .section}

E-MapReduce supports cluster management for EMR-Flume since V3.19.0. By using cluster management, you can configure and manage Flume Agent on the web pages. In the example, Flume Agent is started on the master instance to collect audit log stored in the local disk and sends the log to core instances by using the Avro protocol. Failover Sink Processor is configured and started on the core instances to receive the data from the master instance. Then the data is moved to to HDFS by using sinks. For more scenarios of using Flume, see [How to use Flume](reseller.en-US/Open Source Components /Flume/Configure Flume.md#).

## Preparations {#section_9eq_o48_6x5 .section}

Create an EMR cluster and select Flume from the optional services. For more information, see [Create a cluster](../../../../reseller.en-US/Cluster Planning and Configurations/Configure clusters/Create a cluster.md#).

## Procedure {#section_xb2_y7i_unv .section}

-   Configure and start Flume Agent on core instances

    For example, configure Flume Agent for the core instance group on the emr-worker-1 node as shown in the following figure.

    1.  In the Service Configuration section, set the configurations as follows.

        |default-agent.sinks.default-sink.type|hdfs|
        |default-agent.channels.default-channel.type|file|
        |default-agent.sources.default-source.type|avro|
        |deploy\_node\_hostname|emr-worker-1|

    2.  Click Custom Configuration and add the following configurations.

        |default-agent.sinks.default-sink.hdfs.path|For high-availability clusters, the hdfs://emr-cluster/path format is used for the address.|
        |default-agent.sinks.default-sink.hdfs.fileType|DataStream|
        |default-agent.sinks.default-sink.hdfs.rollSize|0|
        |default-agent.sinks.default-sink.hdfs.rollCount|0|
        |default-agent.sinks.default-sink.hdfs.rollInterval|86400|
        |default-agent.sinks.default-sink.hdfs.batchSize|51200|
        |default-agent.sources.default-source.bind|0.0.0.0|
        |default-agent.sources.default-source.port|Set a value as required.|
        |default-agent.channels.default-channel.transactionCapacity|51200|
        |default-agent.channels.default-channel.dataDirs|The path on which channels store the event data.|
        |Default-agent.channels.default-channel.checkpointDir|The path on which the checkpoint file is stored.|
        |default-agent.channels.default-channel.capacity|Set a value based on the hdfs roll.|

    3.  Save the configuration and start Flume Agent.
    4.  Click **History**. After Successful appears in the Status column, STARTED is displayed for Flume Agent on the emr-worker-1 node in the Status column in the Component Deployment tab page.After Flume Agent on the emr-worker-1 node is started, start Flume Agent on other worker nodes. For example, to start Flume Agent on the emr-worker-2 node, modify the following configuration items.

        |deploy\_node\_hostname|The hostname of the node.|
        |default-agent.sinks.default-sink.hdfs.path|For high-availability clusters, the hdfs://emr-cluster/path format is used for the address.|

    5.  Save the configuration, start all components, and select emr-worker-2 as the target node.
-   Configure and start Flume Agent on the master instance

    For example,

    Configure Agent as follows.

    |additional\_sinks|k1|
    |deploy\_node\_hostname|emr-header-1|
    |default-agent.sources.default-source.type|taildir|
    |default-agent.sinks.default-sink.type|avro|
    |default-agent.channels.default-channel.type|file|

    The new configurations are as follows.

    |Configuration item|Value|
    |------------------|-----|
    |default-agent.sources.default-source.filegroups|f1|
    |default-agent.sources.default-source.filegroups.f1|/mnt/disk1/log/hadoop-hdfs/hdfs-audit.log. \*|
    |default-agent.sources.default-source.positionFile|The path on which the position file is stored.|
    |default-agent.channels.default-channel.checkpointDir|The path on which the checkpoint file is stored.|
    |default-agent.channels.default-channel.dataDirs|The path on which channels store the event data.|
    |default-agent.channels.default-channel.capacity|Set a value as required.|
    |default-agent.sources.default-source.batchSize|2000|
    |default-agent.channels.default-channel.transactionCapacity|2000|
    |default-agent.sources.default-source.ignoreRenameWhenMultiMatching|true|
    |default-agent.sinkgroups|g1|
    |default-agent.sinkgroups.g1.sinks|default-sink k1|
    |default-agent.sinkgroups.g1.processor.type|failover|
    |default-agent.sinkgroups.g1.processor.priority.default-sink|10|
    |default-agent.sinkgroups.g1.processor.priority.k1|5|
    |default-agent.sinks.default-sink.hostname|The IP address of the emr-worker-1 node.|
    |default-agent.sinks.default-sink.port|The port of Flume Agent on the emr-worker-1 node.|
    |default-agent.sinks.k1.hostname|The IP address of the emr-worker- node.|
    |default-agent.sinks.k1.port|The port of Flume Agent on the emr-worker-2 node.|
    |default-agent.sinks.default-sink.batch-size|2000|
    |default-agent.sinks.k1.batch-size|2000|
    |default-agent.sinks.k1.type|avro|
    |default-agent.sinks.k1.channel|default-channel|


## View the result of synchronization {#section_7ak_5sw_re7 .section}

By using the HDFS command, you can see that the data is written to files named FlumeData.$\{timestamp\}. "timestamp" shows when the file was created.

