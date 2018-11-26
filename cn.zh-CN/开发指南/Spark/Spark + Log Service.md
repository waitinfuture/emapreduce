# Spark + Log Service {#concept_tnp_s2h_hfb .concept}

本章节将介绍Spark Streaming如何消费Log Service中的日志数据，统计日志条数。

## Spark 接入 Log Service {#section_x5b_z2h_hfb .section}

下面这个例子演示了Spark Streaming如何消费Log Service中的日志数据，统计日志条数。

-   方法一：Receiver Based DStream

    ```
    val logServiceProject = args(0)    // LogService 中 project 名
        val logStoreName = args(1)     // LogService 中 logstore 名
        val loghubConsumerGroupName = args(2)  // loghubGroupName 相同的作业将共同消费 logstore 的数据
        val loghubEndpoint = args(3)  // 阿里云日志服务数据类 API Endpoint
        val accessKeyId = "<accessKeyId>"     // 访问日志服务的 AccessKeyId
        val accessKeySecret = "<accessKeySecret>" // 访问日志服务的 AccessKeySecret
        val numReceivers = args(4).toInt  // 启动多少个 Receiver 来读取 logstore 中的数据
        val batchInterval = Milliseconds(args(5).toInt * 1000) // Spark Streaming 中每次处理批次时间间隔
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

-   方法二: Direct API Based DStream

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

    从E-MapReduce SDK 1.4.0版本开始，提供基于Direct API的实现方式。这种方式可以避免将Loghub数据重复存储到Write Ahead Log中，也即无需开启Spark Streaming的WAL特性即可实现数据的at least once。目前Direct API实现方式处于experimental状态，需要注意的地方有：

    -   在DStream的action中，必须做一次commit操作。
    -   一个Spark Streaming中，不支持对logstore数据源做多个action操作。
    -   Direct API方式需要zookeeper服务的支持。

## 支持MetaService {#section_ovl_2fh_hfb .section}

上面的例子中，我们都是显式地将AccessKey传入到接口中。不过从E-MapReduce SDK 1.3.2版本开始，Spark Streaming可以基于MetaService实现免AccessKey处理LogService数据。具体可以参考E-MapReduce SDK中的LoghubUtils类说明:

```
LoghubUtils.createStream(ssc, logServiceProject, logStoreName, loghubConsumerGroupName, storageLevel)
LoghubUtils.createStream(ssc, logServiceProject, logStoreName, loghubConsumerGroupName, numReceivers, storageLevel)
LoghubUtils.createStream(ssc, logServiceProject, logStoreName, loghubConsumerGroupName, storageLevel, cursorPosition, mLoghubCursorStartTime, forceSpecial)
LoghubUtils.createStream(ssc, logServiceProject, logStoreName, loghubConsumerGroupName, numReceivers, storageLevel, cursorPosition, mLoghubCursorStartTime, forceSpecial)
```

**说明：** 

-   E-MapReduce SDK支持Log Service的三种消费模式，即BEGIN\_CURSOR，END\_CURSOR和SPECIAL\_TIMER\_CURSOR，默认是“END\_CURSOR”。
    -   BEGIN\_CURSOR：从日志头开始消费，如果有checkpoint记录，则从checkpoint处开始消费。
    -   END\_CURSOR：从日志尾开始消费，如果有checkpoint记录，则从checkpoint处开始消费。
    -   SPECIAL\_TIMER\_CURSOR：从指定时间点开始消费，如果有checkpoint记录，则从checkpoint处开始消费。单位为秒。
    -   以上三种消费模式都受到checkpoint记录的影响，如果存在checkpoint记录，则从checkpoint处开始消费，不管指定的是什么消费模式。E-MapReduce SDK基于“SPECIAL\_TIMER\_CURSOR”模式支持用户强制在指定时间点开始消费：在LoghubUtils\#createStream接口中，以下参数需要组合使用：
        -   cursorPosition：LogHubCursorPosition.SPECIAL\_TIMER\_CURSOR
        -   forceSpecial：true
-   E-MapReduce 的机器（除了 Master 节点）无法连接公网。配置 LogService endpoint 时，请注意使用 Log Service 提供的内网 endpoint，否则无法请求到 Log Service。
-   更多关于 LogService，请查看[日志服务](https://www.alibabacloud.com/help/zh/product/28958.htm)。

## 附录 {#section_elt_kfh_hfb .section}

完整示例代码请看:

-   [Spark 接入 LogService](https://github.com/aliyun/aliyun-emapreduce-demo/blob/master/src/main/scala/com/aliyun/emr/example/LoghubSample.scala)

