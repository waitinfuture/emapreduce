# Configure a Hive job

E-MapReduce \(EMR\) provides a Hive environment. You can use Hive to create tables and perform operations on the tables and the data in them.

-   A project is created. For more information, see [Manage projects](/intl.en-US/Data Development/Manage projects.md).
-   A Hive SQL script, for example, uservisits\_aggre\_hdfs.hive, is uploaded to a path in OSS, such as oss://path/to/.

    Content of uservisits\_aggre\_hdfs.hive:

    ```
    USE DEFAULT;
     DROP TABLE uservisits;
     CREATE EXTERNAL TABLE IF NOT EXISTS uservisits (sourceIP STRING,destURL STRING,visitDate STRING,adRevenue DOUBLE,userAgent STRING,countryCode STRING,languageCode STRING,searchWord STRING,duration INT) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' STORED AS SEQUENCEFILE LOCATION '/HiBench/Aggregation/Input/uservisits';
     DROP TABLE uservisits_aggre;
     CREATE EXTERNAL TABLE IF NOT EXISTS uservisits_aggre (sourceIP STRING, sumAdRevenue DOUBLE) STORED AS SEQUENCEFILE LOCATION '/HiBench/Aggregation/Output/uservisits_aggre';
     INSERT OVERWRITE TABLE uservisits_aggre SELECT sourceIP, SUM(adRevenue) FROM uservisits GROUP BY sourceIP;
    ```


## Procedure

1.  Log on to the [Alibaba Cloud EMR console](https://emr.console.aliyun.com) by using your Alibaba Cloud account.

2.  In the top navigation bar, select the region where your cluster residesand select a resource group based on your business requirements.

3.  Click the **Data Platform** tab.

4.  In the **Projects** section of the page that appears, find the project you want to edit and click **Edit Job** in the Actions column.

5.  In the **Edit Job** pane on the left, right-click the folder on which you want to perform operations and select **Create Job**.

6.  In the Create Job dialog box, specify **Name** and **Description**, and select **Hive** from the **Job Type** drop-down list.

    This option indicates that a Hive job will be created. You can use the following command syntax to submit a Hive job:

    ```
    hive [user provided parameters]
    ```

7.  Click **OK**.

8.  Specify the command line parameters required to submit the job in the **Content** field.

    For example, to use a Hive script uploaded to OSS, enter the following command:

    ```
    -f ossref://path/to/uservisits_aggre_hdfs.hive
    ```

    **Note:** `path` indicates the path in which `uservisits_aggre_hdfs.hive` is stored in OSS.

    Click **+ Enter an OSS path** in the lower part of the page. In the OSS File dialog box, specify File Path. The system automatically completes the path of the Hive script in OSS. File Prefix must be set to **OSSREF** to ensure that EMR can download the file.

9.  Click **Save**.


