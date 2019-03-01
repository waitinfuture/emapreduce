# Spark + OSS {#concept_obv_t1h_hfb .concept}

当前E-MapReduce支持[MetaService](../../../../../intl.zh-CN/用户指南/MetaService.md#)服务，支持用户在E-MapReduce环境免AK访问OSS数据源。旧的显式写AK和Endpoint方式也支持，但需要注意OSS Endpoint请使用内网域名，所有的Endpoint可以参考 [OSS Endpoint](../../../../../intl.zh-CN/开发指南/访问域名（Endpoint）/访问域名和数据中心.md#)。

## Spark 接入 OSS {#section_grr_z1h_hfb .section}

下面这个例子演示了Spark如何免AK从OSS中读入数据，并将处理完的数据写回到OSS 中。

```
val conf = new SparkConf().setAppName("Test OSS")
    val sc = new SparkContext(conf)
    val pathIn = "oss://bucket/path/to/read"
    val inputData = sc.textFile(pathIn)
    val cnt = inputData.count
    println(s"count: $cnt")
    val outputPath = "oss://bucket/path/to/write"
    val outpuData = inputData.map(e => s"$e has been processed.")
    outpuData.saveAsTextFile(outputPath)
```

## 附录 {#section_pcy_bbh_hfb .section}

示例代码请看:

-   [Spark接入OSS](https://github.com/aliyun/aliyun-emapreduce-demo/blob/master/src/main/scala/com/aliyun/emr/example/OSSSample.scala)

