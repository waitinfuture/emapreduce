# Auto-scaling preemptible instances {#concept_g5t_wgn_qfb .concept}

E-MapReduce [Preemptible instances](../../../../intl.en-US/Product Introduction/Instances/Preemptible instance.md#) are suitable for scenarios where there is no requirement for the successful execution of big data jobs and where the price of computing resources is important. You can purchase preemptible instances to increase the computing resources of clusters using auto scaling.

## Enable auto scaling {#section_snw_gjn_qfb .section}

To enable the auto scaling feature and set scaling rules, follow these steps:

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://emr.console.aliyun.com/).
2.  Click Cluster Management.
3.  Find the cluster where you want to add preemptible instances and the click **Manage**.
4.  In the left-side navigation pane, click **Scaling**.
5.  Click **Enable Auto Scaling**.
6.  Configure scaling rules. For more information, see [Configure auto scaling according to time](intl.en-US/User Guide/Auto-scaling/Configure auto-scaling according to time.md#).
7.  In scaling configuration area, select **Preemptible instance**.

## Configure preemptible instances {#section_wsz_r3s_qfb .section}

**Note:** The price of preemptible instances is more favorable than Pay-As-You-Go instances, but Alibaba Cloud may release your instances at any time based on changes in demand resources or market transaction prices.

To configure your preemptible instance, follow these steps:

1.  Select the vCPU and memory for your instance.
2.  Select instance types. You can select up to three instance types. E-MapReduce filters out all other instance types to ensure that you purchase a preemptible instance that meets your requirements.
3.  After you select instance types, click the maximum price of each instance type, and then click **OK**. The instance types will appear in the selected instances list. If you want to modify the price of a selected instance type, select the instance type in the selectable instances list and change the price \(by hour\). Your instance will run when your bid is higher than the current market price. Your final instance type will be billed at the market price.
4.  The system disk is used for deploying basic services such as the OS and EMR, which are set by default. You can set the data disk type and size according to your needs.
5.  The final configuration price includes the maximum bid price, system disk price and data disk price. Click **Save**.

For more information about preemptible instances, see [FAQ about preemptible instances](https://www.alibabacloud.com/help/zh/faq-detail/48269.htm).

