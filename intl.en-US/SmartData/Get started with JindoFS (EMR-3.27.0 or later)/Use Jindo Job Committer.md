# Use Jindo Job Committer

This topic describes how to use Jindo Job Committer, which is also called JindoOssCommitter.

Job Committer is a basic component of distributed computing frameworks such as MapReduce and Spark. It is used to handle data consistency issues of write operations for distributed tasks.

Jindo Job Committer is an effective Job Committer developed by the Alibaba Cloud E-MapReduce \(EMR\) team. It is dedicated for job commits in the OSS scenario. Jindo Job Committer is combined with the multipart upload and file system customization features of OSS. If Jindo Job Committer is used, the output data of tasks is directly written in the final output directory without the need to perform rename operations. Before the job commit is completed, intermediate data is invisible to users. This ensures data consistency.

**Note:**

-   The OSS bandwidth and the enabling of some advanced features may affect the data copy performance of OSS. Therefore, the data copy performance for different users or buckets may vary. If you have any question, contact the technical support personnel of OSS.
-   After all tasks are completed, MapReduce Application Master or Spark Driver commits the job. This triggers a short time window in which only a part of the result files appear in the final output directory. The length of the time window is positively correlated with the number of files. You can set the `fs.oss.committer.threads` parameter to a larger value to speed up concurrent processing.
-   Hive and Presto jobs do not use Hadoop Job Committer.
-   In EMR clusters, Jindo Job Committer is enabled by default.

## Use Jindo Job Committer in MapReduce jobs

1.  Go to the **mapred-site** tab for the YARN service.

    1.  Log on to the [EMR console](https://emr.console.aliyun.com/).

    2.  In the top navigation bar, select the region where your cluster residesand select a resource group based on your business requirements.

    3.  Click the **Cluster Management** tab.

    4.  On the **Cluster Management** page, find your cluster and click **Details** in the Actions column.

    5.  In the left-side navigation pane, choose **Cluster Service** \> **YARN**.

    6.  Click the **Configure** tab.

    7.  In the **Service Configuration** section, click the **mapred-site** tab.

2.  Configure the Job Committer parameter based on your Hadoop version:

    -   Hadoop 2.X

        On the **mapred-site** tab, set the **mapreduce.outputcommitter.class** parameter to com.aliyun.emr.fs.oss.commit.JindoOssCommitter.

    -   Hadoop 3.X

        On the **mapred-site** tab, set the **mapreduce.outputcommitter.factory.scheme.oss** parameter to com.aliyun.emr.fs.oss.commit.JindoOssCommitterFactory.

3.  Save the configuration.

    1.  In the upper-right corner of the Service Configuration section, click **Save**.

    2.  In the **Confirm Changes** dialog box, specify Description and turn on **Auto-update Configuration**.

    3.  Click **OK**.

4.  Go to the **smartdata-site** tab for the SmartData service.

    1.  In the left-side navigation pane, choose **Cluster Service** \> **SmartData**.

    2.  Click the **Configure** tab.

    3.  In the **Service Configuration** section, click the **smartdata-site** tab.

5.  On the **smartdata-site** tab, set **fs.oss.committer.magic.enabled** to true.

6.  Save the configuration.

    1.  In the upper-right corner of the Service Configuration section, click **Save**.

    2.  In the **Confirm Changes** dialog box, specify Description and turn on **Auto-update Configuration**.

    3.  Click **OK**.


**Note:** After you set the **mapreduce.outputcommitter.class** parameter to com.aliyun.emr.fs.oss.commit.JindoOssCommitter, you can use the **fs.oss.committer.magic.enabled** parameter to determine which Job Committer is used. If you set this parameter to true, MapReduce jobs use Jindo Oss Magic Committer, which does not require rename operations. If you set this parameter to false, JindoOssCommitter functions the same as FileOutputCommitter.

## Use Jindo Job Committer in Spark jobs

1.  Go to the **spark-defaults** tab for the Spark service.

    1.  In the left-side navigation pane, choose **Cluster Service** \> **Spark**.

    2.  Click the **Configure** tab.

    3.  In the **Service Configuration** section, click the **spark-defaults** tab.

2.  On the **spark-defaults** tab, configure the parameters listed in the following table.

    |Parameter|Value|
    |---------|-----|
    |**spark.sql.sources.outputCommitterClass**|com.aliyun.emr.fs.oss.commit.JindoOssCommitter|
    |**spark.sql.parquet.output.committer.class**|com.aliyun.emr.fs.oss.commit.JindoOssCommitter|
    |**spark.sql.hive.outputCommitterClass**|com.aliyun.emr.fs.oss.commit.JindoOssCommitter|

    These parameters specify the Job Committers used to write data to Spark data source tables, Spark Parquet data source tables, and Hive tables.

3.  Save the configuration.

    1.  In the upper-right corner of the Service Configuration section, click **Save**.

    2.  In the **Confirm Changes** dialog box, specify Description and turn on **Auto-update Configuration**.

    3.  Click **OK**.

4.  Go to the **smartdata-site** tab for the SmartData service.

    1.  In the left-side navigation pane, choose **Cluster Service** \> **SmartData**.

    2.  Click the **Configure** tab.

    3.  In the **Service Configuration** section, click the **smartdata-site** tab.

5.  On the **smartdata-site** tab, set **fs.oss.committer.magic.enabled** to true.

    **Note:** You can use the `fs.oss.committer.magic.enabled` parameter to determine which Job Committer is used. If you set this parameter to true, Spark jobs use Jindo Oss Magic Committer, which does not require rename operations. If you set this parameter to false, JindoOssCommitter functions the same as FileOutputCommitter.

6.  Save the configuration.

    1.  In the upper-right corner of the Service Configuration section, click **Save**.

    2.  In the **Confirm Changes** dialog box, specify Description and turn on **Auto-update Configuration**.

    3.  Click **OK**.


## Optimize Jindo Job Committer

If your MapReduce or Spark tasks write a large number of files, you can adjust the number of threads that can be concurrently executed for job commit-related tasks to improve job commit performance.

1.  Go to the **smartdata-site** tab for the SmartData service.

    1.  In the left-side navigation pane, choose **Cluster Service** \> **SmartData**.

    2.  Click the **Configure** tab.

    3.  In the **Service Configuration** section, click the **smartdata-site** tab.

2.  On the **smartdata-site** tab, set the **fs.oss.committer.threads** parameter to 8.

    The default value is 8.

3.  Save the configuration.

    1.  In the upper-right corner of the Service Configuration section, click **Save**.

    2.  In the **Confirm Changes** dialog box, specify Description and turn on **Auto-update Configuration**.

    3.  Click **OK**.


