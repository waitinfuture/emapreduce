# Create a job {#concept_qvv_qv4_y2b .concept}

This section describes how to create a job for scheduling in an early E-MapReduce version.

To run a computing task, you need to follow these steps to define a job:

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://emr.console.aliyun.com/).
2.  Select a region where you want to create the job.
3.  Click the Old EMR Scheduling tab to go to the jobs list page.
4.  Click **Create job** in the upper-right corner to go to the job creation page, as shown in the following figure:

    ![Create job](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17841/155063292710493_en-US.png)

5.  Enter the **job name**.
6.  Select a **job type**.
7.  Enter **parameters** of the job. The application parameters must include the following information: the JAR package run by the job, data input and output addresses of the job, and certain command line parameters. You must enter all parameters in the command line in this field. If you need to use an OSS path, click **Select OSS Path** to select the OSS resource path. For more information about the parameters of all job types, see the Job section in User Guide.
8.  **Actual execution**. The job command that has been executed on ECS will be displayed in this field. You can copy the displayed command and directly run it in the command line environment on an E-MapReduce cluster.
9.  **Fail Retry**. You can set the number of retries and their intervals. This feature is disabled by default.
10. Select a **failure policy**. Pause the current execution plan: Indicates that the current execution plan will be paused if this job fails and will wait for you to process. Continue execution of the next job: Indicates that errors will be ignored and the next job will be executed after this job fails.
11. Click **OK** to complete the creation.

## Example {#section_pjr_cx4_y2b .section}

This is a Spark job, which sets parameters such as the input and output paths in the application parameters.

**Note:** This example is for reference only.

![Spark job example](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17841/155063292710494_en-US.jpg)

## OSS and ossref {#section_rjr_cx4_y2b .section}

The **oss://** prefix indicates that the data path points to an OSS path, which specifies the operation path when reading/writing the data. This is similar to **hdfs://**.

The **ossref://** prefix also indicates that the data path points to an OSS path. However, it is used to download the corresponding code to a local disk, and then replace the path in the command line with this local path. It is easier for you to run native code. You do not need to log on to the computer to upload the code and the dependent resource packages.

In this example, the **ossref://xxxxxx/xxx.jar** parameter represents the JAR package of job resources. This JAR package is stored on OSS. When this path is executed in the code, the JAR package will automatically download to the cluster and be executed. The two **oss://xxxx** and the two values following the JAR package are processed by the main class in the JAR package as parameters.

**Note:** The **ossref** cannot be used to download large amounts of data. Otherwise, an error may occur.

