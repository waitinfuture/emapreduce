# Ad hoc queries {#concept_y4g_tch_kfb .concept}

Only three types of ad hoc query are supported: HiveSQL, SparkSQL, and Shell. When you execute an ad hoc query statement, the log and query results are displayed at the bottom of the log and query page.

## Create a job {#section_ibf_v1f_z2b .section}

When you execute a job on the Edit Jobs page and click Details, you are directed to the Details page that shows the operation logs and run logs of this job. Ad hoc queries and jobs are used in different places. Ad hoc queries are usually used by data analysts. You also need to use SQL as a tool to implement an ad hoc query.

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://emr.console.aliyun.com/).
2.  Click the Data Platform tab to enter the Projects page.
3.  Click **Design Workflow** next to the target project to enter the **Edit Jobs** page.
4.  In the navigation pane on the left, click the Query tab to enter the Query page.
5.  In the navigation pane on the left, right-click a folder as required and select **New Job**.
6.  In the **New Job** dialog box, enter the job name and description, and select a job type.

    The job type cannot be modified once the job has been created.

7.  Click **OK**.

    **Note:** By right-clicking a folder, you can rename it, delete it, and create new subfolders.


## Develop a job {#section_zgm_x1f_z2b .section}

For more information, see [HiveSQL](intl.en-US/User Guide/Workflow development/Jobs/Configure a Hive job.md#), [SparkSQL](intl.en-US/User Guide/Workflow development/Jobs/Configure a Spark SQL.md#), and [Shell](intl.en-US/User Guide/Workflow development/Jobs/Configure a Shell job.md#) job types.

**Note:** When you insert an OSS UNI and select OSSREF as the prefix, E-MapReduce downloads OSS files to your cluster and adds them to the classpath.

-   Basic job settings

    In the top-right corner, click **Configure Jobs**. The Job Settings dialog box is displayed.

    -   Resource File: If you want to add resources such as JAR packages or UDFs that a job execution depends on, you must upload these files to OSS. When you select a resource, you can use this resource in a job.
    -   Parameter Configuration: Specifies the values of variables used in a job. You can use variables in your code. The format is $\{variable name\}. Click the plus icon \(+\) on the right to add key-value pairs. The key is the name of the variable, whereas value is the value of the variable. You can also customize the time variable according to the schedule time. The rules are as follows:
        -   yyyy indicates the year \(4-digit format\).
        -   MM indicates the month.
        -   dd indicates the day.
        -   HH 24 indicates the hour. If the 12-hour clock is used, this is displayed as **hh**.
        -   mm indicates the minute.
        -   ss indicates the second.
        -   The time variable can be any combination of time containing yyyy. You can also use the plus symbol \(+\) to advance time and the minus symbol \(-\) to delay time. For example, if $\{yyyy-MM-dd\} indicates the current date, then:

            -   One year from now is: $\{yyyy+1y\} or $\{yyyy-MM-dd hh:mm:ss+1y\}.
            -   Three months from now is: $\{yyyyMM+3m\} or $\{hh:mm:ss yyyy-MM-dd+3m\}.
            -   Five days ago is: $\{yyyyMMdd-5d\} or $\{hh:mm:ss yyyy-MM-dd-5d\}.
-   Advanced job settings

    To configure advanced settings, click the **Advanced** tab on the **Job Settings** page.

    -   **Mode**: Job running modes, including YARN and LOCAL. In YARN mode, the job is submitted on YARN by the Launcher. In LOCAL mode, jobs run directly on the assigned host.
    -   **Scheduling Parameters**: Set job configurations, such as the YARN queue, CPU, memory, and Hadoop users. If you do not set this parameter, the job adopts the default value of the Hadoop cluster.
    -   
## Execute a job {#section_mmj_2bf_z2b .section}

Once a job has been developed and configured, you can click **Run** in the top right corner to run the job.

## View logs {#section_ajt_2bf_z2b .section}

After you execute a job, you can view its running logs on the Log tab at the bottom of the query page.

