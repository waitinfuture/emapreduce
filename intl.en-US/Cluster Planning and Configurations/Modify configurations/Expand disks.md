# Expand disks {#concept_f3v_bw4_y2b .concept}

E-MapReduce \(EMR\) allows you to expand disks for EMR clusters. You can expand both system and data disks.

## Overview {#section_vmm_j0a_4ty .section}

According to the version of EMR that you are using and the type of disks that you need to expand, choose one of the following methods to expand disks:

-   Data disk
    -   EMR V3.11.0 and later: You can expand data disks in the EMR console. The current EMR console does not support shrinking data disks.
    -   Versions earlier than EMR V3.11.0: You can expand data disks in the Elastic Compute Service \(ECS\) console.
-   System disk
    -   All EMR versions: You can expand the system disk in the ECS console.

## Expand data disks \(EMR V3.11.0 and later\) {#section_bp2_hjc_wfb .section}

The current EMR console only supports expanding data disks. For more information about how to expand the system disk, see [Expand the system disk](#section_nzr_sy4_y2b).

**Note:** Before you expand a data disk, make sure that your account has sufficient balance. Fees for data disk expansion are automatically deducted from your account. If your account does not have sufficient balance, the expansion process is terminated.

1.  Log on to the [Alibaba Cloud EMR console](https://partners-intl.console.aliyun.com/#/emr) and then go to the **Cluster Management** page.
2.  On the Cluster Management page, click the ID of the target cluster in the **Cluster ID/Name** column. You can also perform this task on the Overview page.
3.  In the left-side navigation pane, choose Cluster Overview, and then choose **Change Configuration** \> **Disk Expansion** in the upper-right corner of the page.
4.  In the Disk Expansion dialog box, set the parameters.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17864/156747739132531_en-US.png)

    The required parameters are described as follows:

    |Parameter|Description|
    |---------|-----------|
    |**Instance Group**|Instance groups that support disk expansion include:     -   **MASTER \(Master instance group\)**
    -   **CORE \(Core instance group\)**
 |
    |**Select Node Group**|Select a node group from the **Select Node Group** list.|
    |**Billing Method**|Indicates the billing method that the selected EMR cluster is using.|
    |**Configure**|The size of each data disk for the selected instance group.|
    |**Scale To**|The size of the storage space that you want to expand each data disk to.|

5.  After you set the parameters, click **OK**.

    After you expand the disks, the **The disk is expanded. Restart the node group for the change to take effect.** message is displayed at the bottom of the Cluster Overview page for the instance group.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17864/156747739134253_en-US.png)

    **Note:** You must restart the cluster for the disk expansion to take effect. This operation restarts the ECS instances in the cluster. During the restart process, the big data services are unavailable. Make sure that the restart operation does not adversely affect your businesses before you restart the cluster.

6.  Click **The disk is expanded. Restart the node group for the change to take effect.** to configure the restart settings.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17864/156747739134254_en-US.png)

    The cluster restart settings are described as follows:

    |Parameter|Description|
    |---------|-----------|
    |**Rolling Start**|**Rolling restart**:     -   Select the **Rolling Restart** check box: The ECS instances in the cluster are restarted in sequence. EMR must complete restarting an ECS instance and recovering the big data services on the ECS instance before it can restart another ECS instance. It takes about five minutes to restart an ECS instance.
    -   Clear the **Rolling Restart** check box: EMR restarts all ECS instances at the same time.
 |
    |**Restart Scaled Nodes Only**|A scaled node is a node that has its disks or configuration upgraded, such as a master or core node. **Restart scaled nodes only**:     -   Select the **Restart Scaled Nodes Only** check box: Only scaled nodes are restarted. Other nodes are not restarted. For example, if you have expanded the disks of the nodes in a **core instance group**, then only the ECS instances in the **core instance group** are restarted.
    -   Clear the **Restart Scaled Nodes Only** check box: All nodes are restarted. This means that all ECS instances in the cluster are restarted.
 |

7.  After you set the parameters, click **OK** to restart the cluster.

    During the restart process, the instance group displays **Restarting server group...**. If the node group is successfully restarted, the **Restarting server group...** message disappears. You can go to the Cluster Overview page to verify the disk expansion result.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17864/156747739134255_en-US.png)


## Expand data disks \(EMR V3.11.0 and earlier\) {#section_rws_wx4_y2b .section}

**Note:** You must expand the disks of all nodes in a cluster to make sure that the nodes have the same disk size.

