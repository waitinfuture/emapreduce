# Use E-MapReduce to process offline jobs {#concept_bzz_zg1_dfb .concept}

This section describes how to use E-MapReduce to read data from OSS, and a set of offline data processing operations, such as data collection and data clean-up.

## Overview {#section_qlt_kh1_dfb .section}

E-MapReduce clusters can be used in various scenarios. E-MapReduce supports all the scenarios that the Hadoop ecosystem and Spark support. E-MapReduce is based on Hadoop and Spark clusters. You can use Alibaba Cloud ECS instances hosted by E-MapReduce clusters in the same way as you would on your physical machines.

Two popular kinds of big data processing that we use today are offline and online data processing.

-   Offline data processing: You only want to obtain the analytical results of data without a major concern about the time it takes. For example, in a batch data processing scenario, you receive data from OSS and output processing results to OSS, using MapReduce, Hive, Pig, and Spark.
-   Online data processing: You want to obtain the analytical results of data with a strict requirement on the time it takes, such as real-time streaming data processing. Deeply integrated with Spark MLlib, GrapX, and SQL, Spark Streaming can be used to process streaming messages.

This section describes how to run an offline job called word count in E-MapReduce.

## Process {#section_km4_qh1_dfb .section}

OSS -\> EMR -\> Hadoop MapReduce

This process includes two steps:

1.  Store data to OSS.
2.  Read data from OSS and analyze the data by using E-MapReduce.

## Prerequisites {#section_krn_vh1_dfb .section}

-   The following steps are performed in a Windows system. You need to ensure that Maven and Java have been installed and configured properly into your system.
-   You can use E-MapReduce to automatically create a Hadoop cluster. For more information, see [Create a cluster](../../../../intl.en-US/Quick Start/Create E-MapReduce/Create a cluster.md#).
    -   EMR Version: EMR-3.12.1
    -   Cluster Type: HADOOP
    -   Software: HDFS2.7.2, YARN2.7.2, Hive2.3.3, Ganglia3.7.2, Spark2.3.1, HUE4.1.0, Zeppelin0.8.0, Tez0.9.1, Sqoop1.4.7, Pig0.14.0, ApacheDS2.0.0, and Knox0.13.0
    -   The network type of this Hadoop cluster is VPC in the China \(Hangzhou\) region. The master instance group is configured with a public IP and an internal network IP. The high availability mode is set to No \(a non-HA mode\). The following figure shows the details.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21330/154209380711874_en-US.png)


## Procedures {#section_f2w_y31_dfb .section}

1.  Download sample code to your local disk.

    Open git bash in your system and execute the clone command as follows.

    ```
    git clone https://github.com/aliyun/aliyun-emapreduce-demo.git
    ```

    Execute the `mvn install` command to compile the code.

2.  For more information about how to create a bucket, see [Create a bucket](../../../../intl.en-US/Quick Start/Create a bucket.md#).

    **Note:** You must create a bucket and an E-MapReduce cluster in the same region.

3.  Upload jar packages and resource files
    1.  Log on to the [OSS console](https://oss.console.aliyun.com) and click the **Files** tab.
    2.  Click **Upload** to upload resources files in the aliyun-emapreduce-demo/resources directory and jar packages in the aliyun-emapreduce-demo/target directory.
4.  Create a workflow project

    For more information, see [Workflow project management](../DNemapreduce1876943/EN-US_TP_17961.dita#concept_rqw_qz2_z2b).

5.  Create a job

    For more information, see [Edit jobs](../DNemapreduce1876943/EN-US_TP_17962.dita#concept_iny_t1f_z2b). Take a MapReduce job as an example.

6.  After you configure a job, click **Run**. The following figure shows the details.

    -   For more information about how to use OSS, see [OSS usage instructions](https://www.alibabacloud.com/help/zh/doc-detail/42799.html?spm=a2c5t.11065259.1996646101.searchclickresult.63fc71f3FgiO9g).
    -   For more information about how to configure jobs, see the [Hadoop MapReduce job configuration](https://help.aliyun.com/document_detail/28095.html) section of the E-MapReduce User Guide.
    **Note:** 

    -   If the OSS output URI already exists, an error occurs when you execute a job.
    -   When you click the Insert an OSS UNI button and select OSSREF as a File Prefix, E-MapReduce downloads OSS files to your cluster and add these files to a specified classpath.
    -   Currently, only OSS Standard storage is supported for all operations.

## View logs {#section_drj_5k1_dfb .section}

For more information about how to view logs of an execution plan, see [Connect to a cluster using SSH](https://help.aliyun.com/document_detail/28187.html?spm=a2c4g.11186623.6.640.24b454c4CAFUqC).

