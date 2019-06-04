# Use sample projects {#concept_ktz_lng_hfb .concept}

This sample project is a complete, compilable, and runnable project, including the sample code of MapReduce, Pig, Hive, and Spark.

## Sample project {#section_qld_r4g_hfb .section}

See the open source project. The details are as follows:

-   MapReduce

    WordCount: Counts the number of words.

-   Hive

    sample.hive: A simple search for the tables.

-   Pig

    sample.pig: OSS data instances processed by Pig.

-   Spark
    -   SparkPi: Calculates Pi.
    -   SparkWordCount: Counts the number of words.
    -   LinearRegression: Linear regression.
    -   OSSSample: OSS sample.
    -   ODPSSample: ODPS sample.
    -   MNSSample: MNS sample.
    -   LoghubSample: Loghub sample.

## Dependencies {#section_qfw_z4g_hfb .section}

-   Test data \(under the data directory\)
    -   The\_Sorrows\_of\_Young\_Werther.txt: Used as the input data of WordCount \(MapReduce/Spark\).
    -   patterns.txt: Filter characters of the WordCount \(MapReduce\) job.
    -   u.data: The data of the test table of the sample.hive script.
    -   abalone: Test data for linear regression algorithms.
-   The JAR dependencies \(under the lib directory\)

    tutorial.jar: The JAR dependencies required for the sample.pig job.


## Preparations {#section_bxj_jpg_hfb .section}

This project provides some test data. You can simply upload it to OSS to use it. For other samples, such as MaxCompute, MNS, ONS, and Log Service, you need to prepare the data as follows:

