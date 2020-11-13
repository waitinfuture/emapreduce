# Configure a Hive SQL job

This topic describes how to configure a Hive SQL job.

A project is created. For more information, see [Manage projects](/intl.en-US/Data Development/Manage projects.md).

## Procedure

1.  Log on to the [Alibaba Cloud EMR console](https://emr.console.aliyun.com) by using your Alibaba Cloud account.

2.  In the top navigation bar, select the region where your cluster residesand select a resource group based on your business requirements.

3.  Click the **Data Platform** tab.

4.  In the **Projects** section of the page that appears, find the project you want to edit and click **Edit Job** in the Actions column.

5.  In the **Edit Job** pane on the left, right-click the folder on which you want to perform operations and select **Create Job**.

6.  In the Create Job dialog box, specify **Name** and **Description**, and select **HiveSQL** from the **Job Type** drop-down list.

    This option indicates that a Hive SQL job will be created. You can use the following command syntax to submit a Hive SQL job:

    ```
    hive -e {SQL CONTENT}
    ```

    `SQL CONTENT` refers to the SQL statements that you enter in the job editor.

    ![HiveSQL](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1982628951/p46634.png)

7.  Click **OK**.

8.  Enter Hive SQL statements in the **Content** field.

    ```
    -- SQL statement example
    -- The size of SQL statements cannot exceed 64 KB.
    show databases;
    show tables;
    -- LIMIT 2000 is automatically used for the SELECT statement.
    select * from test1;
    ```

    ![HiveSQL_content](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1982628951/p46635.png)

9.  Click **Save**.


