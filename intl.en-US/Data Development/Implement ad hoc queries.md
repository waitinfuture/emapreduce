# Implement ad hoc queries

E-MapReduce \(EMR\) supports ad hoc queries, which are intended for data scientists and data analysts. You can execute SQL statements to implement ad hoc queries. When you run an ad hoc query job, relevant logs and query results are displayed in the lower part of the job page. EMR supports ad hoc queries based on Shell, Spark SQL, Spark Shell, and Hive SQL.

A project is created. For more information, see [Manage projects](/intl.en-US/Data Development/Manage projects.md).

## Create a job

1.  Log on to the [Alibaba Cloud EMR console](https://emr.console.aliyun.com) by using your Alibaba Cloud account.

2.  Click the **Data Platform** tab.

3.  In the **Projects** section of the page that appears, find your project and click **Edit Job** in the Actions column.

4.  In the **Edit Job** pane, click the ![search_temp](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1059625061/p70532.png) icon on the left.

5.  In the **Ad Hoc Queries** pane, right-click the folder on which you want to perform operations and select **Create Job**.

6.  In the **Create Interactive Job** dialog box, specify **Name**, **Description**, and **Job Type**.

    **Note:** The job type cannot be changed after the job is created.

7.  Click **OK**.

    You can also right-click the folder and select **Create Subfolder**, **Rename Folder**, or **Delete Folder** to perform the corresponding operation.


## Configure a job

For more information about how to develop and configure each type of job, see [Jobs](/intl.en-US/Data Development/Jobs/Configure a Shell job.md). This section describes how to configure parameters on the **Basic Settings**, **Advanced Settings**, and **Alert Settings** tabs in the Job Settings panel for a job.

1.  On the job page, click **Job Settings** in the upper-right corner.

2.  In the **Job Settings** panel, configure parameters on the Basic Settings tab.

    |Section|Parameter and description|
    |-------|-------------------------|
    |**Job Overview**|    -   **Name**: the name of the job.
    -   **Job Type**: the type of the job.
    -   **Description**: the description of the job. You can modify the description. |
    |**Resources**|The resources required to run the job, such as JAR packages and user-defined functions \(UDFs\). Click the ![Plus sign](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/7882628951/p66254.png) icon on the right to add resources. Upload resources to OSS first. Then, you can add them to the job.|
    |**Configuration Parameters**|The variables you want to reference in the job script. You can reference a variable in your job script in the format of $\{Variable name\}. Click the ![Plus sign](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/7882628951/p66254.png) icon on the right to add a variable with a key-value pair. The key indicates the name of the variable. The value indicates the value of the variable. In addition, you can configure a time variable based on the start time of scheduling. For more information, see [Configure job time and date](/intl.en-US/Data Development/Jobs/Time and date variables.md). |

3.  Click the **Advanced Settings** tab and configure parameters.

    |Section|Parameter and description|
    |-------|-------------------------|
    |**Mode**|    -   **Job Submission**: the job submission mode. For more information, see [Edit jobs](/intl.en-US/Data Development/Edit jobs.md). Valid values:
        -   **Worker Node**: The job is submitted to YARN by using a launcher and YARN allocates resources to run the job.
        -   **Header/Gateway Node**: The job runs on the allocated node as a process.
    -   **Estimated Maximum Duration**: the estimated maximum running duration of the job. Valid values: 0 to 10800. Unit: seconds. |
    |**Environment Variables**|The environment variables used to run the job. You can also export environment variables in the job script.     -   Example 1: Configure a Shell job with the code `echo ${ENV_ABC}`. Assume that you set the ENV\_ABC variable to `12345`. After you run the `echo` command, a value of `12345` is returned.
    -   Example 2: Configure a Shell job with the code `java -jar abc.jar`. Content of the abc.jar package:

        ```
public static void main(String[] args) {System.out.println(System.getEnv("ENV_ABC"));}
        ```

Assume that you set the ENV\_ABC variable to 12345. After you run the job, a value of `12345` is returned. The effect of setting the ENV\_ABC variable in the Environment Variables section is equivalent to running the following script:

        ```
export ENV_ABC=12345
java -jar abc.jar
        ``` |
    |**Scheduling Parameters**|The parameters for scheduling the job, including Queue, Memory \(MB\), vCores, Priority, and Run By. If you do not specify these parameters, the default settings of the Hadoop cluster are used. **Note:** Memory \(MB\) specifies the memory quota for the launcher. |

4.  Click the **Alert Settings** tab and configure the alert parameters.

    |Parameter|Description|
    |---------|-----------|
    |**Execution Failed**|Specifies whether to send a notification to a specific alert contact group or DingTalk alert group if the job fails.|
    |**Action on Startup Timeout**|Specifies whether to send a notification to a specific alert contact group or DingTalk alert group if the job fails to start.|
    |**Job execution timed out.**|Specifies whether to send a notification to a specific alert contact group or DingTalk alert group if the job times out.|


## Run a job

On the job page, click **Run** in the upper-right corner.

After you run the job, you can view the operational logs on the Log tab in the lower part of the job page.

## Lock job editing

When you edit a job, you can click **Lock** in the upper-right corner of the job page to lock job editing. This way, only the account you use can edit the job. Other members in the project can edit this job only after it is unlocked.

**Note:** Only the RAM user who performs the lock operation and the Alibaba Cloud account can unlock the job.

