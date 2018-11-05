# View job results and logs {#concept_w1b_dzp_y2b .concept}

View job results and logs

## View execution records {#section_zmg_hzp_y2b .section}

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://emr.console.aliyun.com/).
2.  Select the region.
3.  In the upper right corner, click Old MER Scheduling to go to the Jobs page.
4.  In the left-side navigation panel, click Execution plan.
5.  On the right side of the execution plan, click **More** \> **Running log**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17880/154138479710574_en-US.jpg)

    -   **Execution order ID**: The sequence of execution for the corresponding record, indicating the ordinal position in the whole execution queue. For example, 1 stands for the first position of execution while n stands for the n-th position.
    -   **Running Status**: The running status of each execution record.
    -   **Start Time** The time when the execution plan starts to run.
    -   **Running Time**: The total running time until the page is viewed.
    -   **Execute cluster**: The cluster run by the execution plan, can be either a cluster on demand or an existing associated cluster. Click to view the cluster details page.
    -   **Operation**

        **View job list**: Click this button to enter the job list page.


## View job records {#section_ycd_jzp_y2b .section}

On the**Job list** page, you can view the job list in execution records of a single execution plan and details of each job.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17880/154138479710575_en-US.jpg)

-   **Job execution order ID**: A corresponding ID will be created after a job is executed, and this ID is different from the job ID. The job execution ID is the unique record identifier to view the logs on OSS.

-   **Name**: The name of the job.

-   **Status**: The running status of the job.

-   **Type**: The type of job.

-   **Start time**: The time when the job starts to run. It has been converted into local time.

-   **Running time**: The total running time \(in seconds\) of this job.

-   **Operation**

    -   **Stop job**: A job can be stopped if it is in the process of submission or running. If a job is in submission, stopping it will cancel execution. If the job is running, it will be killed.
    -   **stdout**: Records all output content from standard output \(that is, Channel 1\) of the master process. If log saving for the cluster where jobs are run is not enabled, this viewing function cannot be executed.
    -   **stderr**: Records all output content from diagnostic output \(that is, Channel 2\) of the master process. If log saving for the cluster where jobs are run is not enabled, this viewing function cannot be executed.
    -   **Workers log**: To view the log of all job worker nodes. If log saving for the cluster where jobs are run is not enabled, this viewing function cannot be executed.

## View job worker logs {#section_j2f_kzp_y2b .section}

-   **Cloud server instance IP**: The ECS instance ID of a running job and the corresponding intranet IP address.

-   **Container ID**: The container ID of Yarn running.

-   **Type**: Different log types. stdout and stderr come from different outputs.

-   **Operation**

    **View the log**: Click different types to view the corresponding logs.


