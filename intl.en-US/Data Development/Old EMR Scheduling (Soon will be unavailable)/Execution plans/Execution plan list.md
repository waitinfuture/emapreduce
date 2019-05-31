# Execution plan list {#concept_b2w_dwp_y2b .concept}

An execution plan list displays basic information about all of your execution plans, as shown in the following figure.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17879/155928624410569_en-US.jpg)

-   **ID/Name**: The ID and name of the execution plan.
-   **Last run cluster**: The last cluster to execute this execution plan. This can either be a cluster created on demand or an existing associated cluster. If a cluster is created automatically on demand, \(Automatically created\) is displayed beneath it, indicating that the cluster was created on demand by E-MapReduce and will be released automatically after running.
-   **Last run**: The running status of the last execution plan.
    -   **Start time**: The time at which the last execution plan started.
    -   **Running time**: The duration for which the last plan ran.
    -   **Running status**: The running status of the last execution plan.
-   **Scheduling status**: This indicates whether scheduling is in progress or has been stopped. Only periodic jobs have a scheduling status.
-   **Operation**
    -   **Manage**: View and modify execution plans.
    -   **Run now**: A job can only be run manually when it is neither running nor being scheduled. Click **Run now** to run the execution plan immediately.
    -   **More**
        -   **Start/Stop scheduling**: If the scheduling is stopped, **Enable scheduling** is displayed, which you can click to start the scheduling. If **Stop scheduling** is displayed during scheduling, you can click it to stop the scheduling. This button is only available for periodic execution plans.
        -   **Running log**: Click to enter the job log viewing page.
        -   **Delete**: Deletes an execution plan. A running execution plan or one in the process of scheduling cannot be deleted.

