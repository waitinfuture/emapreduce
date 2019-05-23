# Edit a job {#concept_iny_t1f_z2b .concept}

In a project, you can create Shell, Hive, Spark, SparkSQL, MapReduce, Sqoop, Pig, and Spark Streaming jobs.

## Create a job {#section_ibf_v1f_z2b .section}

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://emr.console.aliyun.com/console).
2.  Click the Data Platform tab to enter the Projects page.
3.  Click **Workflows** in the Actions column. Click **Edit Job** in the left-side navigation pane to go to the Edit Job page.
4.  In the left-side navigation pane, right-click a folder as required and select **Create Job** from the drop-down list.
5.  In the **Create Job** dialog box, enter a name and description for the job and select a job type.

    Once selected, the job type cannot be modified.

6.  Click **OK**.

    **Note:** You can also right-click the folder to create a subfolder, rename the folder, and delete the folder.


## Develop a job {#section_zgm_x1f_z2b .section}

For more information, see the [EN-US\_TP\_17867.md\#](intl.en-US/Data Development/Jobs/Configure a Hadoop MapReduce job.md#) section in *EMR User Guide* .

**Note:** If you choose ossref:// as the OSS path prefix, the file that is stored in the OSS path is downloaded to the local machine and the local path is added to the value of the Classpath parameter.

-   Basic job settings

    In the upper-right corner, click **Job Settings**. The Job Settings dialog box appears.

    -   Retries: sets the number of retries when this job fails during the workflow running. This option will not take effect when you run the job on the Edit Job page.
    -   Actions on Failures: sets whether to continue running the next job or suspend the current workflow when this job fails during the workflow running.
    -   Resources: If you want to add resources such as jar packages or UDF that a job execution depends on, you must upload these files to OSS. When you select a resource, you can use this resource in a job directly.
    -   Configuration Parameters: specifies the values of variables used in a job. You can reference variables in your code. The format is: $\{variable name\}. Click the plus \(+\) icon on the right side to add key-value pairs. Key is the name of a variable and value is the value of a variable. In addition, you can follow these rules to customize time variables according to the start time of a schedule.

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

            -   parseDate\(<parameter name\>, <time format\>\): you can use this function to convert a specified parameter to a Date object. A parameter name represents the variable \(key\) name set in the Parameter Configuration area. A time format represents the time format used by the variable name. For example, if the parameter name is $\{yyyyMMddHHmmss-1d\}, the time format is yyyyMMddHHmmss.
            -   formatDate\(<Date object\>, <time format\>\): you can use this function to convert a specified Date object to a time format string.
            Examples:

            -   To retrieve the hour literal value of the current\_time variable: $\{formatDate\(parseDate\(current\_time, 'yyyyMMddHHmmss'\), 'HH'\)\}
            -   To retrieve the year literal value of the current\_time variable: $\{formatDate\(parseDate\(current\_time, 'yyyyMMddHHmmss'\), 'yyyy'\)\}
-   Advanced job settings

    In the **Job Settings** dialog box, click the **Advanced Settings** tab.

    -   Mode: **Worker Node** and **Header/Gateway Node**.
        -   In the **Worker Node** mode, jobs are submitted to resources that are allocated by YARN as launchers.
        -   In the **Header/Gateway Node** mode, jobs are running on allocated nodes as processes.
    -   Environment variables: the environment variables used to run the job. You can also export environment variables in the job script.

        For example, the content in a Shell job is `echo ${ENV_ABC}`. You set `ENV_ABC=12345` for the environment variable. A result of 12345 is returned by using the echo command. If the content of the job is `java -jar abc.jar` and the content of the abc.jar file is as follows:

        ```
        public static void main(String[] args) {System.out.println(System.getEnv("ENV_ABC"));}
        ```

        A result of 12345 is returned.

        Configuring the environment variable has the same effect of executing the following script.

        ```
        export ENV_ABC=12345
        java -jar abc.jar
        ```

    -   Scheduling parameters: includes information, such as YARN queues of a job, vCPU, memory, and Hadoop user. If you do not specify these parameters, a job uses the default values of the Hadoop cluster.

## Description {#section_fgl_cxq_kgb .section}

Job submission supports the Worker Node and Header/Gateway Node modes. \[DO NOT TRANSLATE\]

-   Header/Gateway: the spark-submit process is run on the header node. The spark-submit process requests much memory. A large number of jobs consume many resources of the header node, which causes an unstable cluster.
-   Worker: the spark-submit process is run on a worker node and is allocated to a container of YARN, which This can alleviate the resource usage of header nodes.

The spark-submit process \(LAUNCHER in the data development module\) is a Spark job submission command used to submit Spark jobs, typically occupying more than 600 MB of memory.

The Memory \(MB\) scheduling parameter in Job Settings is used to set the memory size allocated to LAUNCHER.

A complete Spark JOB includes: spark-submit \(memory consumption: 600 MB\) + driver \(memory consumption: depending on the specific JOB, either JOB or LAUNCHER\) + executor \(memory consumption: depending on the specific JOB implementation, JOB\)

-   If Spark uses yarn-client mode, spark-submit + driver is in the same process \(memory consumption: 600 MB + driver \). By using the Header/Gateway Node mode, the script is run on the header node and is not monitored by YARN. By using the Worker Node mode, the script is run on a worker node and monitored by YARN.

-   If Spark uses the YARN cluster mode, the driver runs a separate process and occupies a container of YARN. The driver and spark-submit are not in the same process.


Whether a script is run on the header node or a worker node depends on the job submission mode \(Header/Gateway Node or Worker Node\). Whether the driver is launched within the spark-submit process depends on the deploy mode \(yarn-client or yarn-cluster\).

## Execute a job {#section_mmj_2bf_z2b .section}

After the development and configuration of a job are complete, you can click **Run** in the upper-right corner to run the job.

## View logs {#section_ajt_2bf_z2b .section}

After you execute a job, you can view the running logs on the Records tab page at the bottom of the page. Click **Details** to go to the details page. On this page, you can view details, including the job submission log and YARN Container log.

## FAQs {#section_wk5_mrh_dhb .section}

-   Insufficient disk capacity caused by many logs of streaming jobs

    For streaming jobs such as Spark Streaming jobs, we recommend that you enable log rolling to avoid insufficient disk capacity caused by many logs of long-running jobs. Perform the following steps.

    1.  In the [E-MapReduce console](https://emr.console.aliyun.com/), choose **Data Development** \> **Project ID** \> **Edit Job** \> **Job Settings** \> **Advanced Settings**.
    2.  In the Environment Variables section, click the plus sign \(**+**\) to add the following environment variable.

        ```
        FLOW_ENABLE_LOG_ROLLING = true
        ```

    3.  Save and restart the job.

        **Note:** If you want to delete the log of a job without restarting the job, you can use the `echo > /path/to/log/dir/stderr` command to delete the job.


