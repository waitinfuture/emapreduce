# Create and run a Hive job. {#concept_yrq_tmm_hfb .concept}

This topic describes how to create and run a Hive job.

## Use OSS in Hive {#section_vmn_5mm_hfb .section}

To read and write OSS in Hive, use the following statement to create an external table:

```
CREATE EXTERNAL TABLE eusers (
  userid INT) 
 LOCATION 'oss://emr/users';
```

If your cluster version is obsolete and the preceding method is not supported, or if you want to access an OSS instance that is located in another region with the AccessKey of another account, follow these steps:

```
CREATE EXTERNAL TABLE eusers (
  userid INT) 
 LOCATION 'oss://${AccessKeyId}:${AccessKeySecret}@${bucket}.${endpoint}/users';
```

Parameter descriptions:

-   $\{accessKeyId\}: The AccessKeyId of your account.

-   $\{accessKeySecret\}: The secret key corresponding to the AccessKeyId.

-   $\{endpoint\}: The network used for accessing OSS. It depends on the region in which your cluster exists. The corresponding OSS instance should be in the region in which your cluster exists.

    For more information about specific values, see [OSS endpoints](../../SP_21/DNOSS11827291/EN-US_TP_4350.dita#concept_zt4_cvy_5db).


## Use Tez as the computing engine {#section_yqc_ynm_hfb .section}

Tez is implemented after E-MapReduce version 2.1.0+. Tez is a computing framework for optimizing complex DAG scheduling jobs. It dramatically improves the speed of Hive jobs in many scenarios.

You can optimize your jobs by setting Tez as the execution engine. Follow these steps:

```
set hive.execution.engine=tez
```

-   Example 1

    Follow these steps:

    1.  1. Write the following script, save it as hiveSample1.sql, and then upload it to OSS.

        ```
        USE DEFAULT;
         set hive.input.format=org.apache.hadoop.hive.ql.io.HiveInputFormat;
         set hive.stats.autogather=false;
         DROP TABLE emrusers;
         CREATE EXTERNAL TABLE emrusers (
           userid INT,
           movieid INT,
           rating INT,
           unixtime STRING ) 
          ROW FORMAT DELIMITED 
          FIELDS TERMINATED BY '\t' 
          STORED AS TEXTFILE 
          LOCATION 'oss://${bucket}/yourpath';
         SELECT COUNT(*) FROM emrusers;
         SELECT * from emrusers limit 100;
         SELECT movieid,count(userid) as usercount from emrusers group by movieid order by usercount desc limit 50;
        ```

    2.  Data resources for testing.

        You can download required resources for Hive jobs from the following link and store the resources to the corresponding directory in your OSS instance.

        Download resources: [Public testing data](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/cn/emr/1.3.7/assets/res/u.data).

    3.  Create a job

        Create a new job in E-MapReduce using the following configuration:

        ```
        -f ossref://${bucket}/yourpath/hiveSample1.sql
        ```

        In this example, $\{bucket\} represents your OSS buckets, and yourPath represents a path in the bucket. You need to replace yourPath with where the Hive script is saved.

    4.  Create and run an execution plan.

        Create an execution plan. You can associate it with an existing cluster, or create a cluster as needed and associate the plan with the cluster. Save the job as **Run Manually**. Return to the previous page and click **Run Now** to run the job.

-   Example 2

    Take scan in [HiBench](https://github.com/intel-hadoop/HiBench) as an example.

    Follow these steps:

    1.  Write the following script:

        ```
        USE DEFAULT;
         set hive.input.format=org.apache.hadoop.hive.ql.io.HiveInputFormat;
         set mapreduce.job.maps=12;
         set mapreduce.job.reduces=6;
         set hive.stats.autogather=false;
         DROP TABLE uservisits;
         CREATE EXTERNAL TABLE uservisits (sourceIP STRING,destURL STRING,visitDate STRING,adRevenue DOUBLE,userAgent STRING,countryCode STRING,languageCode STRING,searchWord STRING,duration INT ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' STORED AS SEQUENCEFILE LOCATION 'oss://${bucket}/sample-data/hive/Scan/Input/uservisits';
        ```

    2.  Prepare test data

        You can download required resources for the job from the following link and store the resources to the corresponding directory in your OSS instance.

        Download resources: [uservisits](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/cn/emr/1.3.7/assets/res/uservisits)

    3.  Create a job

        Store the script created in step 1 to OSS. For example, if the storage path is oss://emr/jars/scan.hive, follow these steps to create a job in E-MapReduce:

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17985/154752332513195_en-US.png)

    4.  Create and run an execution plan.

        Create an execution plan. You can associate it with an existing cluster, or create a cluster as needed and associate the plan with the cluster. Save the job as **Run Manually**. Return to the previous page and click **Run Now** to run the job.


