# Use the sample project

This sample project is a complete, compilable, and executable project. It includes the sample code of MapReduce, Pig, Hive, and Spark.

## Sample project

The sample project includes the following jobs:

-   MapReduce

    WordCount: counts how often a word occurs in a file.

-   Hive

    sample.hive: queries data from tables.

-   Pig

    sample.pig: processes Object Storage Service \(OSS\) objects.

-   Spark
    -   SparkPi: calculates Pi.
    -   SparkWordCount: counts how often a word occurs in a file.
    -   LinearRegression: trains a linear regression model and extracts model summary statistics.
    -   OSSSample: uses Spark with OSS.
    -   MaxComputeSample: uses Spark with MaxCompute.
    -   MNSSample: uses Spark with Message Service \(MNS\).
    -   LoghubSample: uses Spark with LogHub.

## Dependencies

-   Test data in the data directory
    -   The\_Sorrows\_of\_Young\_Werther.txt: the input data file for MapReduce WordCount or SparkWordCount.
    -   patterns.txt: the word patterns to be ignored in the MapReduce WordCount job.
    -   u.data: the test table for sample.hive.
    -   abalone: the test data file for linear regression.
-   JAR package in the lib directory

    tutorial.jar: the JAR package required for sample.pig.


## Preparation

This project provides some test data. You can directly upload the test data to OSS for use. You can also prepare test data in services such as MaxCompute, MNS, Message Queue \(MQ\), and Log Service.

-   For more information about how to use Log Service, see [5-minute quick start](/intl.en-US/Quick Start/Quick start.md).

-   For more information about how to create MaxCompute projects and tables, see [Create a project](/intl.en-US/Prepare/Create a project.md) and [Create and view a table](/intl.en-US/Quick Start/Create and view a table.md).

