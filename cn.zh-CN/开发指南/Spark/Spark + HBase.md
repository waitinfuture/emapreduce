# Spark + HBase {#concept_n5q_vfh_hfb .concept}

本章节将介绍 Spark 如何向 Hbase 写数据。

**说明：** 

计算集群需要和 Hbase 集群处于一个安全组内，否则网络无法打通。在 E-Mapreduce 创建集群时，请注意选择 Hbase 集群所处的安全组

## Spark 接入 Hbase {#section_qs1_xfh_hfb .section}

代码如下：

```
object ConnectionUtil extends Serializable {
      private val conf = HBaseConfiguration.create()
      conf.set(HConstants.ZOOKEEPER_QUORUM,"ecs1,ecs1,ecs3")
      conf.set(HConstants.ZOOKEEPER_ZNODE_PARENT, "/hbase")
      private val connection = ConnectionFactory.createConnection(conf)
      def getDefaultConn: Connection = connection
    }
    //创建数据流 unionStreams
    unionStreams.foreachRDD(rdd => {
      rdd.map(bytes => new String(bytes))
        .flatMap(line => line.split(" "))
        .map(word => (word, 1))
        .reduceByKey(_ + _)
        .mapPartitions {words => {
          val conn = ConnectionUtil.getDefaultConn
          val tableName = TableName.valueOf(tname)
          val t = conn.getTable(tableName)
          try {
            words.sliding(100, 100).foreach(slice => {
              val puts = slice.map(word => {
                println(s"word: $word")
                val put = new Put(Bytes.toBytes(word._1 + System.currentTimeMillis()))
                put.addColumn(COLUMN_FAMILY_BYTES, COLUMN_QUALIFIER_BYTES,
                  System.currentTimeMillis(), Bytes.toBytes(word._2))
                put
              }).toList
              t.put(puts)
            })
          } finally {
            t.close()
          }
          Iterator.empty
        }}.count()
    })
    ssc.start()
    ssc.awaitTermination()
```

## 附录 {#section_tbr_yfh_hfb .section}

完整示例代码请看:

-   [Spark 接入 Hbase](https://github.com/aliyun/aliyun-emapreduce-demo/blob/master/src/main/scala/com/aliyun/emr/example/streaming/HBaseSample.scala)

