# Hadoop MapReduce Job Configuration {#concept_rtn_r1p_y2b .concept}

Hadoop MapReduce Job Configuration

## Procedure {#section_kjy_pgp_y2b .section}

1.  Log on to the [Alibaba Cloud E-MapReduce Console](https://emr.console.aliyun.com/?spm=5176.8250060.103.1.48466f55SEaqMe#/cn-hangzhou) to enter the Cluster List page with primary account.
2.  Click the **Data Platform** tab on the top to enter the **Project List** page.
3.  Click **Design Workflow** of the specified project in the **Operation** column.
4.  On the left side of the Job Editing page, right-click on the folder you want to operate and select **New Job**.
5.  In the **New Job** dialog box, enter the job name, job description.

    Once the job type is selected, it cannot be modified.

6.  Select a Hadoop job type to create a Hadoop Mapreduce job. This type of job is Hadoop job submitted in the background via the following process.

    ```
    hadoop jar xxx.jar [MainClass] -Dxxx ....
    ```

7.  Click **OK**.

    **Note:** You can also create subfolder, rename folder, and delete folder by right-clicking on the folder.

8.  Fill in the **Content**box with command line parameters to be provided to submit this job. Please note that content to be filled in this option box shall be started with the first parameter after hadoop jar. That is to say, in the option box, the address for jar to be provided to run this job shall be first followed by \[MainClass\], and you can provide other command line parameters themselves.

    For instance, you want to submit a Hadoop sleep job which doesn't write/read any date, then this job will succeed only by submitting mapper reducer tasks to the cluster and waiting for each task to sleep for a while. In Hadoop \(e.g. hadoop-2.6.0\), this job is packaged in hadoop-mapreduce-client-jobclient-2.6.0-tests.jar of the Hadoop release version. If this job is submitted from the command line, the command will be:

    ```
    hadoop jar /path/to/hadoop-mapreduce-client-jobclient-2.6.0-tests.jar sleep -m 3 -r 3 -mt 100 -rt 100
    ```

    To configure this job in E-MapReduce, in the **Content** field, enter the following content:

    ```
    /path/to/hadoop-mapreduce-client-jobclient-2.6.0-tests.jar sleep -m 3 -r 3 -mt 100 -rt 100
    ```

    **Note:** 

    The jar package path used here is an absolute path on the e-mapreduce host. There is a problem that the user may put these jar packages anywhere, and as the cluster is created and released, these jar packages become unavailable as they are released. Therefore, upload the jar package using the following methods:

    1.  Users send their own jar packages to the bucket of the OSS for storage. When you configure the parameters for hadoop, click **select the OSS path** to select and execute the jar package you want from the OSS directory. System will then auto-complete the OSS address for jar packages. Be sure to switch the prefix of the jar for your code to ossref \(click **switch resource type**\), to ensure that the jar package is downloaded correctly by MapReduce.
    2.  1. Click **OK**, the OSS path for this package will be auto completed in the **Content** field. When a job is submitted, the system will find the corresponding jar packages automatically as per this path.
    3.  Behind the jar package path for this OSS, other command line parameters for running a job will be further filled in.
9.  Click **Save** to The job is configured successfully.

In above example, sleep job has no data input/output. If the job needs to read data and process input results \(e.g. wordcount\), the data input and output paths shall be specified. You can read/write data on HDFS of E-MapReduce cluster as well as data on OSS. To read/write data on OSS, it can be done only by writing the data path as the OSS path when filling input and output paths. For instance:

```
jar ossref://emr/checklist/jars/chengtao/hadoop/hadoop-mapreduce-examples-2.6.0.jar randomtextwriter -D mapreduce.randomtextwriter.totalbytes=320000 oss://emr/checklist/data/chengtao/hadoop/Wordcount/Input
```

