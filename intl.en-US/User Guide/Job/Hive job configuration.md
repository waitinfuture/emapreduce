# Hive job configuration {#concept_cf2_rfp_y2b .concept}

When users are applying for a cluster in E-MapReduce, they are provided with a Hive environment by default. Users can directly create and operate their tables and data by using Hive.

## Procedure {#section_w5j_vfp_y2b .section}

1.  Prepare the Hive script in advance, for example:

    ```
    USE DEFAULT;
     DROP TABLE uservisits;
     CREATE EXTERNAL TABLE IF NOT EXISTS uservisits  (sourceIP STRING,destURL STRING,visitDate STRING,adRevenue DOUBLE,user
     Agent STRING,countryCode STRING,languageCode STRING,searchWord STRING,duration INT ) ROW FORMAT DELIMITED FIELDS TERMI
     NATED BY ',' STORED AS SEQUENCEFILE LOCATION '/HiBench/Aggregation/Input/uservisits';
     DROP TABLE uservisits_aggre;
     CREATE EXTERNAL TABLE IF NOT EXISTS uservisits_aggre ( sourceIP STRING, sumAdRevenue DOUBLE) STORED AS SEQUENCEFILE LO
     CATION '/HiBench/Aggregation/Output/uservisits_aggre';
     INSERT OVERWRITE TABLE uservisits_aggre SELECT sourceIP, SUM(adRevenue) FROM uservisits GROUP BY sourceIP;
    ```

2.  Save this script into a script file, such as uservisits\_aggre\_hdfs.hive, and then upload it to an OSS directory \(for example, oss://path/to/uservisits\_aggre\_hdfs.hive\).
3.  Log on to the [Alibaba Cloud E-MapReduce Console](https://emr.console.aliyun.com/?spm=5176.8250060.103.1.48466f55SEaqMe#/cn-hangzhou).
4.  At the top of the navigation bar, click **Data Platform**.
5.  In the **Actions** column, click **Design Workflow** of the specified project.
6.  On the left side of the Job Editing page, right-click on the folder you want to operate and select **New Job**.
7.  In the **New Job** dialog box, enter the job name and description.
8.  Select the Hive job type to create a Hive job. This type of job is submitted in the background using the following method:

    ```
    hive [user provided parameters]
    ```

9.  Click **OK**.

    **Note:** You can also create subfolder, rename folder, and delete folder by right-clicking on the folder.

10. Enter the parameters in the **Content**box with parameters subsequent to Hive commands. For example, if it is necessary to use a Hive script uploaded to OSS, the following must be entered:

    ```
    -f ossref://path/to/uservisits_aggre_hdfs.hive
    ```

    You can also click **Select OSS path** to view and select from OSS, the system will automatically complete the path of Hive script on OSS. Switch the Hive script prefix to ossref \(click **Switch resource type**\) to guarantee this file is properly downloaded by E-MapReduce.

11. Click **Save** to complete the Hive job configuration.

