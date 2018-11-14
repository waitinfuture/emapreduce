# Use Spark Streaming to consume MQ data {#concept_jj2_qch_hfb .concept}

This topic describes how to use Spark Streaming to consume MQ data and count the words in each batch.

## Use Spark to access MQ {#section_rdt_5dh_hfb .section}

The following example shows how to use Spark Streaming to consume MQ data and count the words in each batch.

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

## Appendix {#section_ht1_ydh_hfb .section}

For the complete sample code, see:

-   [Allow Spark to access ONS](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/cn/emr/1.3.7/assets/sample/TestAliyunONS.scala)

