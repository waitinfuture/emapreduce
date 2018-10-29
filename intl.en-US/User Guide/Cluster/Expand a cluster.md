# Expand a cluster {#concept_jl4_d4n_y2b .concept}

If your cluster resources \(computing and storage resources\) are insufficient, the cluster can be expanded horizontally. Only core and task nodes can be expanded. Configurations of expanding a cluster default to be consistent with the ECS instance purchased previously.

## Expansion entry {#section_yjf_h4n_y2b .section}

Select the cluster to be expanded on the cluster list page, click **More**, and select **Scale Up/Out**. You can also click **View Details** on the right side of the cluster, and click **Scale Up/Out** in the upper right corner.

## Expansion interface {#section_zjf_h4n_y2b .section}

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17854/154080520610431_en-US.png)

**Note:** Only expansion is supported. Reduction is not supported.

-   **Configuration**: Displays configurations of the current instance.
-   **Billing Method**: Displays the payment method of the current cluster.
-   **Current Core Instances**: Displays the quantity of all your current core nodes.
-   **New Instances**: Enter the quantity that you want to add. We recommend that you perform small-scale expansions each time, such as increasing one or two instances.
-   VSwitch: Displays the VSwitch of the current cluster.

## Expansion status {#section_ckf_h4n_y2b .section}

The figure of cluster expansion statuses is shown as follows:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17854/154080520610432_en-US.jpg)

To view the expansion status of a cluster, in the **Cluster Overview** panel, go to the **Core Instance Group \(CORE\)** area. The node that is being expanded is displayed as **Scaling Up/Out**. When the status of an ECS instance changes to **Normal**, the ECS has been added into the cluster and can provide services normally.

