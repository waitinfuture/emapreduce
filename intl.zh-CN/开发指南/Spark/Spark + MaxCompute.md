# Spark + MaxCompute {#concept_tbv_dbh_hfb .concept}

本章节将介绍如何使用E-MapReduce SDK在Spark中完成一次MaxCompute数据的读写操作。

## Spark 接入 MaxCompute {#section_jrz_pbh_hfb .section}

1.  初始化一个OdpsOps对象。在 Spark 中，MaxCompute的数据操作通过OdpsOps类完成，请参照如下步骤创建一个OdpsOps对象：

    ```
    import com.aliyun.odps.TableSchema
         import com.aliyun.odps.data.Record
         import org.apache.spark.aliyun.odps.OdpsOps
         import org.apache.spark.{SparkContext, SparkConf}
         object Sample {
           def main(args: Array[String]): Unit = {    
             // == Step-1 ==
             val accessKeyId = "<accessKeyId>"
             val accessKeySecret = "<accessKeySecret>"
             // 以内网地址为例
             val urls = Seq("http://odps-ext.aliyun-inc.com/api", "http://dt-ext.odps.aliyun-inc.com") 
             val conf = new SparkConf().setAppName("Test Odps")
             val sc = new SparkContext(conf)
             val odpsOps = OdpsOps(sc, accessKeyId, accessKeySecret, urls(0), urls(1))
             // 下面是一些调用代码
             // == Step-2 ==
             ...
             // == Step-3 ==
             ...
           }
           // == Step-2 ==
           // 方法定义1
           // == Step-3 ==
           // 方法定义2
         ｝
    ```

2.  从MaxCompute中加载表数据到Spark中。通过OdpsOps对象的readTable方法，可以将MaxCompute中的表加载到Spark中，即生成一个RDD，如下所示：

    ```
    // == Step-2 ==
             val project = <odps-project>
             val table = <odps-table>
             val numPartitions = 2
             val inputData = odpsOps.readTable(project, table, read, numPartitions)
             inputData.top(10).foreach(println)
             // == Step-3 ==
             ...
    ```

    在上面的代码中，您还需要定义一个read函数，用来解析和预处理MaxCompute表数据，如下所示：

    ```
    def read(record: Record, schema: TableSchema): String = {
               record.getString(0)
             }
    ```

    这个函数的含义是将MaxCompute表的第一列加载到Spark运行环境中。

3.  将 Spark 中的结果数据保存到MaxCompute表中。通过OdpsOps对象的saveToTable方法，可以将Spark RDD持久化到MaxCompute中。

    ```
    val resultData = inputData.map(e => s"$e has been processed.")
             odpsOps.saveToTable(project, table, dataRDD, write)
    ```

    在上面的代码中，您还需要定义一个write函数，用作写MaxCompute表前数据预处理，如下所示：

    ```
    def write(s: String, emptyReord: Record, schema: TableSchema): Unit = {
               val r = emptyReord
               r.set(0, s)
             }
    ```

    这个函数的含义是将RDD的每一行数据写到对应MaxCompute表的第一列中。

4.  分区表参数写法说明

    SDK支持对MaxCompute分区表的读写，这里分区名的写法标准是：分区列名=分区名，多个分区时以逗号分隔，例如有分区列pt和ps：

    -   读分区pt为1的表数据：pt=‘1’
    -   读分区pt为1和分区ps为2的表数据：pt=‘1’,ps=‘2’

## 附录 {#section_wdp_3ch_hfb .section}

示例代码请看:

-   [Spark接入MaxCompute](https://github.com/aliyun/aliyun-emapreduce-demo/blob/master/src/main/scala/com/aliyun/emr/example/ODPSSample.scala)

