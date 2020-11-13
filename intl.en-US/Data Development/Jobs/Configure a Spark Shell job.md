# Configure a Spark Shell job

This topic describes how to configure a Spark Shell job.

A project is created. For more information, see [Manage projects](/intl.en-US/Data Development/Manage projects.md).

## Procedure

1.  Log on to the [Alibaba Cloud EMR console](https://emr.console.aliyun.com) by using your Alibaba Cloud account.

2.  In the top navigation bar, select the region where your cluster residesand select a resource group based on your business requirements.

3.  Click the **Data Platform** tab.

4.  In the **Projects** section of the page that appears, find the project you want to edit and click **Edit Job** in the Actions column.

5.  In the **Edit Job** pane on the left, right-click the folder on which you want to perform operations and select **Create Job**.

6.  In the Create Job dialog box, specify **Name** and **Description**, and select **Spark Shell** from the **Job Type** drop-down list.

7.  Click **OK**.

8.  Specify the command line parameters that follow the Spark Shell command in the **Content** field.

    Example:

    ```
    val count = sc.parallelize(1 to 100).filter { _ =>
      val x = math.random
      val y = math.random
      x*x + y*y < 1
    }.count()println(s"Pi is roughly ${4.0 * count / 100}")
    ```

9.  Click **Save**.