-   \(Optional\) Enable Log Service. See the [User Guide for Log Service](../../SP_7/DNSLS11878107/EN-US_TP_13018.dita#concept_gpw_x2w_ydb).

-   \(Optional\) Create MaxCompute projects and tables. See [Create a MaxCompute project](../../../../reseller.en-US/Prepare/Create a project.md#) and [Quick Start for MaxCompute](../../../../reseller.en-US/Quick Start/Create and view a table.md#).

-   \(Optional\) Use ONS. See [Quick Start for Message Queue](https://partners-intl.aliyun.com/help/doc-detail/34411.htm).

-   \(Optional\) Use MNS. See [topics for using the Message Service console](https://partners-intl.aliyun.com/help/doc-detail/34063.htm).


## Concepts {#section_wyf_mpg_hfb .section}

-   OSSURI: oss://accessKeyId:accessKeySecret@bucket.endpoint/a/b/c.txt. Specify the input and output data sources. OSSURI is similar to URLs like hdfs://.

-   The combination of AccessKey ID and AccessKey Secret is the key for you to access an Alibaba Cloud API. Click [here](https://partners-intl.aliyun.com/login-required#/accesskey) to obtain it.


## Run jobs in clusters {#section_vyy_4pg_hfb .section}

-   Spark

    -   SparkWordCount：

``` {#codeblock_o8x_8vw_ozi}
spark-submit --class SparkWordCount examples-1.0-SNAPSHOT-shaded.jar <inputPath>
                <outputPath> <numPartition>
```

        Parameters are described as follows:

        -   inputPath: Data input path.

        -   outputPath: Data output path.

        -   numPartition: The number of RDD partitions of the input data.

    -   SparkPi: `spark-submit --class SparkPi examples-1.0-SNAPSHOT-shaded.jar`

    -   OSSSample:

``` {#codeblock_1n9_3f8_tre}
spark-submit --class OSSSample examples-1.0-SNAPSHOT-shaded.jar <inputPath>
                <numPartition>
```

        Parameters are described as follows:

        -   inputPath: Data input path.

        -   numPartition: Input the number of data RDD partitions.

    -   ONSSample:

``` {#codeblock_zkc_cj9_nlf}
spark-submit --class ONSSample examples-1.0-SNAPSHOT-shaded.jar <accessKeyId>
                <accessKeySecret> <consumerId> <topic> <subExpression> <parallelism>
```

        Parameters are described as follows:

        -   accessKeyId: Alibaba Cloud AccessKey ID.

        -   accessKeySecret: Alibaba Cloud AccessKey Secret.

        -   topic: Each message queue has a topic.

        -   subExpression: See [Message filtering](https://partners-intl.aliyun.com/help/doc-detail/29543.htm).

        -   parallelism: Specifies the number of receivers to consume messages in the queue.

    -   MaxComputeSample:

``` {#codeblock_2x2_dkl_b9q}
spark-submit --class MaxComputeSample examples-1.0-SNAPSHOT-shaded.jar <accessKeyId>
                <accessKeySecret> <envType> <project> <table> <numPartitions>
```

        Parameters are described as follows:

        -   accessKeyId: Alibaba Cloud AccessKey ID.

        -   accessKeySecret: Alibaba Cloud AccessKey Secret.

        -   envType: 0 indicates the public network, and 1 indicates the private network. Select 0 for local debugging, and select 1 for execution on E-MapReduce.

        -   project: see [MaxCompute Quick Start](../../SP_76/DNODPS1871666/EN-US_TP_11945.dita#concept_qbk_1kv_tdb).

        -   numPartition: The number of RDD partitions of the input data.

    -   MNSSample:

``` {#codeblock_a3r_lfz_nde}
spark-submit --class MNSSample examples-1.0-SNAPSHOT-shaded.jar <queueName>
                <accessKeyId> <accessKeySecret> <endpoint>
```

        Parameters are described as follows:

        -   queueName: Queue name. See [MNS glossary](https://partners-intl.aliyun.com/help/doc-detail/34061.htm).

        -   accessKeyId: Alibaba Cloud AccessKey ID.

        -   accessKeySecret: Alibaba Cloud AccessKey Secret.

        -   endpoint: Address for accessing the queue data.

    -   LoghubSample:

``` {#codeblock_onz_afr_hji}
spark-submit --class LoghubSample examples-1.0-SNAPSHOT-shaded.jar <sls project> <sls
                logstore> <loghub group name> <sls endpoint> <access key id> <access key secret> <batch
                interval seconds>
```

        Parameters are described as follows:

        -   sls project: The name of the Log Service project.

        -   sls logstore: Logstore name.

        -   loghub group name: The name of the group that consumes Logstore data in jobs. You can specify a name as needed. When the values of sls project and sls store are the same, jobs with the same group name consume data in sls store collaboratively. Jobs with different group names consume data in sls store separately.

        -   sls endpoint: See [Service endpoint](../../SP_7/DNSLS11825216/EN-US_TP_13218.dita#reference_wgx_pwq_zdb).

        -   accessKeyId: Alibaba Cloud AccessKey ID.

        -   accessKeySecret: Alibaba Cloud AccessKey Secret.

        -   batch interval seconds: The interval between Spark Streaming jobs \(unit: seconds\).

    -   LinearRegression:

``` {#codeblock_dgh_zj2_vb9}
spark-submit --class LinearRegression examples-1.0-SNAPSHOT-shaded.jar <inputPath>
                <numPartitions>
```

        Parameters are described as follows:

        -   inputPath: Data input path.

        -   numPartition: The number of RDD partitions of the input data.

-   MapReduce

    -   WordCount:

``` {#codeblock_ajv_kxp_4th}
hadoop jar examples-1.0-SNAPSHOT-shaded.jar WordCount
                -Dwordcount.case.sensitive=true <inputPath> <outputPath> -skip <patternPath>
```

        Parameters are described as follows:

        -   inputPath: Data input path.

        -   outputPath: Data output path.

        -   patternPath: The file that contains characters to be filtered. You can use data/patterns.txt.

-   Hive

    -   `hive -f sample.hive -hiveconf inputPath=<inputPath>`

        Parameters are described as follows:

        -   inputPath: Data input path.
-   Pig

    -   ``` {#codeblock_e6e_31u_7y0}
pig -x mapreduce -f sample.pig -param tutorial=<tutorialJarPath> -param
                input=<inputPath> -param result=<resultPath>
```

        Parameters are described as follows:

        -   tutorialJarPath: The JAR dependencies. You can use lib/tutorial.jar.

        -   inputPath: Data input path.

        -   resultPath: Data output path.


**Note:** 

-   If you are working on E-MapReduce, upload the testing data and the JAR dependencies to OSS. The path rule should follow the definition of OSSURI, as described above.
-   If used in a cluster, it can be stored locally.

## Run Locally {#section_xcw_lqg_hfb .section}

Here we describe how to run a Spark program locally to visit Alibaba Cloud's data sources, such as OSS. If you want to debug and run the program locally, we recommend that you use some development tools, such as IntelliJ IDEA or Eclipse, especially when using Windows. Otherwise, you need to configure Hadoop and Spark runtime environments on Windows machines.

-   IntelliJ IDEA
    -   Preparations

        Install IntelliJ IDEA, Maven, Maven plugin for IntelliJ IDEA, Scala and Scala plugin for IntelliJ IDEA.

    -   Development process
        1.  Double-click to enter SparkWordCount.scala.

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17968/155963872113087_en-US.jpg)

        2.  Enter the job configuration page from the arrow as shown in the following figure.

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17968/155963872113088_en-US.jpg)

        3.  Select SparkWordCount and enter the required job parameters in the job parameter box.

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17968/155963872113089_en-US.jpg)

        4.  Click **OK**.
        5.  Click Run to run the job.

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17968/155963872113090_en-US.jpg)

        6.  View job logs

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17968/155963872113091_en-US.jpg)

-   Scala IDE for Eclipse
    -   Preparations

        Install the Scala IDE for Eclipse, Maven and the Maven plugin for Eclipse.

    -   Development process
        1.  Import a project as described in the figure below:

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17968/155963872113093_en-US.jpg)

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17968/155963872113094_en-US.jpg)

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17968/155963872113095_en-US.jpg)

        2.  The shortcut for Run as Maven build is Alt + Shift + X, M. You can also right-click the project name and choose **Run As** \> **Maven build**.
        3.  Right-click on the job to run after it has been compiled, select **Run Configuration** to enter the configuration page.
        4.  In the configuration page, select Scala Application and configure the Main Class and parameters of the job, As shown in the following figure:

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17968/155963872113097_en-US.jpg)

        5.  Click **Run**.
        6.  View the output log of the console, as shown in the following figure:

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17968/155963872113098_en-US.jpg)


