# E-MapReduce Quick Start {#concept_jkh_sb4_y2b .concept}

This section describes clusters and jobs in E-MapReduce \(EMR\) and how to use them. For example, you can create a Spark job, run the job on the cluster to calculate Pi \(π\), and then view the result in the console.

**Note:** Note: Confirm that all [prerequisites](intl.en-US/Quick Start/Prerequisites.md#) have been met.

1.  Create a cluster
    1.  In the [Alibaba Cloud E-MapReduce console](https://emr.console.aliyun.com/), click the Cluster tab to go to the clusters list page. Click **Create Cluster** in the upper-right corner.
    2.  Software Configuration
        1.  Select the latest EMR version. Example: **EMR 3.13.0**.
        2.  Select the default software configuration.
    3.  Hardware Configuration
        1.  Select **Pay-As-You-Go**.
        2.  If no security group has been created, enter a name and then create one.
        3.  Select a master instance with 4 cores and 8 GB of memory.
        4.  Select two core instances with 4 cores and 8 GB of memory.
        5.  The remaining configuration all uses the default settings.
    4.  Basic Configuration
        1.  Enter the name of the cluster.
        2.  Specify a path to store the job log. Make sure that the running log feature is enabled. [Create an OSS bucket](../../../../../intl.en-US/Quick Start/Create a bucket.md#) in the region where the cluster has been created.
        3.  Enter the password that is used to log on to the cluster.
    5.  Click OK to create the cluster.
2.  Create a job.
    1.  Click the Data Platform tab to go to the project list page. Click **New Project** in the upper-right corner.
    2.  In the New Project dialog box, enter the project name and description, and then click **Create**.
    3.  Click **Design Workflow** to the right of the specified project and to go to the **Edit Jobs** page.
    4.  On the left side of the Edit Jobs page, right-click the folder that you want to operate and select**New Job**.
    5.  Enter the job name and description.
    6.  Select Spark as the job type.
    7.  Click **OK**.

        **Note:** You can also right-click a folder and then choose to create a subfolder, rename the folder, or delete the folder.

    8.  Enter parameters as follows:

        ```
        --class org.apache.spark.examples.SparkPi --master yarn-client --driver-memory 512m --num-executors 1 --executor-memory 1g --executor-cores 2 /usr/lib/spark-current/examples/jars/spark-examples_2.11-2.1.1.jar 10
        ```

        **Note:** The `/usr/lib/spark-current/examples/jars/spark-examples_2.11-2.1.1.jar` JAR file name is defined by the version of Spark in the cluster. For example, if the version of Spark is 2.1.1, then the JAR file is named as `spark-examples_2.11-2.1.1.jar`. If the version of Spark is 2.2.0, then the JAR file is named as `spark-examples_2.11-2.2.0.jar`.

    9.  Click **Run**.
3.  View the job log entries and confirm the results.

    After you have executed a job, you can click the Log tab at the bottom of the page to view running log. Click **View Details** to go to the details page. On this page, you can view details, including the job submission log and YARN Container log.


