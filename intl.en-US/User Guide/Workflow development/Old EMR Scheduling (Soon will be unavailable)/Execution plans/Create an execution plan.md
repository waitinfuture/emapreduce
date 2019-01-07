# Create an execution plan {#concept_trx_1wp_y2b .concept}

An execution plan is a set of jobs that can be executed either at one time or periodically by means of scheduling. It can be executed on an existing E-MapReduce cluster and can also create a temporary cluster to execute the jobs dynamically. Its biggest advantage is that it only uses the resources it needs during execution.

## Procedure {#section_rvz_2wp_y2b .section}

To create an execution plan, follow these steps:

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://emr.console.aliyun.com/).
2.  Select a region.
3.  In the upper-right corner, click Old MER Scheduling to go to the Jobs page.
4.  In the navigation panel on the left, click Execution plan.
5.  In the upper-right corner, click **Create an execution plan**.
6.  In the **Create an execution plan** page, select between **Create as needed** and **Existing clusters**.
    1.  **Create as needed**: Create a new cluster to run jobs.
        -   Execution plan for one-time scheduling: Clusters with corresponding configurations are created when the execution starts and are then released upon completion of the operation. For more information about creation parameters, see [Create a cluster](../../../../../intl.en-US/Quick Start/Create E-MapReduce/Create a cluster.md#).
        -   Execution plan for periodic scheduling: A new cluster is created based on the scheduling settings you define and is then released upon completion of the operation.
    2.  **Existing clusters**: Use an existing cluster that complies with the following requirement:

        -   Execution plans can only be added to clusters that are **Running** or **Idle**.
        Select **Existing clusters** and then enter the **Select Cluster** page. Here, you can select a cluster to associate with the execution plan.

7.  Click **Next** to enter the job configuration page. All user jobs are listed in the table on the left. You can select jobs for execution from this table. By clicking the right-facing button, the checked jobs are added to the job queue. Jobs in the queue are then submitted to the cluster for execution in order. The same job can be added and executed several times. If you have not created any jobs, see [Jobs](../../../../../intl.en-US/Developer Guide/API reference/Old EMR Scheduling/Job/CreateJob.md#).
8.  Click **Next** to enter the scheduling mode configuration page. The configuration items are as follows:
    1.  **Name**: Must be between 1-64 characters and may only consist of Chinese characters, English letters, numbers, hyphens \(-\), and underscores \(\_\).
    2.  **Scheduling policy**
        -   **Manual execution**: The execution plan is not executed automatically after it is created. Instead, it must be executed manually. Once the execution is in progress, it cannot be executed again.
        -   **Periodic scheduling**: If you select this function, it is enabled immediately after the execution plan is created. The execution then begins from the configured scheduling time. Periodic scheduling can be disabled in the list page. If a scheduling execution starts, but its last execution is not completed, the scheduling is ignored.
    3.  **Set the scheduling cycle**: There are two scheduling periods: days and hours. The day cycle is one day by default and cannot be changed. However, you can set a specific time interval for hours. The range must be 1-23.
    4.  **First execution time**: The effective start-time of the scheduling. From this point onwards, periodic scheduling is conducted according to the intervals specified.
9.  Click **OK** to complete the creation of the execution plan.

## Other information {#section_j2z_hxp_y2b .section}

-   Example of periodic scheduling

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17877/154684739210565_en-US.png)

    These configurations indicate that the scheduling started on 11/01/2018 at 17:35 with an interval of one day. This means that the second scheduling was conducted on 11/02/2018, at 17:35.

-   Sequence of jobs

    Jobs in the execution plan are executed from first to last according to the sequence that you defined in the job list.

-   Sequence of multiple execution plans

    When multiple execution plans are submitted to the same cluster, each one submits jobs from its own job sequence. This means that jobs run parallel with each other.

-   Example of early job debugging

    During the debugging of a job, it may take some time to create and start a cluster on demand. We recommend that you create a cluster manually first, select **Associate the cluster** in the execution plan to run jobs, and then set the scheduling mode to **Execute** immediately. During debugging, you can view the results by clicking **Run now** on the execution plan list page. Once the debugging is finished, modify the execution plan, modify the way you associate an existing cluster to create a new cluster on demand, and then modify the scheduling mode to periodic scheduling as required. Jobs are then executed automatically on demand.


