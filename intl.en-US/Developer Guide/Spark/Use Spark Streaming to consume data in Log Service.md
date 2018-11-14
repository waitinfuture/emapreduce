# Use Spark Streaming to consume data in Log Service {#concept_tnp_s2h_hfb .concept}

This topic describes how to use Spark Streaming to consume the log data of Log Service and count the log entries.

## Allow Spark to access Log Service {#section_x5b_z2h_hfb .section}

The following example shows how Spark Streaming consumes the log data of Log Service and counts log entries.

-   Method 1: Receiver-based DStream

    ```
    val logServiceProject = args(0)    // project name in Log Service
        val logStoreName = args(1)     // logstore name in Log Service
        val loghubConsumerGroupName = args(2)  // loghubGroupName (The same jobs consume the Logstore data together)
        val loghubEndpoint = args(3)  // API Endpoint (Alibaba Cloud Log Service)
        val accessKeyId = "<accessKeyId>"     // AccessKey ID for accessing Log Service
        val accessKeySecret = "<accessKeySecret>" // AccessKey Secret for accessing Log Service
        val numReceivers = args(4).toInt  // The number of receivers started to read data in Logstore
        val batchInterval = Milliseconds(args(5).toInt * 1000) // The interval of the Spark Streaming proccessing
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

    ```
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

    The E-MapReduce SDK with a version of 1.4.0 or higher supports Direct API. It avoids writing duplicate Loghub data to the Write Ahead Log, which means you can make sure that the data is at least once without enabling the WAL feature of Spark Streaming. The Direct API is experimental. Note that:

    -   For DStream actions, a commit action is required.
    -   For Spark Streaming, multiple actions on Logstore data sources are not supported.
    -   Direct API requires the support of the Zookeeper service.

## Enable MetaService {#section_ovl_2fh_hfb .section}

In the preceding example, we explicitly pass the AccessKey to the API. With E-MapReduce SDK 1.3.2 or a later version, Spark Streaming can process Log Service data using MetaService without an AccessKey. For more information, see the description of the LoghubUtils class in E-MapReduce SDK:

```
LoghubUtils.createStream(ssc, logServiceProject, logStoreName, loghubConsumerGroupName, storageLevel)
LoghubUtils.createStream(ssc, logServiceProject, logStoreName, loghubConsumerGroupName, numReceivers, storageLevel)
LoghubUtils.createStream(ssc, logServiceProject, logStoreName, loghubConsumerGroupName, storageLevel, cursorPosition, mLoghubCursorStartTime, forceSpecial)
LoghubUtils.createStream(ssc, logServiceProject, logStoreName, loghubConsumerGroupName, numReceivers, storageLevel, cursorPosition, mLoghubCursorStartTime, forceSpecial)
```

**Note:** 

-   E-MapReduce SDK supports three consumption modes for Log Service, which are BEGIN\_CURSOR, END\_CURSOR, and SPECIAL\_TIMER\_CURSOR. By default, the mode is END\_CURSOR.
    -   BEGIN\_CURSOR: Consumes from the log header. If a checkpoint record exists, the consumption starts from the checkpoint.
    -   END\_CURSOR: Consumes from the end of the log. If a checkpoint record exists, the consumption starts from the checkpoint.
    -   SPECIAL\_TIMER\_CURSOR: Consumes from a specified time. If a checkpoint record exists, the consumption starts from the checkpoint. Unit: second.
    -   The three consumption modes are affected by checkpoint records. If a checkpoint record exists, the consumption always starts from the checkpoint. E-MapReduce SDK is based on the SPECIAL\_TIMER\_CURSOR mode. It allows you to start consumption from a specified time. In the LoghubUtils\#createStream interface, the following parameters must be used in combination:
        -   cursorPosition: LogHubCursorPosition.SPECIAL\_TIMER\_CURSOR
        -   forceSpecial: true
-   E-MapReduce machines cannot connect to the public network \(except the master node\). When configuring the Log Service endpoint, make sure that you use the internal endpoints provided by Log Service. Otherwise, requests to the Log Service fail.
-   For more information about Log Service, see [Log Service](https://www.alibabacloud.com/help/zh/product/28958.htm).

## Appendix {#section_elt_kfh_hfb .section}

For the complete sample code, see:

-   [Allow Spark to access Log Service](https://github.com/aliyun/aliyun-emapreduce-demo/blob/master/src/main/scala/com/aliyun/emr/example/LoghubSample.scala)

