# Use Jindo Job Committer

This topic describes how to use Jindo Job Committer, which is also called JindoOssCommitter.

Job Committer is a basic component of distributed computing frameworks such as MapReduce and Spark. It is used to handle data consistency issues of write operations for distributed tasks. MapReduce and Spark jobs use FileOutputCommitter by default. When FileOutputCommitter is used, each task writes data to a temporary directory for the task. After a task is completed, the file is moved to a temporary directory for the job. After all tasks are completed, MapReduce Application Master or Spark Driver moves the files to the final output directory. The files are moved based on rename operations. In HDFS, rename operations are cost-effective and secure. However, in Object Storage Service \(OSS\), to complete a rename operation, the system needs to copy and then delete data. As a result, the time taken to commit a job increases linearly with the output data volume for a distributed task. Therefore, FileOutputCommitter is not recommended when large volumes of data are stored in OSS.

Jindo Job Committer is an effective Job Committer developed by the Alibaba Cloud EMR team. It is dedicated for job commits in the OSS scenario. Jindo Job Committer is combined with the multipart upload and file system customization features of OSS. If Jindo Job Committer is used, the output data of tasks is directly written in the final output directory without the need to perform rename operations. Before the job commit is completed, intermediate data is invisible to users. This ensures data consistency.

**Note:**

-   The OSS bandwidth and the enabling of some advanced features may affect the data copy performance of OSS. Therefore, the data copy performance for different users or buckets may vary. If you have any question, contact the technical support personnel of OSS.
-   After all tasks are completed, MapReduce Application Master or Spark Driver commits the job. If a node fails or some other issues occur during the commit process, a task may fail. As a result, data consistency cannot be ensured. This triggers a short time window in which only a part of the result files appear in the final output directory. The length of the time window is positively correlated with the number of files. You can set `fs.oss.committer.threads` to a larger value to speed up concurrent processing.
-   Hive and Presto jobs use data consistency mechanisms that are independent of Hadoop Job Committer. As a result, you cannot use Jindo Job Committer to optimize Hive and Presto job commits.
-   In EMR clusters, Jindo Job Committer is enabled by default.

## Use Jindo Job Committer in MapReduce jobs

1.  Go to the **mapred-site** tab for the YARN service.

    1.  Log on to the [EMR console](https://emr.console.aliyun.com/).

    2.  In the top navigation bar, select the region where your cluster resides. Select the resource group as required. By default, all resources of the account appear.

    3.  Click the **Cluster Management** tab.

    4.  On the **Cluster Management** page that appears, find the target cluster and click **Details** in the Actions column.

    5.  In the left-side navigation pane, click **Cluster Service** and then **YARN**.

    6.  Click the **Configure** tab.

    7.  In the **Service Configuration** section, click the **mapred-site** tab.

2.  Configure the Job Committer parameter based on your Hadoop version:

    -   Hadoop 2.x

        On the **mapred-site** tab, set **mapreduce.outputcommitter.class** to **com.aliyun.emr.fs.oss.commit.JindoOssCommitter**.

    -   Hadoop 3.x

        On the **mapred-site** tab, set **mapreduce.outputcommitter.factory.scheme.oss** to **com.aliyun.emr.fs.oss.commit.JindoOssCommitterFactory**.

3.  Save the configuration.

    1.  In the upper-right corner of the Service Configuration section, click **Save**.

    2.  In the **Confirm Changes** dialog box, specify Description and turn on **Auto-update Configuration**.

    3.  Click **OK**.

4.  Go to the **smartdata-site** tab for the SmartData service.

    1.  In the left-side navigation pane, click **Cluster Service** and then **SmartData**.

    2.  Click the **Configure** tab.

    3.  In the **Service Configuration** section, click the **smartdata-site** tab.

5.  On the **smartdata-site** tab, set **fs.oss.committer.magic.enabled** to **true**.

6.  Save the configuration.

    1.  In the upper-right corner of the Service Configuration section, click **Save**.

    2.  In the **Confirm Changes** dialog box, specify Description and turn on **Auto-update Configuration**.

    3.  Click **OK**.


**Note:** After you set **mapreduce.outputcommitter.class** to **com.aliyun.emr.fs.oss.commit.JindoOssCommitter**, you can use the **fs.oss.committer.magic.enabled** parameter to determine which Job Committer is used. If you set this parameter to true, MapReduce jobs use Jindo Oss Magic Committer, which does not require rename operations. If you set this parameter to false, JindoOssCommitter functions the same as FileOutputCommitter.

## Use Jindo Job Committer in Spark jobs

1.  Go to the **spark-defaults** tab for the Spark service.

    1.  In the left-side navigation pane, click **Cluster Service** and then **Spark**.

    2.  Click the **Configure** tab.

    3.  In the **Service Configuration** section, click the **spark-defaults** tab.

2.  On the **spark-defaults** tab, configure the parameters listed in the following table.

    |Parameter|Description|
    |---------|-----------|
    |**spark.sql.sources.outputCommitterClass**|com.aliyun.emr.fs.oss.commit.JindoOssCommitter|
    |**spark.sql.parquet.output.committer.class**|com.aliyun.emr.fs.oss.commit.JindoOssCommitter|
    |**spark.sql.hive.outputCommitterClass**|com.aliyun.emr.fs.oss.commit.JindoOssCommitter|

    These parameters specify the Job Committers used to write data to Spark data source tables, Spark Parquet data source tables, and Hive tables. Configure these parameters based on your business requirements.

3.  Save the configuration.

    1.  In the upper-right corner of the Service Configuration section, click **Save**.

    2.  In the **Confirm Changes** dialog box, specify Description and turn on **Auto-update Configuration**.

    3.  Click **OK**.

4.  Go to the **smartdata-site** tab for the SmartData service.

    1.  In the left-side navigation pane, click **Cluster Service** and then **SmartData**.

    2.  Click the **Configure** tab.

    3.  In the **Service Configuration** section, click the **smartdata-site** tab.

5.  On the **smartdata-site** tab, set **fs.oss.committer.magic.enabled** to **true**.

    **Note:** You can use the `fs.oss.committer.magic.enabled` parameter to determine which Job Committer is used. If you set this parameter to true, Spark jobs use Jindo Oss Magic Committer, which does not require rename operations. If you set this parameter to false, JindoOssCommitter functions the same as FileOutputCommitter.

6.  Save the configuration.

    1.  In the upper-right corner of the Service Configuration section, click **Save**.

    2.  In the **Confirm Changes** dialog box, specify Description and turn on **Auto-update Configuration**.

    3.  Click **OK**.


## Optimize Jindo Job Committer

If your MapReduce or Spark tasks write a large number of files, you can adjust the number of threads that can be concurrently executed for job commit-related tasks to improve job commit performance.

1.  Go to the **smartdata-site** tab for the SmartData service.

    1.  In the left-side navigation pane, click **Cluster Service** and then **SmartData**.

    2.  Click the **Configure** tab.

    3.  In the **Service Configuration** section, click the **smartdata-site** tab.

2.  On the **smartdata-site** tab, set **fs.oss.committer.threads** to **8**.

    The default value is 8.

3.  Save the configuration.

    1.  In the upper-right corner of the Service Configuration section, click **Save**.

    2.  In the **Confirm Changes** dialog box, specify Description and turn on **Auto-update Configuration**.

    3.  Click **OK**.


