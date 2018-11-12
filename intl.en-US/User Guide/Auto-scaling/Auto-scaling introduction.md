# Auto-scaling introduction {#concept_xhf_cdf_z2b .concept}

This article will introduce how to enable and disable the scaling feature.

In the following scenarios, you can save cost and improve execution efficiency through E-MapReduce scaling.

-   Add computing nodes according to the time period to supplement computing capability temporarily.
-   Make sure that important jobs are completed on time, and expand computing nodes according to certain cluster indicators.

**Note:** 

-   The scaling feature can only expand or reduce the number of task nodes.
-   The scaling feature is only available in Subscribed and Pay-As-You-Go clusters.

## Enable auto-scaling {#section_qp1_vhj_z2b .section}

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://emr.console.aliyun.com/console) and enter the **Cluster Management** page.
2.  At the right side of the target cluster ID, click the **Manage**.
3.  In the left-side navigation pane, click the **Scaling** to enter the auto-scaling page.
4.  In the top right corner of the page, click the **Enable Auto-scaling** button.

    If it is the first time to use the auto-scaling function with your account, you need to authorize the default role of Elastic Scaling Service\(ESS\) to your E-MapReduce account.

5.  Click **Confirm** on the ESS authorization page.

## Disable auto scaling {#section_w1h_33j_z2b .section}

After you click the **Disable Auto-scaling** button, all task nodes expanded by auto-scaling will be released. The data stored in HDFS is located in a core node and will not be affected.

