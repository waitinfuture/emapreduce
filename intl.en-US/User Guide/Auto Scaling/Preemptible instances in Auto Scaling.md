# Preemptible instances in Auto Scaling {#concept_g5t_wgn_qfb .concept}

E-MapReduce [preemptible instances](../../../../../reseller.en-US/Product Introduction/Instances/Preemptible instance.md#) are suitable for scenarios where there is no requirement for the successful execution of big data jobs and where the cost of computing resources plays an important role. By using Auto Scaling, you can purchase preemptible instances to increase the computing resources of your clusters.

## Enable Auto Scaling {#section_snw_gjn_qfb .section}

To enable Auto Scaling and set scaling rules, complete the following steps:

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://partners-intl.console.aliyun.com/#/emr).
2.  Click Cluster Management.
3.  Find the cluster you want to add a preemptible instance to and click **Manage**.
4.  In the navigation pane on the left, click **Scaling**.
5.  Click **Enable Auto Scaling**.
6.  Configure scaling rules. For more information, see [Configure Auto Scaling by time](reseller.en-US/User Guide/Auto Scaling/Configure Auto Scaling by time.md#).
7.  In the scaling configuration area, select **Preemptible instance**.

## Configure a preemptible instance {#section_wsz_r3s_qfb .section}

**Note:** Preemptible instances are more cost-effective than Pay-As-You-Go instances. However, Alibaba Cloud may release your preemptive instances at any time based on changes in demand resources or market rates.

To configure your preemptible instance, complete the following steps:

1.  Select the vCPU and memory for your instance.
2.  Select instance types. You can select up to three instance types. E-MapReduce filters out all other instance types to ensure that you purchase a preemptible instance that meets your requirements.
3.  After you select the instance types, click the maximum price of each type and click **OK**. The instance types appear in the selected instances list. If you want to modify the price of a selected instance type, select the target one in the selected instances list and change the price \(by hour\). Your instance will run when your bid is higher than the current market rate. Your final instance type is billed at the market rate.
4.  The system disk is used for deploying basic services such as the OS and EMR, which are set by default. You can set the data disk type and size according to your requirements.
5.  The final configuration price includes the maximum bid price, system disk price, and data disk price. Click **Save**.

For more information about preemptible instances, see [FAQs](https://partners-intl.aliyun.com/help/faq-detail/48269.htm).

