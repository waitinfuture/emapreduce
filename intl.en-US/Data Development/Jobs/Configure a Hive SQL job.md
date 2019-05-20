# Configure a Hive SQL job {#task_261440 .task}

This topic describes how to configure a Hive SQL job.

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://emr.console.aliyun.com/) by using an Alibaba Cloud account.
2.  Click the Data Platform tab to go to the Projects page.
3.  Click **Workflows** in the Actions column of the project. Click **Edit Jobs** in the left-side navigation pane to go to the Edit Jobs page.
4.  Right-click the folder based on which you want to create a job and click **Create Job**.
5.  Enter a name and description. Select HiveSQL from the Job Type drop-down list. Hive SQL jobs are submitted by using the following command in the background of EMR. 

    ``` {#codeblock_uyi_ftd_qo1}
    hive -e {SQL CONTENT}
    ```

    SQL CONTENT refers to the SQL statements that you enter in the job editor.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/215990/155831828546634_en-US.png)

6.  Click **OK**. 

    **Note:** You can also right-click the folder to create a subfolder, rename the folder, and delete the folder.

7.  In the **Content** field, enter Hive SQL statements. For example: 

    ``` {#codeblock_4o1_7s1_rc7}
    -- SQL statement example
    -- The size of SQL statements cannot exceed 64 KB.
    show databases;
    show tables;
    -- LIMIT 2000 is automatically used for the SELECT command.
    select * from test1;
    ```

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/215990/155831828646635_en-US.png)

8.  Click **Save** to complete the configuration.

