# Sqoop job configuration {#concept_dlq_ckp_y2b .concept}

In this tutorial, you will learn how to configure a Sqoop job.

**Note:** Only E-MapReduce products version V1.3.0 and higher support the Sqoop job type. Running a Sqoop job on lower cluster versions will fail, and errlog will report “Not supported” errors. For parameter details, refer to [Data Transmission Sqoop](https://www.alibabacloud.com/help/doc-detail/28133.htm?spm=a2c63.p38356.a3.3.246051barsyWuU#concept-p22-qkp-y2b).

## Procedure {#section_kzj_3kp_y2b .section}

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://emr.console.aliyun.com/?spm=5176.8250060.103.1.48466f55SEaqMe#/cn-hangzhou).
2.  At the top of the navigation bar, click **Data Platform**.
3.  In the **Actions** column, click **Design Workflow** of the specified project.
4.  On the left side of the Job Editing page, right-click on the folder you want to operate and select **New Job**.
5.  In the **New Job** dialog box, enter the job name and description.
6.  Select the Sqoop job type to create a Sqoop job. In E-MapReduce back-end, Sqoop job will submit through the following process:

    ```
    sqoop [args]
    ```

7.  Click **OK**.

    **Note:** You can also create subfolder, rename folder, and delete folder by right-clicking on the folder.

8.  Complete the **Content** box with parameters subsequent to Sqoop commands.
9.  Click **Save** to complete Sqoop job definition.

