# Configure an Impala SQL job

If you need to use Impala SQL during data development, you can configure an Impala SQL job in an EMR cluster. This topic describes how to configure an Impala SQL job.

A project is created. For more information, see [Manage projects](/intl.en-US/Data Development/Manage a workflow project.md).

## Procedure

1.  Log on to the [E-MapReduce console](https://emr.console.aliyun.com/) by using an Alibaba Cloud account.

2.  In the top navigation bar, select the region where your cluster resides. Select the resource group as required. By default, all resources of the account appear.

3.  Click the Data Platform tab.

4.  In the **Projects** section, click **Edit Job** in the Actions column of the target project.

5.  Right-click the folder where you want to create a job, and select **Create Job**.

    **Note:** You can also right-click the folder to create a subfolder, rename the folder, or delete the folder.

6.  In the dialog box that appears, specify the **Name** and **Description** parameters, and select **Impala SQL** from the Job Type drop-down list.

    You can use the following command syntax to submit an Impala SQL job:

    ```
    impala-shell -f {SQL_CONTENT} [options];
    ```

    Parameter description:

    -   `options`: the setting of the IMPALA\_CLI\_PARAMS parameter that you configure by performing the following operations: Click **Job Settings** in the upper-right corner of the page. In the pane that appears, click the **Advanced Settings** tab. Click the ![add](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2982628951/p74998.png) icon in the **Environment Variables** section and add the IMPALA\_CLI\_PARAMS parameter.

        For example, set IMAPAL\_CLI\_PARAMS to "-u hive".

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


