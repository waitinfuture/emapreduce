---
keyword: [Fair Scheduler, fair]
---

# Use Fair Scheduler

Fair Scheduler is a built-in scheduler in Apache YARN. It is used to evenly allocate resources to applications running on YARN. The resources used by each queue are allocated based on the configured weight.

## Enable Fair Scheduler

**Note:** After you turn on Enable Resource Queue, you can no longer configure data on the **fair-scheduler** tab in the Service Configuration section of the **Configure** tab on the YARN service page. Existing configurations are synchronized to the **Cluster Resources** page. If you want to configure cluster resources on the **Configure** tab of the YARN service page, turn off Enable Resource Queue on the **Cluster Resources** page.

1.  Log on to the [EMR console](https://emr.console.aliyun.com/).

2.  In the top navigation bar, select the region where your cluster resides. Select the resource group as required. By default, all resources of the account appear.

3.  Click the **Cluster Management** tab.

4.  On the **Cluster Management** page that appears, find the target cluster and click **Details** in the Actions column.

5.  In the left-side navigation pane of the Cluster Overview page, click **Cluster Resources**.

6.  On the **Cluster Resources** page, turn on **Enable Resource Queue**.

7.  Select **Fair Scheduler**.

8.  Click **Save**.


## Configure Fair Scheduler

After Enable Resource Queue is turned on, perform the following steps to configure Fair Scheduler:

1.  In the upper-right corner of the **Cluster Resources** page, click **Queue Settings**.

2.  Configure queue information on the **Queue Settings** tab.

    Find your queue, and click **More** and then **Create Child Queue** in the Actions column to create a child queue.

    **root** is a level-1 queue. It is the parent queue of all other queues and manages all resources of YARN. By default, only the default queue is available under the root queue.

    **Note:** Parameters of a parent queue have higher priority than those of its child queues because the queue configurations are nested. If the resource usage configured for a child queue is higher than that of the parent queue, the scheduler allocates resources to the child queue based on the parameter settings of the parent queue.

    -   **Queue Name** is required. A queue name cannot contain periods \(.\).
    -   **Weight** is required. The scheduler evenly allocates resources to queues if the weight threshold is reached. Weights take effect on level-2, level-3, and lower-level queues. For example, a parent queue has a child queue with a weight of 2 and another with a weight of 1. If both of the two child queues have running tasks and the resource allocation ratio reaches 2:1, resources are evenly allocated.
    -   **Maximum Resources** indicates the maximum number of vCores and the maximum memory space that can be allocated to a queue. The value of this parameter must be greater than that of **Minimum Resources** but less than the resource scale that the YARN service can provide. If the value of Maximum Resources is greater than the resource scale, the resource scale takes effect for the queue. Assume that the number of vCores that the YARN service can provide is 16, but the value of vCore for Maximum Resources is 20. The 16 vCores are allocated to the queue.
    -   If you do not specify a queue during the running of applications, jobs are submitted to the default queue.
    -   If you do not restart ResourceManager after you modify the name of a child queue, tasks can still be submitted to the original queue. However, the queue configuration is no longer displayed in the EMR console. After you restart ResourceManager, the original queue becomes unavailable.
    -   After you delete a queue that is not a level-2 queue, click **Deploy** to validate the modification. After you delete a level-2 queue, select **RestartResourceManager** from the **Actions** drop-down list in the upper-right corner to validate the modification.

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

You must use **mapreduce.job.queuename** to specify the queue to which you want to submit a job. Example:

```
`hadoop jar /usr/lib/hadoop-current/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.8.5.jar pi -Dmapreduce.job.queuename=test  2 2`
```

