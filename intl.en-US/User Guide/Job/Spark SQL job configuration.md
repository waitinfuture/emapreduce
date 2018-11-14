# Spark SQL job configuration {#concept_vck_cjp_y2b .concept}

In this tutorial, you will learn how to configure a Spark SQL job.

**Note:** By default, the Spark SQL mode to submit a job is Yarn mode.

## Procedure {#section_ws2_gjp_y2b .section}

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://emr.console.aliyun.com/?spm=5176.8250060.103.1.48466f55SEaqMe#/cn-hangzhou).
2.  At the top of the navigation bar, click **Data Platform**.
3.  In the **Actions** column, click **Design Workflow** of the specified project.
4.  On the left side of the Job Editing page, right-click on the folder you want to operate and select **New Job**.
5.  In the **New Job** dialog box, enter the job name and description.
6.  Click **OK**.

    **Note:** You can also create subfolder, rename folder, and delete folder by right-clicking on the folder.

7.  Select the Spark SQL job type to create a Spark SQL job. This type of job is submitted in the background by using the following method:

    ```
    spark-sql [options] [cli option]
    ```

8.  Enter the parameters in the **Content**box with parameters subsequent to Spark SQL commands.
    -   -e option

        Directly write running SQL for -e options by inputting it into the **Parameters** box of the job, for example:

        ```
        -e "show databases;"
        ```

    -   -f option

        -f options can be used to specify a Spark SQL script file. Loading well prepared Spark SQL script files on OSS can give more flexibility. We recommend that you use this operation mode, for example:

        ```
        -f ossref://your-bucket/your-spark-sql-script.sql
        ```

9.  Click **Save** to complete Spark SQL job configuration.

