# Consume data in real time {#concept_1079275 .concept}

This topic describes how to call Spark DataFrame API operations to develop a streaming job to consume Log Service data.

## Sample code {#section_1ou_bfz_jbq .section}

``` {#codeblock_b99_ynm_g39}
## StructuredLoghubWordCount.Scala

object StructuredLoghubSample {
  def main(args: Array[String]) {
    if (args.length < 7) {
      System.err.println("Usage: StructuredLoghubSample <logService-project> " +
        "<logService-store> <access-key-id> <access-key-secret> <endpoint> " +
        "<starting-offsets> <max-offsets-per-trigger> [<checkpoint-location>]")
      System.exit(1)
    }

    val Array(project, logStore, accessKeyId, accessKeySecret, endpoint, startingOffsets, maxOffsetsPerTrigger, outputPath, _*) = args
    val checkpointLocation =
      if (args.length > 8) args(8) else "/tmp/temporary-" + UUID.randomUUID.toString

    val spark = SparkSession
      .builder
      .appName("StructuredLoghubSample")
      .master("local[5]")
      .getOrCreate()

    import spark.implicits. _

    // Create a dataset to represent the stream of input lines from LogHub.
    val lines = spark
      .readStream
      .format("loghub")
      .option("sls.project", project)
      .option("sls.store", logStore)
      .option("access.key.id", accessKeyId)
      .option("access.key.secret", accessKeySecret)
      .option("endpoint", endpoint)
      .option("startingoffsets", startingOffsets)
      .option("zookeeper.connect.address", "localhost:2181")
      .option("maxOffsetsPerTrigger", maxOffsetsPerTrigger)
      .load()
      .selectExpr("CAST(content AS STRING)")
      .as[String]

    val query = lines.writeStream
      .format("parquet")
      .option("checkpointLocation", checkpointLocation)
      .option("path", outputPath)
      .outputMode("append")
      .trigger(Trigger.ProcessingTime(30000))
      .start()

    query.awaitTermination()
  }
}
```

**Note:** For more information about the Maven project object model \(POM\) file, see [aliyun-emapreduce-demo](https://github.com/aliyun/aliyun-emapreduce-demo/blob/master-2/pom.xml).

## Compilation and running {#section_9c6_n66_b4a .section}

``` {#codeblock_xhm_mp1_kcd}
## Compile and run a command.
mvn clean package -DskipTests

## After the compiled command is run, the JAR file of the job is stored in the target/shaded/ directory.

## Submit and run the job.

spark-submit --master yarn-cluster --executor-cores 2 --executor-memory 1g --driver-memory 1g 
--num-executors 2--class x.x.x.StructuredLoghubSample xxx.jar <logService-project> 
<logService-store> <access-key-id> <access-key-secret> <endpoint> <starting-offsets> 
<max-offsets-per-trigger> <zookeeper-connect-address> <output-path> <checkpoint-location>
```

**Note:** 

-   You need to specify the classpath and package path based on the actual situation in the format of x.x.x.StructuredLoghubSample and xxx.jar.
-   You need to adjust the job resources based on the actual data size and cluster scale. If the cluster is too small, you may fail to run the job.

## Notes {#section_2c8_izj_43p .section}

Some Spark versions are incompatible with E-MapReduce \(EMR\). Some EMR versions are incompatible with the emr-logservice SDK. The following table lists compatible Spark, EMR, and emr-logservice SDK versions.

|emr-logservice SDK version|Spark version|EMR version|
|--------------------------|-------------|-----------|
|1.6.0|2.3.1|EMR 3.18.x or earlier|
|1.7.0|2.4.3|EMR 3.19.x or later|

