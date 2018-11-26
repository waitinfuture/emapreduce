# Use Spark to access OSS {#concept_obv_t1h_hfb .concept}

E-MapReduce supports [MetaService](../DNemapreduce1876943/EN-US_TP_17925.dita#concept_wj5_fpt_1fb), which allows you to access OSS data in the E-MapReduce environment without an AccessKey. The previous way of using an AccessKey and endpoint is also supported. Make sure that you use an internal IP address for the OSS endpoint. For more information about a complete list of endpoints, see [OSS endpoints](../../SP_21/DNOSS11827291/EN-US_TP_4350.dita#concept_zt4_cvy_5db).

## Allow Spark to access OSS {#section_grr_z1h_hfb .section}

The following example shows how Spark reads data from OSS without an AccessKey and writes the processed data back to OSS.

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

## Appendix {#section_pcy_bbh_hfb .section}

For the complete sample code, see:

-   [Allow Spark to access OSS](https://github.com/aliyun/aliyun-emapreduce-demo/blob/master/src/main/scala/com/aliyun/emr/example/OSSSample.scala)

