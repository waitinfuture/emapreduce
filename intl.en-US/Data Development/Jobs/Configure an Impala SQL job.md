# Configure an Impala SQL job

If you need to use Impala SQL during data development, you can configure an Impala SQL job in an E-MapReduce \(EMR\) cluster. This topic describes how to configure an Impala SQL job.

A project is created. For more information, see [Manage projects](/intl.en-US/Data Development/Manage projects.md).

## Procedure

1.  Log on to the [Alibaba Cloud EMR console](https://emr.console.aliyun.com) by using your Alibaba Cloud account.

2.  In the top navigation bar, select the region where your cluster residesand select a resource group based on your business requirements.

3.  Click the **Data Platform** tab.

4.  In the **Projects** section of the page that appears, find the project you want to edit and click **Edit Job** in the Actions column.

5.  In the **Edit Job** pane on the left, right-click the folder on which you want to perform operations and select **Create Job**.

6.  In the Create Job dialog box, specify **Name** and **Description**, and select **Impala SQL** from the **Job Type** drop-down list.

    You can use the following command syntax to submit an Impala SQL job:

    ```
    impala-shell -f {SQL_CONTENT} [options];
    ```

    Parameter description:

    -   `options`: the setting of the IMPALA\_CLI\_PARAMS parameter that you configure by performing the following operations: Click **Job Settings** in the upper-right corner of the job page. In the Job Settings pane, click the **Advanced Settings** tab. Click the ![add](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2982628951/p74998.png) icon in the **Environment Variables** section and add the IMPALA\_CLI\_PARAMS parameter. For example, set `IMAPAL_CLI_PARAMS` to "-u hive".
    -   `SQL_CONTENT`: the SQL statements that you enter.
7.  Click **OK**.

8.  Enter the Impala SQL statements in the **Content** field.

    Example:

    ```
    show databases;
    show tables;
    select * from test1;
    ```

9.  Click **Save**.


