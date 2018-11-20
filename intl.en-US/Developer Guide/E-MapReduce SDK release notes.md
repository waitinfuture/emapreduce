# E-MapReduce SDK release notes {#concept_dx5_gmt_hfb .concept}

E-MapReduce SDK release notes for each version.

**Note:** 

-   emr-core: Supports the interaction between data sources in OSS and Hadoop/Spark. It exists in the cluster by default. You do not need to package emr-code as part of a job.
-   emr-tablestore: Supports the interaction between data sources in Table Store and Hadoop/Hive/Spark. Package emr-tablestore in the JAR file.
-   emr-mns\_2.10/emr-mns\_2.11: Supports Spark to read data sources in MNS. Package emr-mns\_2.10/emr-mns\_2.11 in the JAR file.
-   emr-ons\_2.10/emr-ons\_2.11: Supports Spark to read data sources in Message Queue \(MQ\). Package emr-ons\_2.10/emr-ons\_2.11 in the JAR file.
-   emr-logservice\_2.10/emr-logservice\_2.11: Supports Spark to read data sources in Log Service. Package emr-logservice\_2.10/emr-logservice\_2.11 in the JAR file.
-   emr-maxcompute\_2.10/emr-maxcompute\_2.11: Supports Spark to read data sources in MaxCompute. Package emr-maxcompute\_2.10/emr-maxcompute\_2.11 in the JAR file.

```
<! --Supports interaction with data sources in OSS-->
    <dependency>
        <groupId>com.aliyun.emr</groupId>
        <artifactId>emr-core</artifactId>
        <version>1.4.1</version>
    </dependency>
    <! --Supports interaction with data sources in Table Store
-->
    <dependency>
        <groupId>com.aliyun.emr</groupId>
        <artifactId>emr-tablestore</artifactId>
        <version>1.4.1</version>
    </dependency>
    <! --Supports interaction with data sources in MNS, MQ, Log Service, and MaxCompute (in the Spark 1.x environment) -->
    <dependency>
        <groupId>com.aliyun.emr</groupId>
        <artifactId>emr-mns_2.10</artifactId>
        <version>1.4.1</version>
    </dependency>
    <dependency>
        <groupId>com.aliyun.emr</groupId>
        <artifactId>emr-logservice_2.10</artifactId>
        <version>1.4.1</version>
    </dependency>
    <dependency>
        <groupId>com.aliyun.emr</groupId>
        <artifactId>emr-maxcompute_2.10</artifactId>
        <version>1.4.1</version>
    </dependency>
    <dependency>
        <groupId>com.aliyun.emr</groupId>
        <artifactId>emr-ons_2.10</artifactId>
        <version>1.4.1</version>
    </dependency>
    <! --Supports interaction with data sources in MNS, MQ, Log Service, and MaxCompute (in the Spark 2.x environment)-->
    <dependency>
        <groupId>com.aliyun.emr</groupId>
        <artifactId>emr-mns_2.11</artifactId>
        <version>1.4.1</version>
    </dependency>
    <dependency>
        <groupId>com.aliyun.emr</groupId>
        <artifactId>emr-logservice_2.11</artifactId>
        <version>1.4.1</version>
    </dependency>
    <dependency>
        <groupId>com.aliyun.emr</groupId>
        <artifactId>emr-maxcompute_2.11</artifactId>
        <version>1.4.1</version>
    </dependency>
    <dependency>
        <groupId>com.aliyun.emr</groupId>
        <artifactId>emr-ons_2.11</artifactId>
        <version>1.4.1</version>
    </dependency>
```

-   v1.4.1
    -   MaxCompute: Fixes the problem of truncation for the DATETIME values.
    -   MaxCompute: Fixes the thread-safety problem for the SimpleDateFormat class.
-   v1.4.0
    -   MaxCompute: Adds implementation based on the DataSource class. Only versions 2.x and above of Spark are supported.
    -   Log Service: Adds implementation based on Direct API. Only versions 2.x and above of Spark are supported.
    -   OTS: Optimizes read and write operations.
    -   Fixes the bug that the access key of a user is replaced by the access key of a cluster app role when reading data sources in Log Service.
