# Use Spark and write data to MySQL {#concept_vc2_5jf_hfb .concept}

This topic describes how to run Spark jobs in Hadoop clusters of E-MapReduce to calculate the word count and write the result to MySQL. A Spark Streaming job writes data to MySQL in a similar way as a Spark job does.

## Allow Spark to access MySQL {#section_ky2_t2g_hfb .section}

Use the following code:

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
          if (ps ! = null) {
            ps.close()
          }
          if (conn ! = null) {
            conn.close()
          }
        }
      Iterator.empty
    }).count()
```

## Appendix {#section_r1y_qfg_hfb .section}

For the complete sample code, see:

-   [Allow Spark to write data to RDS](https://github.com/aliyun/aliyun-emapreduce-demo/blob/master-2/src/main/scala/com/aliyun/emr/example/spark/RDSSample1.scala)

