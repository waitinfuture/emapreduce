# Consume Log Service data {#concept_tnp_s2h_hfb .concept}

This topic describes how to use Spark Streaming to consume the log data of Log Service and count the log entries.

## Allow Spark to access Log Service {#section_x5b_z2h_hfb .section}

The following example shows how to use Spark Streaming to consume the log data of Log Service and count the log entries:

-   Method 1: Receiver-based DStream

    ``` {#codeblock_0z8_ssj_d08}
    val logServiceProject = args(0)    // The project name in Log Service.
        val logStoreName = args(1)     // The Logstore name in Log Service.
        val loghubConsumerGroupName = args(2) // The LogHub consumer group name in Log Service. Jobs that have the same LogHub consumer group name consume the data in a Logstore together.
        val loghubEndpoint = args(3)  // The API endpoint of Alibaba Cloud Log Service.
        val accessKeyId = "<accessKeyId>"     // The AccessKey ID used to access Log Service.
        val accessKeySecret = "<accessKeySecret>" // The AccessKey secret used to access Log Service.
        val numReceivers = args(4).toInt // The number of receivers started to read data from the Logstore.
        val batchInterval = Milliseconds(args(5).toInt * 1000) // The interval between Spark Streaming jobs.
        val conf = new SparkConf().setAppName("Test Loghub Streaming")
        val ssc = new StreamingContext(conf, batchInterval)
        val loghubStream = LoghubUtils.createStream(
          ssc,
          logServiceProject,
          logStoreName,
          loghubConsumerGroupName,
          loghubEndpoint,
          numReceivers,
          accessKeyId,
          accessKeySecret,
          StorageLevel.MEMORY_AND_DISK)
        loghubStream.foreachRDD(rdd => println(rdd.count()))
        ssc.start()
        ssc.awaitTermination()
    ```

-   Method 2: Direct API-based DStream

    ``` {#codeblock_nux_9sy_noy}
    val logServiceProject = args(0)
        val logStoreName = args(1)
        val loghubConsumerGroupName = args(2)
        val loghubEndpoint = args(3)
        val accessKeyId = args(4)
        val accessKeySecret = args(5)
        val batchInterval = Milliseconds(args(6).toInt * 1000)
        val zkConnect = args(7)
        val checkpointPath = args(8)
        def functionToCreateContext(): StreamingContext = {
          val conf = new SparkConf().setAppName("Test Direct Loghub Streaming")
          val ssc = new StreamingContext(conf, batchInterval)
          val zkParas = Map("zookeeper.connect" -> zkConnect, "enable.auto.commit" -> "false")
          val loghubStream = LoghubUtils.createDirectStream(
            ssc,
            logServiceProject,
            logStoreName,
            loghubConsumerGroupName,
            accessKeyId,
            accessKeySecret,
            loghubEndpoint,
            zkParas,
            LogHubCursorPosition.END_CURSOR)
          ssc.checkpoint(checkpointPath)
          val stream = loghubStream.checkpoint(batchInterval)
          stream.foreachRDD(rdd => {
            println(rdd.count())
            loghubStream.asInstanceOf[CanCommitOffsets].commitAsync()
          })
          ssc
        }
        val ssc = StreamingContext.getOrCreate(checkpointPath, functionToCreateContext _)
        ssc.start()
        ssc.awaitTermination()
    ```

    E-MapReduce \(EMR\) SDK V1.4.0 or later supports the Direct API method. It avoids writing duplicate LogHub data to the write-ahead log \(WAL\). You can make sure that the data is written at least once without enabling the WAL feature of Spark Streaming. Currently, the Direct API method is experimental. Note that:

    -   For DStream actions, a commit action is required.
    -   In a Spark Streaming job, you can perform only one action on the data source of the Logstore.
    -   The Direct API method requires the support of the ZooKeeper service.

## Use MetaService {#section_ovl_2fh_hfb .section}

In the preceding example, the AccessKey is directly specified. In EMR SDK V1.3.2 or later, Spark Streaming can process Log Service data by using MetaService without an AccessKey. For more information, see the description of the LoghubUtils class in the EMR SDK.

``` {#codeblock_sg9_o7x_ist}
LoghubUtils.createStream(ssc, logServiceProject, logStoreName, loghubConsumerGroupName, storageLevel)
LoghubUtils.createStream(ssc, logServiceProject, logStoreName, loghubConsumerGroupName, numReceivers, storageLevel)
LoghubUtils.createStream(ssc, logServiceProject, logStoreName, loghubConsumerGroupName, storageLevel, cursorPosition, mLoghubCursorStartTime, forceSpecial)
LoghubUtils.createStream(ssc, logServiceProject, logStoreName, loghubConsumerGroupName, numReceivers, storageLevel, cursorPosition, mLoghubCursorStartTime, forceSpecial)
```

**Note:** 

-   The EMR SDK supports the following three consumption modes for Log Service data: BEGIN\_CURSOR, END\_CURSOR, and SPECIAL\_TIMER\_CURSOR. The default mode is END\_CURSOR.
    -   BEGIN\_CURSOR: consumes data from the header of the log. If a checkpoint record exists, the consumption starts from the checkpoint.
    -   END\_CURSOR: consumes data from the end of the log. If a checkpoint record exists, the consumption starts from the checkpoint.
    -   SPECIAL\_TIMER\_CURSOR: consumes data from a specified time. Unit: seconds. If a checkpoint record exists, the consumption starts from the checkpoint.
    -   The three consumption modes are affected by checkpoint records. If a checkpoint record exists, the consumption starts from the checkpoint. The EMR SDK is based on the SPECIAL\_TIMER\_CURSOR mode. It allows you to start data consumption from a specified time. To use the createStream method of the LoghubUtils class, you need to set the following parameters:
        -   cursorPosition: LogHubCursorPosition.SPECIAL\_TIMER\_CURSOR
        -   forceSpecial: true
-   Except for the EMR server on the primary node, other servers cannot connect to the public network. When specifying the Log Service endpoint, make sure that you use an internal network endpoint provided by Log Service. Otherwise, you may fail to request data from Log Service.
-   For more information about Log Service, see [Log Service](https://www.alibabacloud.com/help/product/28958.htm).

## Appendix {#section_elt_kfh_hfb .section}

For more information, see the complete sample code on GitHub:

-   [Allow Spark to access Log Service](https://github.com/aliyun/aliyun-emapreduce-demo/blob/master/src/main/scala/com/aliyun/emr/example/LoghubSample.scala)

