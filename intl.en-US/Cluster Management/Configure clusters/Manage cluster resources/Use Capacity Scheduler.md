---
keyword: [Capacity Scheduler, capacity]
---

# Use Capacity Scheduler

Capacity Scheduler is a built-in scheduler in Apache YARN. The YARN service deployed in an E-MapReduce \(EMR\) cluster uses Capacity Scheduler as the default scheduler. Capacity Scheduler is a multi-tenant, hierarchical resource scheduler. Resources used by each child queue are allocated based on the configured capacity.

## Enable Capacity Scheduler

**Note:** After you turn on Enable Resource Queue, you can no longer configure data on the **capacity-scheduler** tab in the Service Configuration section of the **Configure** tab on the YARN service page. Existing configurations are synchronized to the **Cluster Resources** page. If you want to configure cluster resources on the **Configure** tab of the YARN service page, turn off Enable Resource Queue on the **Cluster Resources** page.

1.  Log on to the [EMR console](https://emr.console.aliyun.com/).

2.  In the top navigation bar, select the region where your cluster resides. Select the resource group as required. By default, all resources of the account appear.

3.  Click the **Cluster Management** tab.

4.  On the **Cluster Management** page that appears, find the target cluster and click **Details** in the Actions column.

5.  In the left-side navigation pane of the Cluster Overview page, click **Cluster Resources**.

6.  On the **Cluster Resources** page, turn on **Enable Resource Queue**.

7.  Click **Save**.

    Capacity Scheduler is enabled.


## Configure Capacity Scheduler

After Enable Resource Queue is turned on, perform the following steps to configure Capacity Scheduler:

1.  In the upper-right corner of the **Cluster Resources** page, click **Queue Settings**.

2.  Configure queue information on the **Queue Settings** tab.

    -   Find your queue, and click **Edit** in the Actions column to modify resource queue information.
    -   Find your queue, and click **More** and then **Create Child Queue** in the Actions column to create a child queue.
    **root** is a level-1 queue. It is the parent queue of all other queues and manages all resources of YARN. By default, only the default queue is available under the root queue.

    **Note:**

    -   The sum of Capacity values set for all child queues at the same level under the same parent queue must be 100. For example, two child queues default and department are available under the root queue. The sum of Capacity values set for these two child queues must be 100. Child queues market and dev are available under the department queue. The sum of Capacity values set for these two child queues must also be 100.
    -   If you do not specify a queue during the running of applications, jobs are submitted to the default queue.
    -   After you create a level-2 queue under root, you only need to click **Deploy** for the configuration to take effect.
    -   After you create or modify a level-3 queue under root, you must restart ResourceManager for the configuration to take effect.
    -   You must restart ResourceManager after you modify the name of a queue.

## Switch the scheduler type

After Enable Resource Queue is turned on, perform the following steps to switch the scheduler type:

1.  In the upper-right corner of the **Cluster Resources** page, click **Select Scheduler**.

2.  Select the required scheduler.

3.  Click **Save**.

4.  Select **RestartResourceManager** from the **Actions** drop-down list in the upper-right corner.

5.  In the **Cluster Activities** dialog box, specify related parameters and click **OK**.

6.  In the **Confirm** message, click **OK**.

    When a success message appears, the scheduler type is switched.


## Submit a job

-   If you do not specify a queue during the running of applications, jobs are submitted to the default queue.
-   You must specify a child queue. Tasks cannot be submitted to the parent queue.
-   You must use **mapreduce.job.queuename** to specify the queue to which you want to submit a job. Example:

    ```
    `hadoop jar /usr/lib/hadoop-current/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.8.5.jar pi -Dmapreduce.job.queuename=test  2 2`
    ```


