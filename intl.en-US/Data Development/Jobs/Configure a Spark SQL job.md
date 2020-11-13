# Configure a Spark SQL job

This topic describes how to configure a Spark SQL job.

A project is created. For more information, see [Manage projects](/intl.en-US/Data Development/Manage projects.md).

**Note:** By default, a Spark SQL job is submitted in yarn-client mode.

## Procedure

1.  Log on to the [Alibaba Cloud EMR console](https://emr.console.aliyun.com) by using your Alibaba Cloud account.

2.  In the top navigation bar, select the region where your cluster residesand select a resource group based on your business requirements.

3.  Click the **Data Platform** tab.

4.  In the **Projects** section of the page that appears, find the project you want to edit and click **Edit Job** in the Actions column.

5.  In the **Edit Job** pane on the left, right-click the folder on which you want to perform operations and select **Create Job**.

6.  In the Create Job dialog box, specify **Name** and **Description**, and select **SparkSQL** from the **Job Type** drop-down list.

    You can use the following command syntax to submit a Spark SQL job:

    ```
    spark-sql [options] [cli options] {SQL_CONTENT}                
    ```

    Parameter description:

    -   `options`: the setting of the SPARK\_CLI\_PARAMS parameter that you configure by performing the following operations: Click **Job Settings** in the upper-right corner of the job page. In the Job Settings panel, click the **Advanced Settings** tab. Click the ![add](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2982628951/p74998.png) icon in the **Environment Variables** section and add the setting of the SPARK\_CLI\_PARAMS parameter, for example, `SPARK_CLI_PARAMS="--executor-memory 1g --executor-cores`.
    -   `cli options`: For example, `-e <quoted-query-string>` indicates that the SQL statements within quotation marks are executed. `-f <filename>` indicates that the SQL statements in the file are executed.
    -   `SQL_CONTENT`: the SQL statements that you enter.
7.  Click **OK**.

8.  Enter the Spark SQL statements in the **Content** field.

    Example:

    ```
    -- SQL statement example
    -- The size of SQL statements cannot exceed 64 KB.
    show databases;
    show tables;
    -- LIMIT 2000 is automatically used for the SELECT statement.
    select * from test1;
    ```

9.  Click **Save**.


