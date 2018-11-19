# Spark + MNS {#concept_hpw_pfh_hfb .concept}

本章节将介绍Spark Streaming如何消费 MNS 中的数据，统计每个 batch 内的单词个数。

## Spark 接入 MNS {#section_otg_rfh_hfb .section}

下面这个例子演示了 Spark Streaming 如何消费 MNS 中的数据，统计每个 batch 内的单词个数。

```
val conf = new SparkConf().setAppName("Test MNS Streaming")
    val batchInterval = Seconds(10)
    val ssc = new StreamingContext(conf, batchInterval)
    val queuename = "queuename"
    val accessKeyId = "<accessKeyId>"
    val accessKeySecret = "<accessKeySecret>"
    val endpoint = "http://xxx.yyy.zzzz/abc"
    val mnsStream = MnsUtils.createPullingStreamAsRawBytes(ssc, queuename, accessKeyId, accessKeySecret, endpoint,
      StorageLevel.MEMORY_ONLY)
    mnsStream.foreachRDD( rdd => {
      rdd.map(bytes => new String(bytes)).flatMap(line => line.split(" "))
        .map(word => (word, 1))
        .reduceByKey(_ + _).collect().foreach(e => println(s"word: ${e._1}, cnt: ${e._2}"))
    })
    ssc.start()
    ssc.awaitTermination()
```

## 支持MetaService {#section_qbc_tfh_hfb .section}

上面的例子中，我们都是显式地将AK传入到接口中。不过从E-MapReduce SDK 1.3.2版本开始，Spark Streaming可以基于MetaService实现免AK处理MNS数据。具体可以参考E-MapReduce SDK中的MnsUtils类说明：

```
MnsUtils.createPullingStreamAsBytes(ssc, queueName, endpoint, storageLevel)
MnsUtils.createPullingStreamAsRawBytes(ssc, queueName, endpoint, storageLevel)
```

## 附录 {#section_sbc_tfh_hfb .section}

完整示例代码请看:

-   [Spark 接入 MNS](https://github.com/aliyun/aliyun-emapreduce-demo/blob/master/src/main/scala/com/aliyun/emr/example/MNSSample.scala)

