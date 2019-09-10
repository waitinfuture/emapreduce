# Spark + OSS {#concept_obv_t1h_hfb .concept}

本章节介绍如何向OSS写数据。

## 背景信息 {#section_9nv_7pe_lti .section}

当前 E-MapReduce ：

-   支持[MetaService](../../../../cn.zh-CN/集群规划与配置/集群配置/MetaService.md#) 服务。
-   支持用户免 AK 访问 OSS 数据源。
-   支持旧的显式写 AK 和 Endpoint 方式访问 OSS 数据源。

    **说明：** OSS Endpoint 需使用内网域名，具体请参见[OSS Endpoint](../../../../cn.zh-CN/开发指南/访问域名（Endpoint）/访问域名和数据中心.md#)。


## Spark 接入 OSS {#section_grr_z1h_hfb .section}

以下示例演示了Spark如何免AK从OSS中读入数据，并将处理完的数据写回到OSS中。

``` {#codeblock_ejz_rbt_775}
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

示例代码请参见：[Spark 接入 OSS](https://github.com/aliyun/aliyun-emapreduce-demo/blob/master/src/main/scala/com/aliyun/emr/example/OSSSample.scala)

