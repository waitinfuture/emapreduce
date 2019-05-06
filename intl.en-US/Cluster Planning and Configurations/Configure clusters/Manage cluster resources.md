# Manage cluster resources {#concept_tqs_whd_5gb .concept}

EMR \(E-MapReduce\) cluster resource management applies to scenarios that involve large-scale clusters with many tenants.

With EMR cluster resource management, the following features are supported.

-   Different resource queues are used based on the users. This helps to isolate resources.
-   Resource queues are elastic, which improves the efficiency of the cluster.

**Note:** 

-   Currently, you can choose [Capacity Scheduler](https://hadoop.apache.org/docs/r2.7.2/hadoop-yarn/hadoop-yarn-site/CapacityScheduler.html) or [Fair Scheduler](https://hadoop.apache.org/docs/r2.7.2/hadoop-yarn/hadoop-yarn-site/FairScheduler.html) for managing EMR cluster resources.
-   Currently, you can only manage resources of EMR Hadoop clusters.

## Enable resource queues {#section_if5_mjd_5gb .section}

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://partners-intl.console.aliyun.com/#/emr). Click **Cluster Management** to go to the Cluster Management page.
2.  Click **Manage** in the Actions column.
3.  In the left-side navigation pane, click **Cluster Resources**.
4.  Turn on **Enable Resource Queue**.

    **Note:** After turning on Enable Resource Queue, on the **Configure** tab page of YARN, capacity-scheduler and fair-scheduler sections are frozen. On the Cluster Resources page, the configurations are synchronized. You need to turn off Enable Resource Queue on the **Cluster Resources** page to configure clusters by using XML again on the **Configure** tab page of YARN.


## Configure resource queues {#section_vmm_zkd_5gb .section}

-   After turning on Enable Resource Queue, choose fair scheduler or capacity scheduler as the scheduler type.
-   Click **Queue Settings** to go to the Queue Settings page.
-   Click **Set Scheduler Defaults** to set the default configurations of the scheduler.

## Resource queues taking effect {#section_nlf_54d_5gb .section}

After configuring resources queues, click **Check Configuration** to preview the configured XML file and click **Deploy** to deploy the configurations.

**Note:** 

-   After switching the scheduler type \(for example, from Fair Scheduler to Capacity Scheduler\) or modifying parameters related to resource preemption in Fair Scheduler, you need to restart all YARN components on the Clusters and Services page before clicking Deploy.
-   After creating resource queues or setting related parameters, you only need to click Deploy.
-   You can view the deployment progress of configurations on the Clusters and Services page.

