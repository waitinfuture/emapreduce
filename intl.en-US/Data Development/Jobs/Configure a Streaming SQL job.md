# Configure a Streaming SQL job

This topic describes how to configure a Streaming SQL job.

-   A project is created. For more information, see [Manage projects](/intl.en-US/Data Development/Manage a workflow project.md).
-   The information about required resources and data files to be processed is obtained, such as the JAR file name, data file name, and storage paths of the files.

For more information about Streaming SQL, see [Common keywords](/intl.en-US/Developer Guide/Spark Streaming SQL/Common keywords.md).

When configuring a Streaming SQL job, you need to specify dependent libraries. The following table lists some recent versions and other details about the dependent library provided by Spark Streaming SQL. You need to use the latest version of the dependent library.

|Library|Version|Released at|Reference string|Description|
|-------|-------|-----------|----------------|-----------|
|datasources-bundle|1.9.0|November 20, 2019|sharedlibs:streamingsql:datasources-bundle:1.9.0|Supported data sources include Kafka, Loghub, Druid, TableStore, HBase, Java Database Connectivity \(JDBC\), Datahub, Redis, and Kudu.|
|1.8.0|October 17, 2019|sharedlibs:streamingsql:datasources-bundle:1.8.0|Supported data sources include Kafka, Loghub, Druid, TableStore, HBase, JDBC, Datahub, and Redis.|
|1.7.0|July 29, 2019|sharedlibs:streamingsql:datasources-bundle:1.7.0|Supported data sources include Kafka, LogHub, Druid, Table Store, HBase, and JDBC.|

-   Copy the reference string to the **Dependent Libraries** section of the **Streaming Task Settings** tab in the **Job Settings** dialog box in Data Platform of the E-MapReduce console.
-   All the data sources in the preceding table support stream reads and writes.
-   For more information, see [Data sources](/intl.en-US/Developer Guide/Spark Streaming SQL/Data source/Overview.md).

1.  Log on to the [E-MapReduce console](https://emr.console.aliyun.com/) by using an Alibaba Cloud account.

2.  Click the Data Platform tab.

3.  In the **Projects** section, click **Edit Job** in the Actions column of the target project.

4.  Right-click the folder where you want to create a job, and select **Create Job**.

    **Note:** You can also right-click the folder to create a subfolder, rename the folder, or delete the folder.

5.  In the dialog box that appears, set the **Name** and **Description** parameters, and select Streaming SQL from the Job Type drop-down list.

6.  Click **OK**.

7.  Specify the command line arguments required to submit the job in the **Content** field.

    Example:

    ```
    --- Create a Log Service table. 
    CREATE TABLE IF NOT EXISTS ${slsTableName} 
       USING loghub 
       OPTIONS ( 
            sls.project = '${logProjectName}', 
            sls.store = '${logStoreName}', 
            access.key.id = '${accessKeyId}', 
            access.key.secret = '${accessKeySecret}', 
            endpoint = '${endpoint}'
       ); 
    --- Import data to HDFS. 
    INSERT INTO 
        ${hdfsTableName} 
    SELECT 
        col1, col2 
    FROM ${slsTableName} 
    WHERE ${condition}
    ```

    **Note:** The command used to submit a Streaming SQL job is `streaming-sql -f {sql_script}`. `sql_script` refers to the code for executing the Streaming SQL job, namely, Streaming SQL statements.

8.  Configure dependent libraries and actions on failures.

    1.  Click **Job Settings** in the upper-right corner. In the dialog box that appears, click the **Streaming Task Settings** tab.

    2.  Specify the dependent libraries and actions on failures on the Streaming Task Settings tab.

        |Section|Parameter|Description|
        |-------|---------|-----------|
        |**Actions on Failures**|**Action on Current Statement Failure**|The action to perform when E-MapReduce fails to execute a statement. You can choose to perform either of the following actions:         -   **Execute Next Statement**: execute the next statement.
        -   **Terminate Job**: terminate the job. |
        |**Dependent Libraries**|**Libraries**|The data source libraries on which Streaming SQL jobs depend. E-MapReduce publishes these libraries to the repository of the scheduling center as dependent libraries. When you create a job, you need to specify dependent libraries for the job.

To specify a dependent library, enter its reference string, such as sharedlibs:streamingsql:datasources-bundle:1.9.0. |

9.  Click **Save**.


