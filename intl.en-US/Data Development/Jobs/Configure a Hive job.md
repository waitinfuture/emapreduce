# Configure a Hive job {#concept_cf2_rfp_y2b .concept}

When you apply for clusters in E-MapReduce, you are provided with a Hive environment by default. Using Hive, you cancreate and operate tables and data.

## Procedure {#section_w5j_vfp_y2b .section}

1.  Prepare the Hive script in advance. For example:

    ``` {#codeblock_7kb_ryu_ddi}
    USE DEFAULT;
     DROP TABLE uservisits;
     CREATE EXTERNAL TABLE IF NOT EXISTS uservisits (sourceIP STRING,destURL STRING,visitDate STRING,adRevenue DOUBLE,userAgent STRING,countryCode STRING,languageCode STRING,searchWord STRING,duration INT) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' STORED AS SEQUENCEFILE LOCATION '/HiBench/Aggregation/Input/uservisits';
     DROP TABLE uservisits_aggre;
     CREATE EXTERNAL TABLE IF NOT EXISTS uservisits_aggre (sourceIP STRING, sumAdRevenue DOUBLE) STORED AS SEQUENCEFILE LOCATION '/HiBench/Aggregation/Output/uservisits_aggre';
     INSERT OVERWRITE TABLE uservisits_aggre SELECT sourceIP, SUM(adRevenue) FROM uservisits GROUP BY sourceIP;
    ```

2.  Save this script into a script file, such as uservisits\_aggre\_hdfs.hive, and upload it to an OSS directory \(for example, oss://path/to/uservisits\_aggre\_hdfs.hive\).
3.  Log on to the [Alibaba Cloud E-MapReduce console](https://partners-intl.console.aliyun.com/#/emr) .
4.  At the top of the navigation bar, click **Data Platform**.
5.  In the **Actions** column, click **Design Workflow**next to the specified project.
6.  On the left of the Job Editing page, right-click the folder you want to operate and select **New Job**.
7.  In the **New Job** dialog box, enter the job name and description.
8.  Select the Hive job type to create a Hive job. This type of job is submitted in the background using the following method.

    ``` {#codeblock_j5z_eqh_tue}
    hive [user provided parameters]
    ```

9.  Click **OK**.

    **Note:** You can also create subfolders, rename folders, and delete folders by right-clicking on them.

10. Enter the parameters in the **Content** field after the Hive commands. For example, if you want to use a Hive script uploaded to OSS, enter the following.

    ``` {#codeblock_w79_pak_c20}
    -f ossref://path/to/uservisits_aggre_hdfs.hive
    ```

    You can also click **Select OSS path** to view and select from OSS. The system will automatically complete the path of the Hive script on OSS. Switch the Hive script prefix to ossref by clicking**Switch resource type**. This ensures that the file is correctly downloaded by E-MapReduce.

11. Click **Save** to complete the Hive job configuration.

