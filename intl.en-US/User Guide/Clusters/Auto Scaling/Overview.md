# Overview {#concept_xhf_cdf_z2b .concept}

This topic describes how to enable and disable the Auto Scaling feature.

You can use EMR Auto Scaling to reduce costs and improve execution efficiency for the following scenarios.

-   You have a temporary need for adding nodes to enhance computing power.
-   You need to add nodes based on some cluster metrics to ensure that important jobs are completed on schedule.

**Note:** 

-   Auto Scaling only supports scaling in and scaling out a cluster by adding or removing task nodes.
-   Auto Scaling only supports Subscription and Pay-As-You-Go Hadoop clusters.
-   We recommend that you choose Scale by Time as the rule type if you can specify the time to scale a cluster.
-   We recommend that you choose Scale by Rule as the rule type if you cannot specify the time to scale a cluster and need to add and remove computing resources based on the specified YARN metrics.

## Enable Auto scaling {#section_qp1_vhj_z2b .section}

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://emr.console.aliyun.com/). Click the Cluster Management tab to go to the cluster management page.
2.  Click **Clusters and Services** in the Actions column for the cluster that you want to perform auto scaling for.
3.  In the left-side navigation pane, click **Scaling**.
4.  In the top right corner of the page, click **Enable Auto Scaling**.

    When using Auto Scaling through an Alibaba Cloud account for the first time, you need to assign an Auto Scaling default role to the EMR service.

5.  Click **OK** on the authorization page of Auto Scaling.

## Disable Auto Scaling {#section_w1h_33j_z2b .section}

Click **Disable Auto Scaling**. The task nodes that have been added through Auto Scaling are to be released. HDFS data that is stored on core nodes is not affected.

## Restrictions {#section_mgj_k2w_tgb .section}

You can only choose one scaling rule \(Scale by Time or Scale by Rule\) for Auto Scaling.

If you switch the scaling type, the original scaling rules are reserved but invalid and not to be triggered. Nodes that have been added for scaling out are not to be removed unless the rule for scaling in is triggered. \[DO NOT TRANSLATE\]

