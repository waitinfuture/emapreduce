# Manage execution plans {#concept_a1m_cwp_y2b .concept}

Manage execution plans

1.  Log on to the [Alibaba Cloud E-MapReduce Console Plan Execution Page](https://emr.console.aliyun.com/#/schedule/region/cn-hangzhou).
2.  Find corresponding execution plan items and click the **Manage**button to enter the execution plan management page. On this page you can:
    -   **View details of the execution plan**

        You can view the basic information of execution plans, such as names, associated clusters, job configurations, scheduling strategy and status, alarm information.

    -   **Modify the execution plan**

        **Note:** Jobs can be modified if they are not in the process of running or being scheduled. For an execution plan to be immediately executed, it can be modified only when it is not currently running. If the execution plan is periodically scheduled, wait for the completion of its current operation, verify whether it is in periodical scheduling, and click **Stop scheduling** \(if yes\) before modifying it.

        **Modify independently**

        Each separate module can be modified independently. Click the **Modify** button to perform modification.

    -   **Configure alarm notification**

        There are three types of alarm notifications:

        -   Notification for booting timeout: If a periodical scheduling has not been conducted properly at the specified time and is not executed within 10 minutes of timeout, an alarm will be sent.

        -   Notification for failed execution: If any job in the execution plan fails, an alarm will be sent.

        -   Notification for successful execution: If all jobs in the execution plan are successfully executed, a notification will be sent.

    -   Run and view results

        When the execution plan can be run, there will be a **Run** button to the right of **Scheduling status** in **Basic Information**. Once this button is clicked, a schedule will be executed.

        At the bottom of the page, there are running records displaying the execution plan instances executed each time, facilitating views of the corresponding job list and logs.


