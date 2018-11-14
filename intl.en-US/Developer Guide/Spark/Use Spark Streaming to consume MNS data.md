# Use Spark Streaming to consume MNS data {#concept_hpw_pfh_hfb .concept}

This topic describes how to use Spark Streaming to consume data in MNS and calculate the word count of every batch.

## Allow Spark to access MNS {#section_otg_rfh_hfb .section}

The following example describes how to use Spark Streaming to consume data in MNS and calculate the word count of every batch.

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

## Use Spark Streaming with MetaService {#section_qbc_tfh_hfb .section}

In the preceding example, we explicitly pass the AccessKey to the API. However, since E-MapReduce SDK 1.3.2, Spark Streaming can process MNS data using MetaService without an AccessKey. For more information, see the description of the MnsUtils class in E-MapReduce SDK:

```
MnsUtils.createPullingStreamAsBytes(ssc, queueName, endpoint, storageLevel)
MnsUtils.createPullingStreamAsRawBytes(ssc, queueName, endpoint, storageLevel)
```

## Appendix {#section_sbc_tfh_hfb .section}

For the complete sample code, see:

-   [Allow Spark to access MNS](https://github.com/aliyun/aliyun-emapreduce-demo/blob/master/src/main/scala/com/aliyun/emr/example/MNSSample.scala)

