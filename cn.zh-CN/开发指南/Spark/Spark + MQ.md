# Spark + MQ {#concept_jj2_qch_hfb .concept}

本章节介绍如何使用 Spark Streaming 如何使用 MQ 中的数据并计算每个批次中的单词数。

## 通过 Spark 访问 MQ {#section_rdt_5dh_hfb .section}

下面的示例演示 Spark Streaming 如何使用 MQ 中的数据并计算每个批次中的单词数：

```
val Array(cId, topic, subExpression, parallelism, interval) = args
    val accessKeyId = "<accessKeyId>"
    val accessKeySecret = "<accessKeySecret>"
    val numStreams = parallelism.toInt
    val batchInterval = Milliseconds(interval.toInt)
    val conf = new SparkConf().setAppName("Test ONS Streaming")
    val ssc = new StreamingContext(conf, batchInterval)
    def func: Message => Array[Byte] = msg => msg.getBody
    val onsStreams = (0 until numStreams).map { i =>
      println(s"starting stream $i")
      OnsUtils.createStream(
        ssc,
        cId,
        topic,
        subExpression,
        accessKeyId,
        accessKeySecret,
        StorageLevel.MEMORY_AND_DISK_2,
        func)
    }
    val unionStreams = ssc.union(onsStreams)
    unionStreams.foreachRDD(rdd => {
      rdd.map(bytes => new String(bytes)).flatMap(line => line.split(" "))
        .map(word => (word, 1))
        .reduceByKey(_ + _).collect().foreach(e => println(s"word: ${e._1}, cnt: ${e._2}"))
    })
    ssc.start()
    ssc.awaitTermination()
```

## 附录 {#section_ht1_ydh_hfb .section}

完整示例代码请参见：

-   [通过 Spark 访问 ONS](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/cn/emr/1.3.7/assets/sample/TestAliyunONS.scala)

