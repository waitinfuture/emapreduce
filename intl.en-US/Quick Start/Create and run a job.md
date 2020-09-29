# Create and run a job

This topic describes how to create and run a Spark job.

## Create a project

1.  Log on to the [EMR console](https://emr.console.aliyun.com/).

2.  Click the **Data Platform** tab.

3.  On the **Data Platform** tab, click **Create Project** in the upper-right corner.

4.  In the **Create Project** dialog box, set **Project Name** to test and **Project Description** to test.

    ![create project](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5957731061/p134989.png)

5.  Click **Create**.


## Create a job

1.  In the **Projects** section, find the created project and click **Edit Job** in the Actions column.

    ![project list](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5957731061/p134994.png)

2.  In the **Edit Job** pane, right-click the folder under which you want to create a job and select **Create Job** from the shortcut menu.

    ![create job](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5957731061/p134992.png)

    **Note:** You can also right-click the folder to create a subfolder, rename the folder, or delete the folder.

3.  In the **Create Job** dialog box, configure the following parameters:

    1.  Set **Name** to Spark\_test.

    2.  Set **Description** to test.

    3.  Select **Spark** from the **Job Type** drop-down list.

    4.  Click **OK**.


## Configure and run a job

1.  Configure a job.

    1.  On the **Cluster Overview** page, view the Spark version.

        ![check spark](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5957731061/p135013.png)

    2.  Enter the following job content on the Data Platform tab:

        ```
        --class org.apache.spark.examples.SparkPi --master yarn-client --driver-memory 512m --num-executors 1 --executor-memory 1g --executor-cores 2 /usr/lib/spark-current/examples/jars/spark-examples_2.11-2.4.5.jar 10
        ```

        `2.4.5` in `/usr/lib/spark-current/examples/jars/spark-examples_2.11-2.4.5.jar` is the Spark version in your cluster.

2.  Click **Save** in the upper-right corner.

3.  Click **Run** in the upper-right corner.

4.  In the **Run Job** dialog box, click **OK**.


## View log data

You can click the **Records** tab to view the operational log data.

![log](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5957731061/p135017.png)

Click **Details** in the Action column that corresponds to your job to go to the Scheduling Center tab. On this tab, you can click the Job Instance Info, Log, or YARN Containers sub-tab to view relevant information.