1.  Log on to the [Alibaba Cloud EMR console](https://partners-intl.console.aliyun.com/#/emr) and then go to the **Cluster Management** page.
2.  On the Cluster Management page, click the ID of the target cluster in the **Cluster ID/Name** column. You can also perform this task on the Overview page.
3.  Select the instance group that you want to expand disks for in the Instance Info area at the bottom of the Cluster Overview page. Copy the **ECS ID** of an ECS instance shown on the right side.

    **ECS ID** example: **i-bp1bsithym5hh9h93xxx**.

4.  Click the **ECS ID** of the ECS instance to log on to the ECS console.

    After you log on to the ECS console, information about the selected ECS instance is displayed.

5.  In the left-side navigation pane, choose **Disks**. You can then expand the data disks for the ECS instance on the right side of the page. For more information, see [Resize cloud disks online](../../../../reseller.en-US/Block Storage/Block storage/Resize cloud disks/Resize cloud disks online.md#) or [Resize cloud disks offline](../../../../reseller.en-US/Block Storage/Block storage/Resize cloud disks/Resize cloud disks offline.md#).

    **Note:** Your cluster may not be able to meet all the requirements for expanding disks online. In this case, you can expand disks offline. Currently you cannot expand multiple disks at the same time. You can only expand one disk at a time.

6.  After the disks are expanded, you have to expand the partitions and file systems of the disks. For more information, see [Resize partitions and file systems of Linux data disks](../../../../reseller.en-US/Block Storage/Block storage/Resize cloud disks/Resize partitions and file systems of Linux data disks.md#).

    **Note:** 

    -   During the partition and file system expansion process, if an error occurs when the system runs the umount command, disable the **YARN** and **HDFS** services on the EMR cluster to resolve this issue.
    -   For the partition Disk1, the system may fail to run the umount command if the **ilogtail** service is writing log data at the same time. To resolve this issue, run the sudo pgrep ilogtail | sudo xargs kill command to disable the **ilogtail** service. After the partitions and file systems are expanded, restart the instance and recover the **ilogtail** service.
7.  After you complete the preceding tasks, log on to the ECS instance through SSH and then run the df -h command to verify the disk expansion result.
8.  Reference the preceding steps to expand other data disks for the ECS instance.

## Expand the system disk {#section_nzr_sy4_y2b .section}

**Note:** It is a complex procedure to expand the system disk. Do not expand the system disk unless it is necessary. Clusters other than HA clusters become unavailable during the system disk expansion process.

1.  Log on to the [Alibaba Cloud EMR console](https://partners-intl.console.aliyun.com/#/emr) and then go to the **Cluster Management** page.
2.  On the Cluster Management page, click the ID of the target cluster in the **Cluster ID/Name** column. You can also perform this task on the Overview page.
3.  Select the instance group that you want to expand the system disk for in the Instance Info area at the bottom of the Cluster Overview page. Copy the **ECS ID** of an ECS instance shown on the right side.

    **ECS ID** example: **i-bp1bsithym5hh9h93xxx**.

4.  Click the **ECS ID** of the ECS instance to log on to the ECS console.

    After you log on to the ECS console, information about the selected ECS instance is displayed.

5.  In the left-side navigation pane, select **Disks**. You can then expand the system disk for the ECS instance on the right side of the page. Each ECS instance has only one system disk. For more information, see [Resize cloud disks online](../../../../reseller.en-US/Block Storage/Block storage/Resize cloud disks/Resize cloud disks online.md#) or [Resize cloud disks offline](../../../../reseller.en-US/Block Storage/Block storage/Resize cloud disks/Resize cloud disks offline.md#).

    **Note:** Your cluster may not be able to meet all the requirements for expanding disks online. In this case, you can expand disks offline.

6.  After the disks are expanded, you have to expand the partitions and file systems of the disks. For more information, see [Extend the file system of the Linux system disk](../../../../reseller.en-US/Block Storage/Block storage/Resize cloud disks/Extend the file system of the Linux system disk.md#).

    **Note:** 

    -   During the partition and file system expansion process, if an error occurs when the system runs the umount command, disable the **YARN** and **HDFS** services on the EMR cluster to resolve this issue.
    -   For the partition Disk1, the system may fail to run the umount command if the **ilogtail** service is writing log data at the same time. To resolve this issue, run the sudo pgrep ilogtail | sudo xargs kill command to disable the **ilogtail** service. After the partitions and file systems are expanded, restart the instance and recover the **ilogtail** service.
7.  After you complete the preceding tasks, log on to the ECS instance through SSH and then run the df -h command to verify the disk expansion result.

    **Note:** After the system disk is expanded, the ECS instance may have the following issues:

    -   The ECS instance will make some changes to the system disk. This may change the /etc/hosts file on the ECS instance. You need to fix the file after you expand the system disk.
    -   The SSH logon configuration becomes invalid. You need to manually fix the configuration. Other services on the ECS instance are not affected.
8.  Reference the preceding steps to expand the system disk for other ECS instances.

