# Job editing {#concept_iny_t1f_z2b .concept}

In the project, you can create jobs such as Shell, Hive, Spark, SparkSQL, MapReduce, Sqoop, and Pig.

## Create a job {#section_ibf_v1f_z2b .section}

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://partners-intl.console.aliyun.com/#/emr).
2.  Click the Data Platform tab to enter the Projects page.
3.  Click **Design Workflow** to the right of the specified project and to go to the Edit Jobs page.
4.  On the left side of the Job Editing page, right-click on the folder you want to operate and select **Create Job**.
5.  In the **New Job**dialog box, enter the job name and job description and select a job type.

    The job type cannot be modified once the job has been created.

6.  Click **OK**.

    **Note:** You can right click on a folder and then select the corresponding option to perform New Subfolders, Rename Folder, and Delete Folder operations.


## Develop a job {#section_zgm_x1f_z2b .section}

For more information about how to develop jobs, see the [Jobs](reseller.en-US/Data Development/Jobs/Configure a Hadoop MapReduce job.md#) section of the *E-MapReduce user guide*.

**Note:** When you insert an OSS UNI and select OSSREF as a File Prefix, E-MapReduce will download OSS files to your cluster and add these files to the classpath.

-   Basic job settings

    In the top-right corner, click **Configure Jobs**, and then the Job Settings dialog box appears.

    -   Failed Retries: Sets the number of retries when this job fails during the workflow running. This option will not take effect when you run the job directly on the Job Editing page.
    -   Failure Policy: Sets whether to continue running the next job or suspend the current workflow when this job fails during the workflow running.
    -   Add Running Resources: If you add resources such as Jar packages or UDFs that the job running depends on, you need to upload the resources to OSS first. When you select a resource, you can use this resource in a job directly.
    -   Parameter Configuration: specifies the values of variables used in a job. You can use variables in your code. The format is: $\{variable name\}. Click the plus \(+\) icon on the right side to add key-value pairs. Key is the name of a variable and value is the value of a variable. In addition, you can follow these rules to customize time variables according to the start time of a schedule.

        -   yyyy represents a 4-digit year.
        -   MM represents a 2-digit month.
        -   dd represents a 2-digit day.
        -   HH indicates that the 24-hour clock is used. hh indicates that the 12-hour clock is used.
        -   mm represents a 2-digit minute.
        -   ss represents a 2-digit second.

            A time variable consists of the combination of a 4-digit year and one or more other time formats. In addition, you can use plus \(+\) and minus \(-\) to add or reduce a period of time for the current time. For example, the $\{yyyy-MM-dd\} variable represents the current date.

            -   One year after the current date can be represented as $\{yyyy+Ny\} or $\{yyyy-MM-dd hh:mm:ss+1y\}.
            -   Three months after the current date can be represented as: $\{yyyyMM+Nm\} or $\{hh:mm:ss yyyy-MM-dd+3m\}.
            -   Five days before the current date can be represented as:$\{yyyyMMdd-Nd\} or $\{hh:mm:ss yyyy-MM-dd-5d\}.
            **Note:** The parameter of a time variable is required to start with yyyy. For example, $\{yyyy-MM\}. If you want to obtain the values based on a specific period such as a month, you can use the following functions in a job.

            -   ParseDate \(<parameter name\>, <time format\>\): Date converts a given parameter to a Date object. A parameter name represents the variable \(key\) name set in the Parameter Configuration area. A time format represents the time format used by the variable name. For example, if the parameter name is $\{yyyyMMddHHmmss-1d\}, the time format is yyyyMMddHHmmss.
            -   formatDate\(<Date object\>, <time format\>\): You can use this function to convert a specified Date object to a time format string.
            Examples:

            -   To retrieve the hour literal value of the current\_time variable: $\{formatDate\(parseDate\(current\_time, 'yyyyMMddHHmmss'\), 'HH'\)\}
            -   To retrieve the year literal value of the current\_time variable: $\{formatDate\(parseDate\(current\_time, 'yyyyMMddHHmmss'\), 'yyyy'\)\}
-   Advanced job settings

    In the **Job Settings** dialog box, click the **Advanced** tab.

    -   Mode: Submit on Worker Node and Submit on Header/Gateway Node. In the Submit on Worker Node mode, jobs are submitted to resources that are allocated by YARN as launchers. In the Submit on Header/Gateway Node mode, jobs are running on allocated nodes as processes.
    -   Environment Variable: The environment variables for running a job. You can also export the environment variables from a job script.
    -   Scheduling Parameters: YARN scheduler, CPU and memory specifications, and Hadoop users. Default values of a Hadoop cluster are applied if you do not specify the parameter values.

## Execute a job {#section_mmj_2bf_z2b .section}

After the development and configuration of a job are complete, you can click **Run** in the top-right corner to run the job.

## View logs {#section_ajt_2bf_z2b .section}

After a job runs, you can click the View Records tab to view the running logs of the job. Click **View Details** to go to the details page. On this page, you can view details, including the job submission log and YARN Container log.

