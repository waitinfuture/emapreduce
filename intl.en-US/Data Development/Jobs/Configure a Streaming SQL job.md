# Configure a Streaming SQL job

This topic describes how to configure a Streaming SQL job.

-   A project is created. For more information, see [Manage projects](/intl.en-US/Data Development/Manage projects.md).
-   Resources and data files required for a job are obtained, such as JAR packages, names of the data files, and storage paths of both the JAR packages and data files.

For more information about Streaming SQL, see [Spark Streaming SQL](/intl.en-US/Developer Guide/Spark Streaming SQL/Common keywords.md).

When you configure a Streaming SQL job, you must specify dependency libraries. The following table describes the recent versions and other details about the dependency library provided by Spark Streaming SQL. We recommend that you use the latest version of the dependency library.

|Dependency library|Supported version|Release date|Reference string|Description|
|------------------|-----------------|------------|----------------|-----------|
|datasources-bundle|2.0.0 \(recommended\)|2020/02/26|sharedlibs:streamingsql:datasources-bundle:2.0.0|Supported data sources include Kafka, LogHub, Druid, Tablestore, HBase, JDBC, DataHub, Redis, Kudu, and DTS.|
|1.9.0|2019/11/20|sharedlibs:streamingsql:datasources-bundle:1.9.0|Supported data sources include Kafka, LogHub, Druid, Tablestore, HBase, JDBC, DataHub, Redis, and Kudu.|
|1.8.0|2019/10/17|sharedlibs:streamingsql:datasources-bundle:1.8.0|Supported data sources include Kafka, LogHub, Druid, Tablestore, HBase, JDBC, DataHub, and Redis.|
|1.7.0|2019/07/29|sharedlibs:streamingsql:datasources-bundle:1.7.0|Supported data sources include Kafka, LogHub, Druid, Tablestore, HBase, and JDBC.|

For more information, see [Overview](/intl.en-US/Developer Guide/Spark Streaming SQL/Data source/Overview.md).

## Procedure

1.  Create a job.

    1.  Log on to the [Alibaba Cloud EMR console](https://emr.console.aliyun.com) by using your Alibaba Cloud account.

    2.  In the top navigation bar, select the region where your cluster residesand select a resource group based on your business requirements.

    3.  Click the Data Platform tab.

    4.  In the **Projects** section of the page that appears, find your project and click **Edit Job** in the Actions column.

    5.  In the **Edit Job** pane on the left, right-click the folder on which you want to perform operations and select **Create Job**.

        **Note:** You can also right-click the folder to create a subfolder, rename the folder, or delete the folder.

    6.  In the Create Job dialog box, specify **Name** and **Description**, and select **Streaming SQL** from the **Job Type** drop-down list.

    7.  Click **OK**.

2.  Specify the command line parameters required to submit the job in the **Content** field.

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
    FROM  ${slsTableName} 
    WHERE ${condition}
    ```

    **Note:** The command used to submit a Streaming SQL job is `streaming-sql -f {sql_script}`. The SQL statements that you enter in the job editor are saved in `sql_script`.

3.  Configure dependency libraries and actions on failures.

    1.  Click **Job Settings** in the upper-right corner of the job page.

    2.  Specify the dependency libraries and actions on failures on the **Streaming Task Settings** and Shared Libraries tabs.

        |Section|Parameter|Description|
        |-------|---------|-----------|
        |**Actions on Failures**|**Action on Current Statement Failure**|The action to perform when EMR fails to execute a statement. You can perform one of the following actions:         -   **Execute Next Statement**: Execute the next statement.
        -   **Terminate Job**: Terminate the job. |
        |**Dependent Libraries**|**Libraries**|The data source libraries on which Streaming SQL jobs depend. EMR publishes the libraries to the repository of the scheduling center as dependency libraries. You must specify dependency libraries when you create a job.To specify a dependency library, enter its reference string, such as sharedlibs:streamingsql:datasources-bundle:2.0.0. |

4.  Click **Save**.


