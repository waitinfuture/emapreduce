# Create a job {#concept_qvv_qv4_y2b .concept}

In this tutorial, you will learn how jobs are created in E-MapReduce.

To run a computing task, you need to define a job first according to the following steps:

1.  Log on to [Alibaba Cloud E-MapReduce console](https://emr.console.aliyun.com/console/#/job/region/ap-southeast-1).
2.  Select the region where the job is created.
3.  At the top of the page, click **Old EMR Scheduling**.
4.  In the upper right corner of the page, click **Create Job**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17841/154218774110493_en-US.png)

5.  Enter the job **Name**.
6.  Select a job **Type**.
7.  Enter **Parameters** of the job. Parameters must include full information of the jar package used by the job, data input and output addresses of the job, and some command line parameters, that is, all your parameters in the command line must be completed in this field. You can click **Select OSS path** to select an OSS resource path. For parameter configurations of all job types, see the Job chapter in User Guide.
8.  **Actual execution**: The actual executed command for the job on ECS will be displayed. If you copy the displayed command, the command can be run directly in the command line environment of the E-MapReduce cluster.
9.  **Fail retry**: If you select **Yes**, you can set the number of retries and each retry interval. It is set **No** by default.
10. **Failure policy**: Pausing the current execution plan will pause the entire execution plan after this job fails and will wait for your handling. Continuing to execute the next job will ignore this error and continue to execute the next job after this job fails.
11. Click **OK** to complete the creation.

## Job Example {#section_pjr_cx4_y2b .section}

This is a Spark job, where relevant parameters, input and output paths, are set in application parameters.

**Note:** This example is only for reference.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17841/154218774110494_en-US.jpg)

## OSS and ossref {#section_rjr_cx4_y2b .section}

The **oss://** prefix indicates that the data path is directed to an OSS path, which specifies the operation path similar to **hdfs://** when reading/writing the data.

The **ossref://** is also directed to an OSS path, however, it will be used to download the corresponding code resource to the local disk, and then replace the path in the command line with the local path. It is intended for easily running some native codes without the need to log on to the computer to upload the code and the dependent resource package.

The ossref://xxxxxx/xxx.jar parameter in the preceding example represents the jar of job resources. This jar is stored in OSS. When the system is running, the jar will be downloaded to clusters for running automatically in E-MapReduce. The two oss://xxxx and another two values behind a jar file appearing as parameters are passed to the main class in a jar file.

**Note:** The **ossref** cannot be used to download excessive data resources, otherwise it will lead to failure of the cluster job.

