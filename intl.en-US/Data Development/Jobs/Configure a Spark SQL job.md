# Configure a Spark SQL job {#concept_vck_cjp_y2b .concept}

This topic describes how to configure a Spark SQL job.

**Note:** Spark SQL uses yarn-client mode to submit jobs by default.

## Procedure {#section_ws2_gjp_y2b .section}

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://partners-intl.console.aliyun.com/#/emr) and go to the Cluster Management page.
2.  Click the Data Platform tab to go to the Projects page.
3.  Click **Workflows** in the Actions column of the project. Click **Edit Job** in the left-side navigation pane to go to the Edit Job page.
4.  Right-click the folder based on which you want to create a job and click **Create Job**.
5.  Enter a name and description for the job.
6.  Select SparkSQL from the Jop Type drop-down list. Spark SQL jobs are submitted in the background of EMR by using the following command.

    ```
    spark-sql [options] [cli option]spark-sql [options] -e {SQL_CONTENT}                    
    ```

    -   options: click Job Settings, click the Advanced Settings tab, and then click the plus \(+\) icon in the Environment Variables section to add the SPARK\_CLI\_PARAMS parameter. For example, set the value of SPARK\_CLI\_PARAMS to --executor-memory 1g --executor-cores.
    -   SQL\_CONTENT: the SQL statements that you enter in the job editor.
7.  Click **OK**.

    **Note:** You can also right-click a folder and create a subfolder, rename the folder, and delete the folder.

8.  In the **Contents** field, enter Spark SQL statements. For example:

    ``` {#codeblock_81y_h6g_sly}
    -- SQL statement example
    -- The size of SQL statements cannot exceed 64 KB.
    show databases;
    show tables;
    -- LIMIT 2000 is automatically used for the SELECT command.
    select * from test1;
    ```

9.  Click **Save** to complete the configuration of the Spark SQL job.

