# Configure a Hadoop MapReduce job {#concept_rtn_r1p_y2b .concept}

## Procedure {#section_kjy_pgp_y2b .section}

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://emr.console.aliyun.com/?spm=5176.8250060.103.1.48466f55SEaqMe#/cn-hangzhou).
2.  At the top of the navigation bar, click **Data Platform**.
3.  In the **Actions** column, click **Design Workflow** next to the specified project.
4.  On the left of the Job Editing page, right-click the folder you want to operate and select **New Job**.
5.  In the **New Job** dialog box, enter the job name and description.
6.  Select a Hadoop job type to create a Hadoop MapReduce job. This type of job is submitted in the background using the following process.

    ```
    hadoop jar xxx.jar [MainClass] -Dxxx ....
    ```

7.  Click **OK**.

    **Note:** You can also create subfolders, rename folders, and delete folders by right-clicking on them.

8.  Enter the parameters in the **Content** field that are required to submit this job. Enter the parameters after the Hadoop jar, followed by other command line parameters.

    For instance, if you want to submit a Hadoop sleep job that does not read or write any data, this will only succeed if you submit Mapper/Reducer tasks to the cluster and wait for each task to sleep for a while. In Hadoop, this job is packaged in the Hadoop release version's hadoop-mapreduce-client-jobclient-2.6.0-tests.jar. If this job is submitted from the command line, the command should read as follows.

    ```
    hadoop jar /path/to/hadoop-mapreduce-client-jobclient-2.6.0-tests.jar sleep -m 3 -r 3 -mt 100 -rt 100
    ```

    To configure this job in E-MapReduce, enter the following content in the **Content** field.

    ```
    /path/to/hadoop-mapreduce-client-jobclient-2.6.0-tests.jar sleep -m 3 -r 3 -mt 100 -rt 100
    ```

    **Note:** 

    The jar package path used here is an absolute path on the E-MapReduce host. However, the user may put these jar packages anywhere, and as clusters are created and released, the packages become unavailable. Therefore, upload the jar package by performing the following steps:

    1.  Users send their own jar packages to the bucket of OSS for storage. When you configure the parameters for Hadoop, click **Select OSS path** to select and execute the jar package you want from the OSS directory. Thes system will then auto-complete the OSS address for jar packages. Be sure to switch the prefix of the jar to ossref by clicking **Switch resource type**. This ensures that the jar package is downloaded correctly by MapReduce.
    2.  Click **OK**. The OSS path for this package will be auto-completed in the **Content** field. When a job is submitted, the system will find the corresponding jar packages automatically based on this path.
    3.  Behind the jar package path for this OSS, other command line parameters for running jobs will be filled in further.
9.  Click **Save**.

In the example above, the sleep job has no data input/output. If you want the job to read data and process input results, such as word counts, the data input and output paths need to be specified. You can read/write data on the HDFS of the E-MapReduce cluster as well as on OSS. To read/write data on OSS, write the data path as the OSS path when specifying the input and output paths. For instance:

```
jar ossref://emr/checklist/jars/chengtao/hadoop/hadoop-mapreduce-examples-2.6.0.jar randomtextwriter -D mapreduce.randomtextwriter.totalbytes=320000 oss://emr/checklist/data/chengtao/hadoop/Wordcount/Input
```

