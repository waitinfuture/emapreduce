# Configure a Flink job {#task_1461222 .task}

This topic describes how to configure a Flink job.

-   You have created a project. For more information, see [Manage a workflow project](reseller.en-US/Data Development/Manage a workflow project.md#).
-   You have prepared the required resources and data to be processed, for example, JAR files, data files, and storage paths for these resources.

1.  Log on to the [E-MapReduce \(EMR\) console](https://partners-intl.console.aliyun.com/#/emr) by using an Alibaba Cloud account and open the Cluster Management tab.
2.  Select the Data Platform tab to view the Projects section.
3.  Click **Workflows** next to the target job. In the left-side navigation pane, select **Edit Job**.
4.  On the left side of the Edit Job tab, right-click a folder and select **Create Job**. 

    **Note:** You can also select Create Subfolder, Rename Folder, or Delete Folder from the list.

5.  In the Create Job dialog box, enter the **Name** and **Description**, and select **Flink** from the **Job Type** drop-down list.
6.  After the configuration is complete, click **OK** to create a new job.
7.  After you create a job, you need to specify content for the job. 

    ![Specify the job content](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1161712/156534057953974_en-US.png)

    The following is an example of **job content**.

    ``` {#codeblock_8fb_unr_l1z}
    run ossref://path/to/oss/of/WordCount.jar --input /path/to/some/text/data --output /path/to/result
    ```

    **Note:** If a job is stored in OSS as a JAR file, you can use the following endpoint as an example to reference the JAR file: ossref://<a directory name\>/.../<a file name\>.jar. You can click **OSS Path** to select the JAR file from OSS. The full path of the JAR file that includes the Spark Streaming job will be added to the job content. You must change the default OSS protocol to the ossref protocol.

    If you submit a Flink job in the EMR console, use a command with the following format.

    -   The following command is an example of how to submit a Flink job.

        ``` {#codeblock_7ml_fn3_tjh}
        flink run [options] <a file name>.jar args
        ```

    -   In this example, use the following command to submit a Flink job.

        ``` {#codeblock_n0a_i1g_v2w}
        flink run WordCount.jar --input /path/to/some/text/data --output /path/to/result
        ```

8.  After configuring the preceding parameters, click **Save** to finish the configuration of a Flink job.

