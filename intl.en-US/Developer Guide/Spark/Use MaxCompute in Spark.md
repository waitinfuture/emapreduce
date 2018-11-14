# Use MaxCompute in Spark {#concept_tbv_dbh_hfb .concept}

This topic describes how to use the E-MapReduce SDK to read and write MaxCompute data in Spark.

## Allow Spark to access MaxCompute {#section_jrz_pbh_hfb .section}

1.  Initialize an OdpsOps object. In Spark, data operations of MaxCompute are performed using the OdpsOps class. Follow these steps to create an OdpsOps object:

    ```
    import com.aliyun.odps.TableSchema
         import com.aliyun.odps.data.Record
         import org.apache.spark.aliyun.odps.OdpsOps
         import org.apache.spark.{ SparkContext, SparkConf}
         object Sample {
           def main(args: Array[String]): Unit = {    
             // == Step-1 ==
             val accessKeyId = "<accessKeyId>"
             val accessKeySecret = "<accessKeySecret>"
             // Take the internal network address as an example
             val urls = Seq("http://odps-ext.aliyun-inc.com/api", "http://dt-ext.odps.aliyun-inc.com") 
             val conf = new SparkConf().setAppName("Test Odps")
             val sc = new SparkContext(conf)
             val odpsOps = OdpsOps(sc, accessKeyId, accessKeySecret, urls(0), urls(1))
             // A part of the calling code is shown as follows
             // == Step-2 ==
             ...
             // == Step-3 ==
             ...
           }
           // == Step-2 ==
           // Method definition 1
           // == Step-3 ==
           // Method definition 2
         }
    ```

2.  Load the table data from MaxCompute to Spark. By using the readTable method of the OdpsOps object, you can load a MaxCompute table to Spark and create an RDD as follows:

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

    In the preceding code, you must define a read function to parse and pre-process the MaxCompute table data, as follows:

    ```
    def read(record: Record, schema: TableSchema): String = {
               record.getString(0)
             }
    ```

    This function loads the first column of the MaxCompute table into the Spark runtime environment.

3.  Save the result data in Spark to the MaxCompute table. Using the saveToTable method of the OdpsOps object, you can save Spark RDD to MaxCompute.

    ```
    val resultData = inputData.map(e => s"$e has been processed.")
             odpsOps.saveToTable(project, table, dataRDD, write)
    ```

    In the preceding code, you must define a write function for data pre-processing before writing to the MaxCompute table as follows:

    ```
    def write(s: String, emptyReord: Record, schema: TableSchema): Unit = {
               val r = emptyReord
               r.set(0, s)
             }
    ```

    This function writes RDD data in each row to the first column of the corresponding MaxCompute table.

4.  Notation of the partition table parameters.

    The SDK supports reading and writing MaxCompute partition tables. The standard naming of the tables is partition\_column\_name=partition\_name \(with multiple partitions separated by commas\). Assume that we have partition columns pt and ps:

    -   Read the table data of the partition where pt is 1.
    -   Read the table data of the partition where pt is 1 and ps is 2.

## Appendix {#section_wdp_3ch_hfb .section}

For the complete sample code, see:

-   [Allow Spark to access MaxCompute](https://github.com/aliyun/aliyun-emapreduce-demo/blob/master/src/main/scala/com/aliyun/emr/example/ODPSSample.scala)

