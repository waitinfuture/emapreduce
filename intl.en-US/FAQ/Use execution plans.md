# Use execution plans {#concept_enq_hz1_pfb .concept}

## Apply for high-configuration instances {#section_ctv_3z1_pfb .section}

If you have not activated a high-configuration instance, an error will occur when you use high-configuration instances to create a cluster, and the following error message appears:

The specified InstanceType is not authorized for usage.

Click [Here](https://workorder-intl.console.aliyun.com/#/ticket/createIndex) to submit a ticket and activate high-configuration instances.

## Use security groups {#section_acr_dbb_pfb .section}

You need to use security groups that are created in EMR when creating clusters in EMR. This is because only port 22 of the cluster in EMR is accessible. We recommend that you sort your existing instances into different security groups based on their functions. For example, the security group of EMR is "EMR-security group" and you can name your existing security group "User-security group." Each security group applies its own access control based on your needs. If it is necessary to bind the security groups with the cluster that has been created, follow these steps:

-   Add an EMR cluster to the existing security group

    Click **Details**. Security groups related to all ECS instances are displayed. In the ECS console, click the Security Group tab in the lower-left corner, find the security group "EMR-security group". Click **Manage Instance**. ECS instance names starting with emr-xxx are displayed. These are the corresponding ECS instances in the EMR cluster. Select all of these instances, and click **Move to Security Group** in the upper-right corner to move these instances to another security group.

-   Add the existing cluster into the "EMR-security group"

    Find the security group in which the existing cluster is located. Repeat the preceding operations, and move the cluster to the "EMR-security group." Select the instances that are not used by the cluster in the ECS console and move them to the "EMR-security group" by using the batch operations.

-   Rules of security groups

    The security group rules are subject to the OR relationship when an ECS instance is in several different security groups. For example, only port 22 of EMR security is accessible while all ports of "User-security group" are accessible. When an EMR cluster is added into "User-security group", all ports of instances in EMR open are accessible. Note the following rule:

    **Note:** When setting up security group rules, make sure that you restrict access by IP address range. Do not set the IP range to 0.0.0.0 to avoid attacks.


## Execution plan FAQs {#section_dmq_cxb_pfb .section}

-   Edit an execution plan.

    You can edit execution plans that are not in the running or scheduling status. If you cannot click the edit button, confirm the status of the execution plan and try again.

-   Run an execution plan.

    If you set the scheduling mode to Execute immediately when creating an execution plan, the plan is automatically executed after it is created. If it is an existing execution plan, you need to manually run the execution plan. The execution plan is not immediately run after creation.

-   Periodical execution time.

    The start time of a periodical execution cluster indicates the time when the execution plan starts to run. The time is accurate to minutes. The schedule cycle indicates the interval between two executions since the start time. As shown in the following example:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18053/154451496514368_en-US.png)

    The first run is at 14:30:00, December 01, 2015 and the second run is at 14:30:00, December 02, 2015. The execution plan is run once a day.

    If the current time is later than the time you have scheduled, then the latest time for scheduling is 14:30:00, December 01, 2015.

    Example:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18053/154451496514370_en-US.png)

    If the current time is 09:30, December 02, 2015, then the latest time for scheduling is 10:00:00, December 02, 2015, which is based on the scheduling rule. The first run starts at this time.


