# Execution plan list {#concept_b2w_dwp_y2b .concept}

Displays basic information of all your execution plans.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17879/154138478110569_en-US.jpg)

-   **ID/Name**: The ID and corresponding name of the execution plan.
-   **Last run cluster**: The last cluster to execute this execution plan. It is a cluster created on demand or an existing associated cluster. If a cluster is created automatically on demand, \(Automatically created\) will be displayed below the cluster, indicating that the cluster is created automatically by E-MapReduce on demand and will be released automatically after running.
-   **Last run**: The running status of the last execution plan.
    -   **Start time**: The time from which the last plan starts to run.
    -   **Running time**: The duration for which the last plan is running.
    -   **Running status**: The running status of the last execution plan.
-   **Scheduling status**: Whether the scheduling is in progress or has been stopped. Only periodic jobs have the scheduling status.
-   **Operation**
    -   **Manage**: View and modify execution plans.
    -   **Run now**: Manual run can be made only when the job is neither running nor being scheduled. Click it to run the execution plan immediately.
    -   **More**
        -   **Start/Stop scheduling**: When the scheduling is stopped, Enable scheduling will appear which can be clicked to start scheduling. When Stop scheduling is displayed during scheduling, clicking it will stop the scheduling. This button is only for periodic execution plans.
        -   **Running log**: Click to enter the job log viewing page.
        -   **Delete**: Deletes an execution plan. The execution plan in the process of scheduling or running cannot be deleted.

