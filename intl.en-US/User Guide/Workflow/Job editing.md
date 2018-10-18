# Job editing {#concept_iny_t1f_z2b .concept}

## Create a Job {#section_ibf_v1f_z2b .section}

In the project, you can create jobs such as Shell, Hive, Spark, SparkSQL, MapReduce、Sqoop, Pig and Spark Streaming.

1.  Log on to the [Alibaba Cloud E-MapReduce Console](https://emr.console.aliyun.com/?spm=5176.8250060.103.1.48466f55SEaqMe#/cn-hangzhou) to enter the Cluster List page with primary account.
2.  Click the **Data Platform** tab on the top to enter the **Project List** page.
3.  Click **Design Workflow** of the specified project in the **Operation** column.
4.  On the left side of the Job Editing page, right-click on the folder you want to operate and select **New Job**.
5.  In the **New Job** dialog box, enter the job name, job description, and select the job type.

    Once the job type is selected, it cannot be modified.

6.  Click **OK**.

    **Note:** You can also create subfolder, rename folder, and delete folder by right-clicking on the folder.


## Develop jobs {#section_zgm_x1f_z2b .section}

For the development guides for various types of jobs, see [Job](intl.en-US/User Guide/Job/Hadoop MapReduce Job Configuration.md#) section in the E-MapReduce User Guide.

**Note:** When you insert the OSS path, if you select the OSSREF file prefix, the OSS file will be downloaded to the cluster and added to the classpath.

-   Basic settings

    Click the **Job Configuration** in the upper right corner of the page to enter the **Job Running Configuration** page.

    -   Failed Retries: Sets the number of retries when this job fails during the workflow running. This option will not take effect when you run the job directly on the **Job Editing** page.
    -   Failure Policy: Sets whether to continue running the next job or suspend the current workflow when this job fails during the workflow running.
    -   Add Running Resources: If you add resources such as Jar packages or UDFs that the job running depends on, you need to upload the resources to OSS first. After adding the resource, you can reference the resource directly in the job code.
    -   Configuration Parameter: Specify the value of the variable referenced in the job code. You can reference variables in the code with the format $\{variable name\}. Click the plus icon on the right to add key and value, the key is the variable name, and the value is the value of the variable. In addition, you can customize the time variable according to the schedule time. The rules are as follows:

        -   yyyy indicates the year which is 4-digit.
        -   MM indicates the month.
        -   dd indicates the day.
        -   hh24 indicates the hour, uses \*hh\* if 12-hour system is adopted.
        -   mm indicates the minute.
        -   ss indicates the second.

            The time variable can be any combination of time containing yyyy. You can also use the '+' symbol to advance time and use '-' symbol to delay time. For example, the variable $\{yyyy-MM-dd\} indicates the current date, then:

            -   The representation of 1 year later: $\{yyyy+Ny\} or $\{yyyy-MM-dd hh:mm:ss+1y\}.
            -   The representation of 3 months later: $\{yyyyMM+Nm\} or $\{hh:mm:ss yyyy-MM-dd+3m\}.
            -   The representation of 5 days before: $\{yyyyMMdd-Nd\} or $\{hh:mm:ss yyyy-MM-dd-5d\}.
-   Advanced settings

    On the **Job Running Configuration** page, click the **Advanced Settings** tab.

    -   Mode: Job running modes, including YARN and LOCAL. In YARN mode, the job is submitted on the YARN by the Launcher. In LOCAL mode, jobs run directly on the assigned host.
    -   Environment variables: Add environment variables for job running, or export environment variables directly in the job script.
    -   Scheduling Parameters: Sets job running configuration such as the YARN queue, CPU, memory and Hadoop users. You can leave it unset, and the default value of the Hadoop cluster is adopted when the job is running.

## Run job {#section_mmj_2bf_z2b .section}

Once the job has been developed and configured, you can click the **Run** button at the top right corner to run the job.

## View log {#section_ajt_2bf_z2b .section}

After the job runs, you can view the running log of the job in the **Logs** tab at the bottom of the page. Click **Log Details** to jump to the detailed log page of the job, you can see information such as the job's submitting log and YARN Container log.

