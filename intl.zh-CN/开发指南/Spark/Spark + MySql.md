# Spark + MySql {#concept_vc2_5jf_hfb .concept}

本章节介绍在E-MapReduce的Hadoop集群运行Spark作业统计单词个数，将结果写入MySQL。Spark Streaming作业写入MySQL的方法与此类似。

## Spark 接入 MySQL {#section_ky2_t2g_hfb .section}

代码如下：

```
val input = getSparkContext.textFile(inputPath, numPartitions)
    input.flatMap(_.split(" ")).map(x => (x, 1)).reduceByKey(_ + _)
      .mapPartitions(e => {
        var conn: Connection = null
        var ps: PreparedStatement = null
        val sql = s"insert into $tbName(word, count) values (?, ?)"
        try {
          conn = DriverManager.getConnection(s"jdbc:mysql://$dbUrl:$dbPort/$dbName", dbUser, dbPwd)
          ps = conn.prepareStatement(sql)
          e.foreach(pair => {
            ps.setString(1, pair._1)
            ps.setLong(2, pair._2)
            ps.executeUpdate()
          })

          ps.close()
          conn.close()
        } catch {
          case e: Exception => e.printStackTrace()
        } finally {
          if (ps != null) {
            ps.close()
          }
          if (conn != null) {
            conn.close()
          }
        }
      Iterator.empty
    }).count()
```

## 附录 {#section_r1y_qfg_hfb .section}

完整示例代码请看：

-   [Spark写入RDS](https://github.com/aliyun/aliyun-emapreduce-demo/blob/master-2/src/main/scala/com/aliyun/emr/example/spark/RDSSample1.scala)

