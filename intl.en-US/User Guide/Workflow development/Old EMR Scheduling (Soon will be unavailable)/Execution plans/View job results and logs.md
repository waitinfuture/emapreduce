# View job results and logs {#concept_w1b_dzp_y2b .concept}

In this tutorial, you will learn how to view job results and logs.

## View execution records {#section_zmg_hzp_y2b .section}

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://emr.console.aliyun.com/).
2.  Select a region.
3.  In the upper-right corner, click Old MER Scheduling to go to the Jobs page.
4.  In the navigation panel on the left, click Execution plan.
5.  To the right of the execution plan, click **More** \> **Running log**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17880/154684742810574_en-US.jpg)

    -   **Execution order ID**: The sequence of execution for the execution record, which indicates its position in the execution queue. For example, 1 stands for the first position.
    -   **Running status**: The running status of each execution record.
    -   **Start time** The time at which the execution plan starts.
    -   **Running time**: The total running time until the page is viewed.
    -   **Execute cluster**: The cluster run by the execution plan can either be created on demand or it can be an existing associated cluster. Click to view the cluster details page.
    -   **Operation**

        **View job list**: Click to enter the job list page.


## View job records {#section_ycd_jzp_y2b .section}

On the**Job list** page, you can view the job list in the execution records of a single execution plan as well as the details of each job, as shown in the following figure.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17880/154684742810575_en-US.jpg)

-   **Job execution order ID**: After a job is executed, a corresponding ID is created, which is different from the job ID. The job execution ID is the unique identifier for viewing logs on OSS.

-   **Name**: The name of the job.

-   **Status**: The running status of the job.

-   **Type**: The type of job.

-   **Start time**: The time at which the job starts. This is converted into local time.

-   **Running time**: The total running time of the job, in seconds.

-   **Operation**

    -   **Stop job**: You can stop a job if it is in the process of submission or running. If a job is in submission, stopping it will cancel execution. If the job is running, it will be killed.
    -   **stdout**: Records all output content from the standard output \(Channel 1\) of the master process. If log saving is not enabled for the cluster where jobs are run, this function cannot be executed.
    -   **stderr**: Records all output content from the diagnostic output \(Channel 2\) of the master process. If log saving is not enabled for the cluster where jobs are run, this function cannot be executed.
    -   **Workers log**: Views the logs of all job worker nodes. If log saving is not enabled for the cluster where jobs are run, this function cannot be executed.

## View job worker logs {#section_j2f_kzp_y2b .section}

-   **Cloud server instance IP**: The ECS instance ID of a running job and the corresponding intranet IP address.

-   **Container ID**: The container ID that YARN runs.

-   **Type**: Different log types. stdout and stderr come from different outputs.

-   **Operation**

    **View the log**: Click different types to view the corresponding logs.


