# Job operations {#concept_iny_t1f_z2b .concept}

In a project, you can create jobs such as Shell, Hive, Spark, SparkSQL, MapReduce, Sqoop, Pig and Spark Streaming jobs.

## Create a job {#section_ibf_v1f_z2b .section}

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://emr.console.aliyun.com/?spm=5176.8250060.103.1.48466f55SEaqMe#/cn-hangzhou).
2.  Click the **Data Platform** tab to enter the **Projects** page.
3.  Click **Design Workflow** next to the target project in the **Actions** column.
4.  On the left side of the job editing page, right-click the folder you want to operate on and select **New Job**.
5.  In the **New Job** dialog box, enter a name and description for the job and select a job type.

    Once the job type is selected, it cannot be modified.

6.  Click **OK**.

    **Note:** You can also create subfolders, rename folders, and delete folders by right-clicking on them.


## Develop a job {#section_zgm_x1f_z2b .section}

For more information about the various types of jobs, see [Jobs](reseller.en-US/User Guide/Workflow development/Jobs/Configure a Hadoop MapReduce job.md#).

**Note:** When you insert an OSS path, if you select the OSSREF file prefix, the OSS file is downloaded to the cluster and added to the classpath.

-   Basic settings

    Click the **Job Settings** in the upper-right corner of the page to enter the **Job Running Configuration** page.

    -   **Number of Retries**: Sets the number of retries for if a job fails during a workflow. This option does not take effect if you run the job directly on the **Job Editing** page.
    -   **Failure Policy**: Sets whether to run the next job or suspend the current workflow in the event that a job fails during a workflow.
    -   **Resource File**: If you add resources such as JAR packages or UDFs that the current job depends on, you need to upload the resources to OSS first. After doing so, you can reference the resources directly in the job code.
    -   **Parameter Configuration**: Specifies the value of the variable referenced in the job code. You can reference variables in the code with the format $\{variable name\}. Click the plus icon \(+\) on the right to add the key and value. The key is the variable name, whereas the value is the value of the variable. You can also customize the time variable according to the schedule time. The rules are as follows:
        -   yyyy indicates the year \(4-digit format\).
        -   MM indicates the month.
        -   dd indicates the day.
        -   HH24 indicates the hour. If the 12-hour clock is used, this is displayed as **hh**.
        -   mm indicates the minute.
        -   ss indicates the second.

            The time variable can be any combination of time containing yyyy. You can also use the plus symbol \(+\) to advance time and the minus symbol \(-\) to delay time. For example, if $\{yyyy-MM-dd\} indicates the current date, then:

            -   One year from now is: $\{yyyy+1y\} or $\{yyyy-MM-dd hh:mm:ss+1y\}.
            -   Three months from now is: $\{yyyyMM+3m\} or $\{hh:mm:ss yyyy-MM-dd+3m\}.
            -   Five days ago is: $\{yyyyMMdd-5d\} or $\{hh:mm:ss yyyy-MM-dd-5d\}.
-   Advanced settings

    To configure advanced settings, click the **Advanced** tab on the **Job Settings** page.

    -   **Mode**: Job running modes, including YARN and LOCAL. In YARN mode, the job is submitted on YARN by the Launcher. In LOCAL mode, jobs run directly on the assigned host.
    -   **Environment Variables**: Add environment variables to run jobs, or export environment variables directly in the job script.
    -   **Scheduling Parameters**: Set job configurations, such as the YARN queue, CPU, memory, and Hadoop users. If you do not set this parameter, the job adopts the default value of the Hadoop cluster.

## Run a job {#section_mmj_2bf_z2b .section}

Once a job has been developed and configured, you can click **Run** in the top right corner to run the job.

## View a log {#section_ajt_2bf_z2b .section}

After the job has been set to run, you can view its running log in the **View Records** tab at the bottom. Click **Workflow** to enter the detailed log page. Here, you can see information such as the job's submitting log and the YARN Container log.

