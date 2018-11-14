# Use Spark to write data to HBase {#concept_n5q_vfh_hfb .concept}

This topic describes how Spark writes data to HBase. Note that computing clusters must be in the same security group as HBase clusters. Otherwise, the network cannot be connected. When you create a cluster in E-MapReduce, make sure that you select the security group where the HBase cluster is located.

## Allow Spark to access HBase {#section_qs1_xfh_hfb .section}

Use the following code:

```
object ConnectionUtil extends Serializable {
      private val conf = HBaseConfiguration.create()
      conf.set(HConstants.ZOOKEEPER_QUORUM,"ecs1,ecs1,ecs3")
      conf.set(HConstants.ZOOKEEPER_ZNODE_PARENT, "/hbase")
      private val connection = ConnectionFactory.createConnection(conf)
      def getDefaultConn: Connection = connection
    }
    //Create data streaming unionStreams
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
                val put = new Put(Bytes.toBytes(word. _1 + System.currentTimeMillis()))
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

## Appendix {#section_tbr_yfh_hfb .section}

For the complete sample code, see:

-   [Allow Spark to access HBase](https://github.com/aliyun/aliyun-emapreduce-demo/blob/master/src/main/scala/com/aliyun/emr/example/streaming/HBaseSample.scala)

