# Hudi

Hudi是一种数据湖的存储格式，在Hadoop文件系统之上提供了更新数据和删除数据的能力以及流式消费变化数据的能力。

## 应用场景

-   近实时数据摄取

    Hudi支持插入、更新和删除数据的能力。您可以实时摄取消息队列（Kafka）和日志服务SLS等日志数据至Hudi中，同时也支持实时同步数据库Binlog产生的变更数据。

    Hudi优化了数据写入过程中产生的小文件。因此，相比其他传统的文件格式，Hudi对HDFS文件系统更加的友好。

-   近实时数据分析

    Hudi支持多种数据分析引擎，包括Hive、Spark、Presto和Impala。Hudi作为一种文件格式，不需要依赖额外的服务进程，在使用上也更加的轻量化。

-   增量数据处理

    Hudi支持Incremental Query查询类型，您可以通过Spark Streaming查询给定COMMIT后发生变更的数据。Hudi提供了一种流式消费HDFS变化数据的能力，可以用来优化现有的系统架构。


## 表类型

Hudi支持如下两种表类型：

-   Copy On Write

    使用专门的列式文件格式（Parquet）存储数据。Copy On Write表的更新操作需要通过重写实现。

-   Merge On Read

    使用列式文件格式（Parquet）和行式文件格式（Avro）混合的方式来存储数据。Merge On Read使用列式格式存放Base数据，同时使用行式格式存放增量数据。最新写入的增量数据存放至行式文件中，根据可配置的策略执行COMPACTION操作合并增量数据至列式文件中。


## 查询类型

Hudi支持如下三种查询类型：

-   Snapshot Queries

    可以查询COMMIT的最新快照数据。针对Merge On Read类型的表，查询时需要在线合并列存中的Base数据和日志中的实时数据；针对Merge On Write表，可以查询最新版本的Parquet数据。

    Copy On Write和Merge On Read表支持该类型的查询。

-   Incremental Queries

    支持增量查询的能力，可以查询给定COMMIT之后的最新数据。

    仅Copy On Write表支持该类型的查询。

-   Read Optimized Queries

    只能查询到给定COMMIT之前所限定范围的最新数据。Read Optimized Queries是对Merge On Read表类型快照查询的优化。

    Copy On Write和Merge On Read表支持该类型的查询。


## 数据写入

EMR-3.32.0以及后续版本中，默认支持Hudi服务，因此使用Hudi时只需要在pom文件中添加Hudi依赖即可。

```
<dependency>
   <groupId>org.apache.hudi</groupId>
   <artifactId>hudi-spark_2.11</artifactId>
   <version>0.6.0</version>
  <scope>provided</scope>
</dependency>
```

写数据示例如下：

-   插入或更新数据

    ```
     val spark = SparkSession
          .builder()
          .master("local[*]")
          .appName("hudi test")
          .config("spark.serializer", "org.apache.spark.serializer.KryoSerializer")
          .getOrCreate()
    
    import spark.implicits._
        val df = (for (i <- 0 until 10) yield (i, s"a$i", 30 + i * 0.2, 100 * i + 10000, s"p${i % 5}"))
          .toDF("id", "name", "price", "version", "dt")
    
        df.write.format("hudi")
          .option(TABLE_NAME, "hudi_test_0")
          // .option(OPERATION_OPT_KEY, UPSERT_OPERATION_OPT_VAL) //更新数据
          .option(OPERATION_OPT_KEY, INSERT_OPERATION_OPT_VAL) //插入数据
          .option(RECORDKEY_FIELD_OPT_KEY, "id")
          .option(PRECOMBINE_FIELD_OPT_KEY, "version")
          .option(KEYGENERATOR_CLASS_OPT_KEY, classOf[SimpleKeyGenerator].getName)
          .option(HIVE_PARTITION_EXTRACTOR_CLASS_OPT_KEY, classOf[MultiPartKeysValueExtractor].getCanonicalName)
          .option(PARTITIONPATH_FIELD_OPT_KEY, "dt")
          .option(HIVE_PARTITION_FIELDS_OPT_KEY, "ds")
          .option(META_SYNC_ENABLED_OPT_KEY, "true")//开启元数据同步功能
          .option(HIVE_USE_JDBC_OPT_KEY, "false")
          .option(HIVE_DATABASE_OPT_KEY, "default")
          .option(HIVE_TABLE_OPT_KEY, "hudi_test_0")
          .option(INSERT_PARALLELISM, "8")
          .option(UPSERT_PARALLELISM, "8")
          .mode(Overwrite)
          .save("/tmp/hudi/h0")
    ```

-   删除数据

    ```
    df.write.format("hudi")
          .option(TABLE_NAME, "hudi_test_0")
          .option(OPERATION_OPT_KEY, DELETE_OPERATION_OPT_VAL) // 删除数据
          .option(RECORDKEY_FIELD_OPT_KEY, "id")
          .option(PRECOMBINE_FIELD_OPT_KEY, "version")
          .option(KEYGENERATOR_CLASS_OPT_KEY, classOf[SimpleKeyGenerator].getName)
          .option(PARTITIONPATH_FIELD_OPT_KEY, "dt")
          .option(DELETE_PARALLELISM, "8")
          .mode(Append)
          .save("/tmp/hudi/h0")
    ```


## 数据查询

Hive或Presto查询Hudi表时，需要在写入阶段开启元数据同步功能（设置META\_SYNC\_ENABLED\_OPT\_KEY为true）。

针对社区版Hudi， Copy On Write表和Merge On Read表需要设置`set hive.input.format = org.apache.hudi.hadoop.hive.HoodieCombineHiveInputFormat`。EMR版本的Copy On Write表不用设置format参数。

Hudi的详细信息，请参见[https://hudi.apache.org/](https://hudi.apache.org/)。