-   v1.3.2
    -   Fixes some bugs in Table Store.
-   v1.3.1
    -   Fixes the problem that NullPointerExceptions are thrown in some scenarios when Spark interacts with Log Service.
    -   E-MapReduce SDK supports the Spark 2.x environments since this version.
-   v1.3.0
    -   Supports Hadoop MapReduce, Spark, SparkSQL, and Hive to read data in Table Store.
    -   MNS and Log Service support the MetaService service provided by E-MapReduce. Based on the MetaService service, you can access data in MNS and Log Service without AccessKeys.
    -   Upgrades some dependencies.
-   v1.1.3.1

    SDK:

    -   Fixes the problem of dependency conflict between MNS and Spark/Hadoop packages.
    -   Fixes the problem that NullPointerExceptions are thrown in some scenarios when Spark Streaming interacts with MNS.
    -   Fixes some bugs for the Python SDK.
    -   Supports custom time and locations in the scenarios when Spark Streaming integrates with Loghub.
-   Core
    -   Fixes the problem that Hadoop does not support the Snappy native files. Currently, E-MapReduce supports processing the Snappy files that Log Service have archived to OSS.
    -   Fixes the problem that Spark does not support the Snappy files.
    -   Fixes the problem that OSS does not support the two algorithms of the OutputCommitter class in Apache Hadoop 2.7.2.
    -   Optimizes the performance of Hadoop/Spark reading and writing data in OSS.
    -   Fixes the problem that a Log4j exception is thrown when Spark prints a job.
-   v1.1.2
    -   Fixes the problem that the ConnectionClosedException is thrown when a job is reading data in OSS.
    -   Fixes the problem that some Hadoop commands are not available when accessing OSS data sources.
    -   Fixes the java.text.ParseException: Unparseable date problem.
    -   Optimizes the support of emr-core for local debugging.
    -   Interprets the "\_$folder$" files created in the earlier versions as directory paths instead of regular files.
    -   Adds a retry mechanism for Hadoop/Spark failing to read data in OSS.
-   v1.1.1
    -   Fixes the imbalance of disk usage caused by writing temporary files in OSS locally.
    -   Removes the $\_folder$ tag files created during OSS directory creation in a job execution.
-   v1.1.0
    -   Upgrades the LogHub SDK to 0.6.2. Abandons the Client DB mode and uses the Server DB mode instead.
    -   Upgrades the OSS SDK to 2.2.0. Fixes the run-time exceptions caused by the bugs of the OSS SDK.
    -   Adds support for MNS.
    -   Compatibility.
        -   For the versions 1.0.x SDKs.
            -   Interface:
                -   Compatible
            -   Namespace:
                -   Incompatible: Adjusts the package structure. Modifies the package name from com.aliyun to com.aliyun.emr.
    -   Modifies the groupId of the project from com.aliyun to com.aliyun.emr. The modified dependency in the POM file is as follows:

        ```
        <dependency>
              <groupId>com.aliyun.emr</groupId>
              <artifactId>emr-sdk_2.10</artifactId>
              <version>1.1.3.1</version>
          </dependency>
        ```

-   v1.0.5
    -   Optimizes the LoghubUtils interface and parameter input.
    -   Optimizes the output format of data in LogStore. Adds the topic and the source fields.
    -   Adds the parameter configurations for the time interval of pulling data in LogStore. Parameter name: spark.logservice.fetch.interval.millis. Default value: 200. Unit: milliseconds.
    -   Upgrades the ODPS SDK to 0.20.7-public.
-   v1.0.4
    -   Downgrades the dependency of Guava to 11.0.2 to avoid a conflict with the dependency of Guava in Hadoop.
    -   The MapReduce task supports files greater than 5 GB.
-   v1.0.3
    -   Adds configuration parameters related to the OSS Client.
-   v1.0.2
    -   Fixes the bug that the OSS URIs are resolved incorrectly.
-   v1.0.1
    -   Optimize the settings for OSS URIs.
    -   Adds support for MQ.
    -   Adds support for Log Service.
    -   Supports the Append Object feature of OSS.
    -   Supports uploading data in OSS using the multipart upload API.
    -   Supports copying data from OSS using the upload part copy mode.

