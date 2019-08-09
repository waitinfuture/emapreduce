# Configure a Spark Streaming job {#task_1461465 .task}

This topic describes how to configure a Spark Streaming job.

-   You have created a project. For more information, see [Manage a workflow project](reseller.en-US/Data Development/Manage a workflow project.md#).
-   You have prepared all required resources and data to be processed.

1.  Log on to the [E-MapReduce \(EMR\) console](https://partners-intl.console.aliyun.com/#/emr) by using an Alibaba Cloud account and open the Cluster Management tab.
2.  Select the Data Platform tab in the top navigation bar and view the Projects section.
3.  Click **Workflows** next to the target project. In the left-side navigation pane, select **Edit Job**.
4.  On the left side of the Edit Job tab, right-click a folder and select **Create Job**. 

    **Note:** You can also select Create Subfolder, Rename Folder, or Delete Folder from the list.

5.  In the Create Job dialog box, enter the **Name** and **Description**, and select **Spark Streaming** from the **Job Type** drop-down list.
6.  After the configuration is complete, click **OK** to create a new job.
7.  After creating a job, you must configure the job. 

    Take a job named **SlsStreaming** as an example. The **job content** is shown as follows.

    ``` {#codeblock_5p0_060_yf6}
    --master yarn-client --driver-memory 7G --executor-memory 5G --executor-cores 1 --num-executors 32 --class com.aliyun.emr.checklist.benchmark.SlsStreaming emr-checklist_2.10-0.1.0.jar <project> <logstore> <accessKey> <secretKey>
    ```

    **Note:** If a job is stored in OSS as a JAR file, you can use the following endpoint as an example to reference the JAR file: ossref://<a directory name\>/.../<a file name\>.jar. You can also click **OSS Path** to select the JAR file from OSS. The full path of the JAR file that includes the Spark Streaming job will be added to the job content. You must change the default OSS protocol to the ossref protocol.

    If you submit a Spark Streaming job in the EMR console, use a command with the following format.

    -   The following command is an example of how to submit a Spark Streaming job.

        ``` {#codeblock_etj_wh6_96r}
        spark-submit [options] --class [MainClass] <a file name>.jar args
        ```

    -   In this example, use the following command to submit a Spark Streaming job.

        ``` {#codeblock_iyj_h95_4x0}
        spark-submit --master yarn-client --driver-memory 7G --executor-memory 5G --executor-cores 1 --num-executors 32 --class com.aliyun.emr.checklist.benchmark.SlsStreaming emr-checklist_2.10-0.1.0.jar <project> <logstore> <accessKey> <secretKey>
        ```

8.  After configuring the preceding parameters, click **Save** to finish the configuration of a Spark Streaming job.

