# Consume data offline {#concept_1079319 .concept}

This topic describes how to call Spark RDD API operations to develop an offline job to consume Log Service data.

## Sample code {#section_n3d_rkh_nwr .section}

``` {#codeblock_myk_usy_39l}
## TestBatchLoghub.Scala

object TestBatchLoghub {
  def main(args: Array[String]): Unit = {
    if (args.length < 6) {
      System.err.println(
        """Usage: TestBatchLoghub <sls project> <sls logstore> <sls endpoint>
          |  <access key id> <access key secret> <output path> <start time> <end time=now>
        """.stripMargin)
      System.exit(1)
    }

    val loghubProject = args(0)
    val logStore = args(1)
    val endpoint = args(2)
    val accessKeyId = args(3)
    val accessKeySecret = args(4)
    val outputPath = args(5)
    val startTime = args(6).toLong

    val sc = new SparkContext(new SparkConf().setAppName("test batch loghub"))
    var rdd:JavaRDD[String] = null
    if (args.length > 7) {
      rdd = LoghubUtils.createRDD(sc, loghubProject, logStore, accessKeyId, accessKeySecret, endpoint, startTime, args(7).toLong)
    } else {
      rdd = LoghubUtils.createRDD(sc, loghubProject, logStore, accessKeyId, accessKeySecret, endpoint, startTime)
    }

    rdd.saveAsTextFile(outputPath)
  }
}
```

**Note:** For more information about the Maven project object model \(POM\) file, see [aliyun-emapreduce-demo](https://github.com/aliyun/aliyun-emapreduce-demo/blob/master-2/pom.xml).

## Compilation and running {#section_tyn_wp5_ffq .section}

``` {#codeblock_d3e_hrx_b34}
## Compile and run a command.
mvn clean package -DskipTests

## After the compiled command is run, the JAR file of the job is stored in the target/shaded/ directory.

## Submit and run the job.

spark-submit --master yarn-cluster --executor-cores 2 --executor-memory 1g --driver-memory 1g 
--num-executors 2--class x.x.x.TestBatchLoghub xxx.jar <sls project> <sls logstore> 
<sls endpoint> <access key id> <access key secret> <output path> <start time> [<end time=now>]
```

**Note:** 

-   You need to specify the classpath and package path based on the actual situation in the format of x.x.x.TestBatchLoghub and xxx.jar.
-   You need to adjust the job resources based on the actual data size and cluster scale. If the cluster is too small, you may fail to run the job.

