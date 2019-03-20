# Multiple host groups {#concept_wyy_32c_5gb .concept}

When you use an EMR Hadoop cluster, you need to specify the instance types for the instance groups based on the real scenarios. For example:

-   You need some memory instances \(CPU: Mem = 1 vCore: 8 GiB\) to perform offline big data processing and some compute instances \(CPU: Mem=1 vCore: 2 GiB\) to train models.
-   You want to assign certain departments with memory instances \(CPU: Mem = 1 vCore: 8 GiB\) and compute instances \(CPU: Mem=1 vCore: 2 GiB\) based on their requirements.

You can use [Task instances](intl.en-US/User Guide/Configure clusters/Instance types.md#section_zwh_1l3_y2b) to create multiple host groups and select different configurations for the host groups to meet the preceding requirements. Procedure:

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://emr.console.aliyun.com/). Click **Cluster Management** to go to the cluster management page.
2.  Click **Clusters and Services** in the Actions column for the cluster that you want to create host groups for.
3.  In the left-side navigation pane, click **Cluster Overview**.
4.  On the Cluster Overview page, click **Resource Allocation**. From the Resource Allocation drop-down list, select **Scale Up/Out**.
5.  Click the TASK \(Task Instance Group\) tab, click **Add Host Group**.
6.  In the Host Group Name field, enter a name for the host group. Select the specifications as needed. A host group name is required to be unique for each cluster.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/124632/155306356538830_en-US.png)


**Note:** You can create a maximum of 10 host groups to categorize the task instances of an EMR Hadoop cluster.