-   For more information about how to use MQ, see [Quick Start](https://www.alibabacloud.com/help/doc-detail/34411.htm) of MQ.

-   For more information about how to use MNS, see the **"Overview"** section in **Quick-start** of MNS.


## Concepts

-   OSS URI: specifies the input or output data file for a job. It is similar to URLs like hdfs://. The OSS URI is in the oss://accessKeyId:accessKeySecret@bucket.endpoint/a/b/c.txt format.

-   AccessKey ID and AccessKey secret: the AccessKey for you to access Alibaba Cloud API. Click [Security Management](https://ak-console.aliyun.com/#/accesskey) to obtain the AccessKey.


## Run jobs in your E-MapReduce cluster

-   Spark

    -   SparkWordCount:

```
spark-submit --class SparkWordCount examples-1.0-SNAPSHOT-shaded.jar <inputPath>
                <outputPath> <numPartition>
```

        The following table describes parameters for submitting the SparkWordCount job.

        |Parameter|Description|
        |---------|-----------|
        |inputPath|The path of the input data file.|
        |outputPath|The path of the output data file.|
        |numPartition|The number of Resilient Distributed Dataset \(RDD\) partitions of the input data file.|

    -   SparkPi:

```
spark-submit --class SparkPi examples-1.0-SNAPSHOT-shaded.jar
```

    -   OSSSample:

```
spark-submit --class OSSSample examples-1.0-SNAPSHOT-shaded.jar <inputPath>
                <numPartition>
```

        The following table describes parameters for submitting the OSSSample job.

        |Parameter|Description|
        |---------|-----------|
        |inputPath|The path of the input data file.|
        |numPartition|The number of RDD partitions of the input data file.|

    -   ONSSample:

```
spark-submit --class ONSSample examples-1.0-SNAPSHOT-shaded.jar <accessKeyId>
                <accessKeySecret> <consumerId> <topic> <subExpression> <parallelism>
```

        The following table describes parameters for submitting the ONSSample job.

        |Parameter|Description|
        |---------|-----------|
        |accessKeyId|Your AccessKey ID.|
        |accessKeySecret|Your AccessKey secret.|
        |consumerId|The IDs of consumers. For more information, see [Terms](https://www.alibabacloud.com/help/zh/doc-detail/29533.htm). |
        |topic|The topic of message queues. Each message queue has a topic.|
        |subExpression|The tag for filtering messages. For more information, see [Message filtering](https://www.alibabacloud.com/help/zh/doc-detail/29543.htm). |
        |parallelism|The number of consumers that consume messages in the queue.|

    -   MaxComputeSample:

```
spark-submit --class MaxComputeSample examples-1.0-SNAPSHOT-shaded.jar <accessKeyId>
                <accessKeySecret> <envType> <project> <numPartitions>
```

        The following table describes parameters for submitting the MaxComputeSample job.

        |Parameter|Description|
        |---------|-----------|
        |accessKeyId|Your AccessKey ID.|
        |accessKeySecret|Your AccessKey secret.|
        |envType|The environment type. A value of 0 indicates the public network, and 1 indicates the internal network.

If the sample is run on a local server, set this parameter to 0. If the program is run in E-MapReduce, set this parameter to 1. |
        |project|The name of the project. For more information, see [MaxCompute glossary](https://help.aliyun.com/document_detail/odps/summary/glossary.html?spm=5176.docodps/quick_start/prerequisite.6.88.A5zVKu).|
        |numPartition|The number of RDD partitions of the input data file.|

    -   MNSSample:

```
spark-submit --class MNSSample examples-1.0-SNAPSHOT-shaded.jar <queueName>
                <accessKeyId> <accessKeySecret> <endpoint>
```

        The following table describes parameters for submitting the MNSSample job.

        |Parameter|Description|
        |---------|-----------|
        |queueName|The name of the queue. |
        |accessKeyId|Your AccessKey ID.|
        |accessKeySecret|Your AccessKey secret.|
        |endpoint|The endpoint used to access the queue.|

    -   LoghubSample:

```
spark-submit --class LoghubSample examples-1.0-SNAPSHOT-shaded.jar <sls project> <sls
                logstore> <loghub group name> <sls endpoint> <access key id> <access key secret> <batch
                interval seconds>
```

        The following table describes parameters for submitting the LoghubSample job.

        |Parameter|Description|
        |---------|-----------|
        |sls project:|The name of the project.|
        |sls logstore|The name of the Logstore.|
        |loghub group name|The name of the group that consumes Logstore data in the job. You can specify a name as needed. When the values of the sls project and sls store parameters are the same, jobs in the same group collaboratively consume data in the Logstore, and jobs in different groups separately consume data in the Logstore.|
        |sls endpoint|The endpoint used to access Log Service. For more information, see [Service endpoint](/intl.en-US/Developer Guide/API Reference/Endpoints.md).|
        |accessKeyId|Your AccessKey ID.|
        |accessKeySecret|Your AccessKey secret.|

    -   LinearRegression:

```
spark-submit --class LinearRegression examples-1.0-SNAPSHOT-shaded.jar <inputPath>
                <numPartitions>
```

        The following table describes parameters for submitting the LinearRegression job.

        |Parameter|Description|
        |---------|-----------|
        |inputPath|The path of the input data file.|
        |numPartition|The number of RDD partitions of the input data file.|

-   MapReduce

    WordCount:

    ```
    hadoop jar examples-1.0-SNAPSHOT-shaded.jar WordCount
                    -Dwordcount.case.sensitive=true <inputPath> <outputPath> -skip <patternPath>
    ```

    The following table describes parameters for submitting the MapReduce WordCount job.

    |Parameter|Description|
    |---------|-----------|
    |inputPath|The path of the input data file.|
    |outputPath|The path of the output data file.|
    |patternPath|The path of the file that contains the word patterns to be ignored. You can use the patterns.txt file in the data directory.|

-   Hive

    `hive -f sample.hive -hiveconf inputPath=<inputPath>`

    The following table describes parameters for submitting the sample.hive job.

    inputPath: the path of the input data file.

-   Pig

    ```
    pig -x mapreduce -f sample.pig -param tutorial=<tutorialJarPath> -param
                    input=<inputPath> -param result=<resultPath>
    ```

    The following table describes parameters for submitting the sample.pig job.

    |Parameter|Description|
    |---------|-----------|
    |tutorialJarPath|The JAR package. You can use the tutorial.jar package in the lib directory.|
    |inputPath|The path of the input data file.|
    |resultPath|The path of the output data file.|


**Note:**

-   To run jobs in E-MapReduce, you can upload the test data and the JAR package to OSS. Make sure that you set the storage path by following the OSS URI format.
-   Alternatively, you can store the test data and the JAR package in the E-MapReduce cluster.

## Run jobs on a local server

This section describes how to run Spark jobs on a local server to access data stored in Alibaba Cloud services, such as OSS. If you want to debug and run the jobs on a local server, we recommend that you use some development tools, such as IntelliJ IDEA or Eclipse, especially when you use Windows. Otherwise, you need to configure Hadoop and Spark runtime environments on Windows servers.

-   IntelliJ IDEA
    -   Preparation

        Install IntelliJ IDEA, Maven, Maven plugin for IntelliJ IDEA, Scala, and Scala plugin for IntelliJ IDEA.

    -   Procedure
        1.  In IntelliJ IDEA, find and double-click SparkWordCount.scala in the left-side project list to open it.

            ![Double-click SparkWordCount](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4668357851/p13087.jpg)

        2.  Go to the Run/Debug Configurations page for SparkWordCount.scala.

            ![Run/Debug Configurations page](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5668357851/p13088.jpg)

        3.  Click SparkWordCount in the application list and set required parameters on the right.

            ![Set parameters](../images/p13089.jpg)

        4.  Click **OK**.
        5.  Click the **Run** icon to run SparkWordCount.

            ![Run SparkWordCount](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5668357851/p13090.jpg)

        6.  View operational logs.

            ![View operational logs](../images/p13091.jpg)

-   Scala IDE for Eclipse
    -   Preparation

        Install the Scala IDE for Eclipse, Maven, and Maven plugin for Eclipse.

    -   Procedure
        1.  Import a Maven project as shown in the following figures.

            ![Import a project](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5668357851/p13093.jpg)

            ![Import a project 2](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5668357851/p13094.jpg)

            ![Import a project 3](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5668357851/p13095.jpg)

        2.  Build the Maven project. You can press Alt + Shift + X and then M to build the Maven project. Alternatively, you can right-click the project name and choose **Run As** \> **Maven build**.
        3.  After the project is built, right-click the project and select **Run Configuration** to go to the configuration page.
        4.  On the configuration page, choose Scala Application \> SparkWordCount$ and set the main class and required arguments on the right, as shown in the following figure.

            ![Set parameters](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6668357851/p13097.jpg)

        5.  Click **Run**.
        6.  View the output logs on the Console tab, as shown in the following figure.

            ![View the output logs on the Console tab](../images/p13098.jpg)


