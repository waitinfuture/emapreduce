# Configure auto-scaling according to time {#concept_njp_r3j_z2b .concept}

If there are significant peaks and troughs in a Hadoop cluster computing capability during a certain period of time, you can set up a fixed period of time to expand a certain number of task nodes to supplement the computing capability. This will not only ensure the completion of jobs, but also save you costs.

The expansion nodes are billed in Pay-As-You-Go mode, and the price of the same computing capability for Pay-As-You-Go mode and Subscription mode is about 3:1. So it is necessary to design the ratio of computing capability for the Pay-As-You-Go mode and Subscription mode respectively according to expansion time you need. For example, the peak period of business lasts for 8 hours a day, and the price paid by Pay-As-You-Go mode are roughly the same as that of Subscription mode. When the peak period is more than 8 hours, the Subscription mode are more favorable than the Pay-As-You-Go mode.

## Configure scaling instance number {#section_j3f_t3j_z2b .section}

-   Maximum number of nodes: The maximum number of the nodes that can be expanded. Once it is reached, even if the auto-scaling rule is matched, the expansion and contraction will not continue. Currently, you can set up to 1,000 task nodes.
-   Minimum number of nodes: The minimum number of the nodes that can be expanded. If the expansion or contraction number of task nodes set in the auto-scaling rule is less than the minimum number of nodes here, the cluster will scale with the minimum number of nodes at the first execution.

    For example, the auto-scaling rule is set to expand 1 node at 00:00:00 of every day, but the minimum number of nodes is 3. Then the system will expand 3 nodes at the 00:00:00 of the first day.


## Configure scaling rules {#section_zqs_s3j_z2b .section}

The auto-scaling rules include expansion rules and contraction rules. When the auto-scaling function is disabled, all rules are cleared. If auto-scaling function is enabled again, the scaling rules need to be reconfigured.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17958/154322361810971_en-US.png)

-   **Rule Name**: In a cluster, the scaling rule names \(including expansion rules and contraction rules\) are not allowed to be repeated.
-   Execution cycle:
    -   **Run Once**: The cluster performs a scaling operation at a specified time.
    -   **Repeat**: You can choose to perform a scaling operation at a specific time every day, every week or every month.
-   **Retry Timeout**: Auto-scaling may not be performed for various reasons when the specified time is reached. With the retry expiration time set, the system will detect the condition that scaling can be performed every 30 seconds in the time range. Once the condition is met, scaling is performed. The range is 0 to 21600 seconds.

    It is assumed that the expansion operation A needs to be performed in the specified time period, if another expansion operation B is performing or the cluster is in the cooling period at that time, the operation A cannot be performed. During the retry expiration time you set, the system will detect the condition that operation A can be performed every 30 seconds. Once the conditions are met, the cluster will immediately perform scaling.

-   **Increase Task Nodes**: The number of task nodes to be increased or decreased each time by cluster when the rule is triggered.
-   **Cool-down Time**: The interval between scaling operation is completed and the same operation can be performed again. No scaling operation will be performed during cooling time.

## Configure scaling instance specification {#section_drs_s3j_z2b .section}

You can specify the hardware specifications of the scaling nodes. It can only be configured when the auto-scaling function is enabled, and can not be modified after the configuration is saved. If it needs to be modified for a special case, you can disable the auto-scaling function and enable it again.

-   When you select specifications for vCPU and memory, the system automatically matches the instances that meet the criteria based on your selection and displays them in the instance list below. You need to add an optional instance to the list on the right so that the cluster can scale according to the selected instance specification.
-   To avoid scaling failures due to insufficient ECS stock, you can choose up to 3 ECS instance types.
-   Whether you choose an efficient cloud disk or a SSD cloud disk, the data disk is set to a minimum of 40G.

