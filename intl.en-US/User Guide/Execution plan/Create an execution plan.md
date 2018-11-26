# Create an execution plan {#concept_trx_1wp_y2b .concept}

An execution plan is a set of jobs that can be executed once or periodically through scheduling configuration. It can be executed on an existing E-MapReduce cluster and also can create a temporary cluster to execute assignment dynamically. Its biggest advantage is to use resources actually needed during execution to maximize resource savings.

## To create an execution plan, follow these steps: {#section_rvz_2wp_y2b .section}

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://emr.console.aliyun.com/).
2.  Select the region.
3.  In the upper right corner, click Old MER Scheduling to go to the Jobs page.
4.  In the left-side navigation panel, click Execution plan.
5.  In the upper right corner, click **Create an execution plan**.
6.  In the Create an execution plan page, select one mode from **Create as needed** and **Existing clusters**.
    1.  **Create as needed**: Create a new cluster to run jobs.
        -   Execution plan for one-time scheduling: Clusters with corresponding configuration will be created when execution starts and then released upon the completion of the operation. For specific descriptions of creation parameters, see [Create a cluster](../../../../intl.en-US/Quick Start/Create E-MapReduce/Create a cluster.md#).
        -   Execution plan for periodic scheduling: A new cluster will be created as per users’ settings when each scheduling period starts and then released upon the completion of the operation.
    2.  **Existing clusters**: Use an existing cluster that complies with the following requirement:

        -   Execution plans can only be submitted to clusters in **Running** or **Idle** status.
        Select the **Existing clusters** and then enter the Select Cluster page. You can select the cluster to associate with the execution plan.

7.  Click **Next** to enter the job configuration page. All user jobs will be listed in the left table, and you can select jobs for execution. By clicking the right-facing button, the checked jobs will be added into the job queue. Jobs in the queue will be submitted to the cluster for execution as per their order. The same job can be added and executed several times. If you have not created any jobs, see operating instructions to create jobs.
8.  Click **Next** to enter the scheduling mode configuration page. The configuration items are described as follows:
    1.  **Name**: Must be between 1-64 characters and consist of only Chinese characters, letters, numbers, “-“, and “\_”.
    2.  **Scheduling policy**
        -   **Manual execution**: The execution plan will not be automatically executed after creation, it must be manually executed. Once the execution is in progress, it cannot be conducted again.
        -   **Periodic scheduling**: This function will be enabled immediately after the execution plan is created. The execution can begin from the configured scheduling time point. Periodic scheduling can be disabled in the list page. If a scheduling execution starts, but its last scheduling execution has not completed, this scheduling will be ignored.
    3.  **Set the scheduling cycle**: There are two scheduling periods: days and hours. The ‘day’ cycle is ‘one day’ by default and remains unchanged. However, for the hour parameter, you can set the specific time interval and the range must be 1-23.
    4.  **First execution time**: The effective start time of scheduling. From this time on, periodic scheduling will be conducted as per scheduling periods. The first scheduling will be conducted from the latest time point when requirements are met as per actual time.
9.  Click **OK** to complete the creation of the execution plan.

## Others {#section_j2z_hxp_y2b .section}

-   Example for periodic scheduling

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17877/154157859310565_en-US.png)

    These configurations indicates that the scheduling is initially started on 11/03/2016, 12:15 and then conducted every other day. The second scheduling is conducted on 11/04/2016, 12:15.

-   Execution sequence of jobs

    For jobs in the execution plan, they will be executed from first to last as per the sequence of user-selected jobs in the job List.

-   Execution sequence of multiple execution plans

    Each execution plan can be deemed as an integral whole. When multiple execution plans are submitted to the same cluster, each execution plan submits jobs from its internal job sequence, which is consistent with the sequence of a single execution plan. While jobs among multiple execution plans are in parallel.

-   Practical example for early job debugging

    During job debugging, if the cluster is created on demand automatically, the speed will be slow and will take a long time to start the cluster. We recommend that you manually create a cluster first, and then select “Associate the cluster” in the execution plan to run jobs, and then to set the scheduling mode as Execute immediately. During debugging, results are viewed by clicking Run on the execution plan list page. Modify the execution plan once job debugging is completed, and then modify the mode of associating existing clusters into creating a new cluster on demand. Then modify the scheduling mode into periodic scheduling as needed. Tasks will be executed automatically on demand.


