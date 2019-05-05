# Configure Auto Scaling by time {#concept_njp_r3j_z2b .concept}

If the computing capability of a Hadoop cluster sees significant peaks and troughs over a specified period of time, you can set a fixed time frame within which a certain number of task nodes supplement the computing capability. This not only ensures that jobs are completed, but it also saves you money.

The expansion nodes are billed in Pay-As-You-Go mode. However, for the same computing capability, the price ratio between Pay-As-You-Go and Subscription modes is around 3:1. Therefore, it is necessary to design a ratio for both modes based on the time needed. For example, if there are 8 peak business hours a day, the price for Pay-As-You-Go is essentially the same as that for Subscription. If peak hours last longer than 8 hours, the Subscription mode is more cost-effective than Pay-As-You-Go.

## Configure the number of scaling instances {#section_j3f_t3j_z2b .section}

-   Maximum number of nodes: The maximum number of nodes that can be expanded. Once this number is reached, even if the Auto Scaling rule is met, expansion and contraction will stop. Currently, you can set up to 1,000 task nodes.
-   Minimum number of nodes: The minimum number of nodes that can be expanded. If the number of task nodes set in the Auto Scaling rule is less than this minimum number of nodes, the cluster scales based on the minimum number of nodes at the first execution.

    For example, if the Auto Scaling rule is set to expand 1 node at 00:00:00 every day and the minimum number of nodes is 3, then the system expands 3 nodes at the 00:00:00 on the first day.


## Configure scaling rules {#section_zqs_s3j_z2b .section}

Auto Scaling rules include expansion rules and contraction rules. When Auto Scaling is disabled, all rules are cleared. If it is enabled again, the scaling rules need to be reconfigured.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17958/155704114510971_en-US.png)

-   **Rule Name**: In a cluster, the scaling rule names \(including expansion rules and contraction rules\) cannot be repeated.
-   Execution cycle:
    -   **Run Once**: The cluster performs a scaling operation at a specified time.
    -   **Repeat**: You can choose to perform a scaling operation at a specific time every day, every week, or every month.
-   **Retry Timeout**: When the specified time is reached, scaling cannot be performed. By setting the retry timeout, the system can detect that scaling can be performed every 30 seconds in the time range. Once the condition is met, scaling is performed. The range is 0 to 21600 seconds.

    It is assumed that if expansion A needs to be performed in the specified time period, but expansion B is being performed or the cluster is in **cool-down time**, expansion A cannot be performed. During the retry timeout you set, the system detects that expansion A can be performed every 30 seconds. Once the conditions are met, the cluster immediately performs scaling.

-   **Increase Task Nodes**: The number of task nodes to be increased or decreased by the cluster each time the rule is triggered.
-   **Cool-down Time**: The interval between a scaling operation being completed and the same operation being performed again. Scaling operations are not performed during cool-down time.

## Configure scaling instance specifications {#section_drs_s3j_z2b .section}

When Auto Scaling is enabled, you can specify the hardware specifications for the scaling nodes. The specifications cannot be modified after the configuration is saved. If you need to modify them, you can disable Auto Scaling and then enable it again.

-   When you select specifications for vCPU and memory, the system automatically matches the instances that meet the criteria and displays them in the instance list below. For the cluster to be able scale according to the selected instance specifications, you need to add an optional instance to the list on the right.
-   To avoid scaling failures caused by insufficient ECS instance storage, you can choose up to 3 ECS instance types.
-   Regardless of whether you choose an efficient cloud disk or a SSD cloud disk, the data disk is set to a minimum of 40G.

