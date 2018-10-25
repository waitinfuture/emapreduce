# Quick start {#concept_jkh_sb4_y2b .concept}

In this tutorial, you will learn how clusters, jobs, and execution plans are used in E-MapReduce. You will also learn how to create a Spark Pi job, run it successfully in the cluster, and check the results.

**Note:** Make sure you have completed the necessary [prerequisites](intl.en-US/Quick Start/Prerequisites.md#).

1.  Create a cluster.
    1.  At the top of the [EMR console](https://emr.console.aliyun.com/), click **Cluster Management** \> **Create Cluster**.
    2.  Software configurations.
        1.  Use the latest EMR product version, such as **EMR-3.13.0**.
        2.  Use default software configurations.
    3.  Hardware configurations.
        1.  Select **Pay-as-You-Go**.
        2.  If there is no security group, enter a name to create new security group.
        3.  Select **4 vCPU 8G** for the **Master Instance Type**.
        4.  Select **4 vCPU 8G** for the **Core Instance Type** \(one instance\).
        5.  Keep others in the default status.
    4.  Basic configurations.
        1.  Enter the name of the cluster.
        2.  Select a log path to save job logs and make sure that the **Running Logs** button is turned on. In the cluster related region, [create an OSS bucket](../../../../intl.en-US/Quick Start/Create a bucket.md#).
        3.  Enter the password.
        4.  Click **Next**.
    5.  Click **Create** to create a cluster.
2.  Create a job.
    1.  At the top of the page, click the **Data Platform** tab.
    2.  In the upper right corner of the**Projects** page, click **New Project**.
    3.  In the **New Project** panel, enter the project name and project description, and click **Create**.
    4.  At the right side of the corresponding project, click **Design Workflow**.
    5.  In the **New Job** dialog box, enter the job name, job description, and select Spark as the job type.

        Once the job type is selected, it cannot be modified.

    6.  Click **OK**.

        **Note:** You can also create subfolders, rename folders, and delete folders by right-clicking on the folders.

    7.  In the**Content** box, enter parameters as follows.

        ```
        --class org.apache.spark.examples.SparkPi --master yarn-client --driver-memory 512m --num-executors 1 --executor-memory 1g --executor-cores 2 /usr/lib/spark-current/examples/jars/spark-examples_2.11-2.1.1.jar 10
        ```

        **Note:** The `/usr/lib/spark-current/examples/jars/spark-examples_2.11-2.1.1.jar` jar file name is decided by the Spark version in the cluster, for example, if Spark version is 2.1.1, it should be `spark-examples_2.11-2.1.1.jar`, if Spark version is 2.2.0, then file name is `spark-examples_2.11-2.2.0.jar`.

    8.  Click **Run**.
3.  View job logs and confirm the results.

    After the job runs, at the bottom of the page, in the**Running Records** tab, you can view running logs of the job. Click **Logs** to jump to the detailed log page of the job, you can see information in **Job Intance**, **Runtime Log**, and **YARN Containers**.


