# Auto-scaling Introduction {#concept_xhf_cdf_z2b .concept}

This article will introduce how to enable and disable Auto-scaling.

In the following scenarios, you can save cost and improve execution efficiency through E-MapReduce auto-scaling.

-   Add computing nodes according to the time period to supplement computing capability temporarily.
-   Make sure that important jobs are completed on time, and expand computing nodes according to certain cluster indicators.

**Note:** The auto-scaling function can only expand or reduce the number of Task node.

## Enable Auto-scaling {#section_qp1_vhj_z2b .section}

1.  Log on to the [Alibaba Cloud E-MapReduce Console](https://emr.console.aliyun.com/console) to enter the **Cluster List** page with primary account.
2.  Click the **Manage** link in the Operation column.
3.  Click the **Auto-scaling** on the left navigation pane to enter the Auto-scaling page.
4.  Click the **Enable Auto-scaling** button in the top right corner of the page.

    If the current account is the first time to use the auto-scaling function, you need to authorize the default role of Elastic Scaling Service\(ESS\) to E-MapReduce account. Click **Confirm** on the ESS authorization page.


## Disable Auto Scaling {#section_w1h_33j_z2b .section}

After you click **Disable Auto-scaling** button, all Task nodes expanded by auto-scaling will be released. The data stored in HDFS is located in the Core node and will not be affected.

