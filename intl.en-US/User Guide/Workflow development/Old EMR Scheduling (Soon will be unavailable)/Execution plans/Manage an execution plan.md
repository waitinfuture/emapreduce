# Manage an execution plan {#concept_a1m_cwp_y2b .concept}

You can view, manage, and modify your execution plans as follows.

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://emr.console.aliyun.com/).
2.  Select a region.
3.  In the upper-right corner, click Old MER Scheduling to go to the Jobs page.
4.  In the navigation panel on the left, click Execution plan.
5.  Click **Manage** next to a plan to go to the execution plan detail page. Here, you can perform the following operations:
    -   View details of the execution plan

        You can view the basic information of the execution plan, such as its name, associated clusters, job configurations, scheduling mode and status, and alarm information.

    -   Modify the execution plan

        **Note:** Jobs can only be modified if they are not currently running or being scheduled. For an execution plan to be executed immediately, it can only be modified when it is not currently running. If the execution plan is scheduled periodically, wait for the completion of its current operation and verify whether it is in periodical scheduling. If it is, click **Stop scheduling** before modifying it.

        Each separate module can be modified independently. Click the pen icon to modify information.

    -   Configure alarm notifications

        There are three types of alarm notifications:

        -   Booting timeout: If the periodical scheduling has not been conducted correctly at the specified time and is not executed within 10 minutes of timeout, an alarm is sent.
        -   Failed execution: If any job in the execution plan fails, an alarm is sent.
        -   Successful execution: If all jobs in the execution plan are executed successfully, a notification is sent.
    -   Run and view results

        If the execution plan can be run, in **Basic Information**, there will be a **Run now** button to the right of **Scheduling status**. If you click this button, a schedule will be executed.

        At the bottom of the page, there are running records displaying the execution plan instances executed each time, making it easy to view the corresponding job list and logs.


