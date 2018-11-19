# Preparations {#concept_wnv_rvg_hfb .concept}

This topic describes how to prepare for Spark development.

## Install E-MapReduce SDK {#section_ys4_nwg_hfb .section}

You can install E-MapReduce SDK using either of the following methods:

-   Method 1: Directly use the JAR package in Eclipse. Follow these steps:
    1.  Download the [dependencies](http://mvnrepository.com/search?q=com.aliyun.emr) required by E-MapReduce from the Maven repository.

    2.  Copy the required JAR files to your project directory. \(Choose SDK version 2.10 or 2.11 based on the Spark version you use in the cluster in which your jobs run. Version 2.10 is recommended for Spark1.x, and 2.11 for Spark2.x.\)

    3.  Right-click the project name in Eclipse, and then choose **Properties** \> **Java Build Path** \> **Add JARs**.

    4.  Select the downloaded SDK.

    5.  After you have completed the preceding steps, you can read and write data in OSS, Log Service, MNS, ONS, Table Store, and MaxCompute.

-   Method 2: Edit the Maven project to add the dependencies as follows:

    ```
    <! -- Support for OSS data sources -->
        <dependency>
            <groupId>com.aliyun.emr</groupId>
            <artifactId>emr-core</artifactId>
            <version>1.4.1</version>
        </dependency>
        <! -- Support for Table Store data sources -->
        <dependency>
            <groupId>com.aliyun.emr</groupId>
            <artifactId>emr-tablestore</artifactId>
            <version>1.4.1</version>
        </dependency>
        <! -- Support for Message Service (MNS), Open Notification Service (ONS), Log Service, MaxCompute data sources (Spark 1.x environment) -->
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
        <! -- Support for MNS, ONS, Log Service, MaxCompute data sources (spark 2.x environment) -->
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


## Debug Spark code locally {#section_ugl_zwg_hfb .section}

The spark.hadoop.mapreduce.job.run-local property is for scenarios where you need to debug for Spark reading and writing OSS. Use the default settings in other scenarios.

If you need to debug for Spark reading and writing OSS, configure the SparkConf object by setting spark.hadoop.mapreduce.job.run-local to true. See the following example code:

```
val conf = new SparkConf().setAppName(getAppName).setMaster("local[4]")
   conf.set("spark.hadoop.fs.oss.impl", "com.aliyun.fs.oss.nat.NativeOssFileSystem")
   conf.set("spark.hadoop.mapreduce.job.run-local", "true")
   val sc = new SparkContext(conf) 
   val data = sc.textFile("oss://...")
   println(s"count: ${data.count()}")
```

## Third-party dependency description {#section_m1y_wyg_hfb .section}

To allow E-MapReduce to access Alibaba Cloud data sources \(such as OSS and MaxCompute\), you need to install the required third-party dependencies.

See the [pom](https://github.com/aliyun/aliyun-emapreduce-demo/blob/master/pom.xml) file to add or delete required third-party dependencies.

## OSS output directory settings {#section_axv_scv_vfb .section}

Configure the parameter fs.oss.buffer.dirs to the local hadoop configuration file. The parameter value is a local directory path. If the parameter value is not set, a null pointer exception will occur when OSS data is written during the process of running Spark.

## Clean up data {#section_rdp_yyg_hfb .section}

When a Spark job fails, the data generated is not deleted automatically. If a job fails, check the OSS output directory to see if any file exists. You must also check OSS fragment management for any fragments that have not been submitted. Remove fragments that are found.

## Use PySpark {#section_qg3_zyg_hfb .section}

If you use Python to create a Spark job, see the [instructions](https://github.com/aliyun/aliyun-emapreduce-sdk/blob/master/docs/how_to_run_spark_with_python_sdk.md) on configuring the environment.

