# Execution plan list {#concept_b2w_dwp_y2b .concept}

Displays basic information of all your execution plans.

-   **Execution plan ID/name**: The ID and corresponding name of the execution plan.

-   **Recent Execution Cluster**: The last cluster to execute this execution plan. It is a cluster created on demand or an existing associated cluster. If a cluster is created automatically on demand, \(Automatically created\) will be displayed below the cluster, indicating that the cluster is created automatically by E-MapReduce on demand and will be released automatically after running.

-   **Last running condition**: The running status of the last execution plan.

    -   **Start time**: The time from which the last plan starts to run.

    -   **Running time**: The duration for which the last plan is running.

    -   **Running status**: The running status of the last execution plan.

-   **Scheduling status**: Whether the scheduling is in progress or has been stopped. Only periodic jobs have the scheduling status.

-   **Operation**

    -   **Run now**: Manual run can be made only when the job is neither running nor being scheduled. The execution plan will be run once immediately after clicking.

    -   **Enable/Stop scheduling**: When the scheduling is stopped, Enable scheduling will appear which can be clicked to start scheduling. When Stop scheduling is displayed during scheduling, clicking it will stop the scheduling. This button is only for periodic execution plans.

    -   **Running records**: Click to enter the job log viewing page in the execution plan.

    -   **More**

        -   **Modify**: To modify the configuration of an execution plan. The execution plan in the process of scheduling and running cannot be modified.

        -   **Delete**: To delete an execution plan. The execution plan in the process of scheduling or running cannot be deleted.


