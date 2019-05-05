# Disk expansion {#concept_f3v_bw4_y2b .concept}

In the event that you do not have enough data storage space, cluster disks of E-MapReduce 3.11.0 or later can be expanded on the cluster overview page.

## Enter the expansion interface {#section_bp2_hjc_wfb .section}

On the Cluster Management page, find the target cluster. Click **Details**. On the Cluster Overview page, click Change Configuration. From the drop-down list, select **Disk Expansion** .

Select the instance group that needs to be expanded.

**Note:** Make sure your account balance is sufficient. The fee of expanding a data disk is deducted automatically. When your account balance is insufficient the, the process of expanding a data disk will be interrupted.

## Expansion interface {#section_zjf_h4n_y2b .section}

The following figure shows the disk expansion interface displayed.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17864/155704134632531_en-US.png)

-   Configuration: Data disk configurations of the current instance group.
-   Expand to: Data disk size after the disk is expanded.

**Note:** 

-   E-MapReduce currently supports the expansion disks, but not their reduction.
-   The expansion of disks is only supported for data disks.

## Restart a cluster {#section_ap5_klt_cgb .section}

1.  After completing the disk expansion, the corresponding core instance group displays **The disk capacity expansion is complete. Restart the servers for the changes to take effect**, as shown in the following figure.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17864/155704134634253_en-US.png)

2.  Click **The disk capacity expansion is complete. Restart the servers for the changes to take effect**, as shown in the following figure.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17864/155704134634254_en-US.png)

    **Note:** If you restart a cluster, you also restart ECS instances. ECS instance data is unavailable during restarting.

    -   **Rolling Start** 
        -   If selected, only ECS instances you select is restarted. Instances are restarted one after another. Each restart takes about five minutes.
        -   If not selected, all ECS instances are restarted.
    -   **Only Restart Updated Nodes** 
        -   Configuration-updated nodes are nodes whose disks or configurations have been expanded or upgraded.
        -   If selected, only configuration-updated nodes are restarted. For example, if you expanded a core group node, but did not expand a master group node's disk or upgrade its configurations, only the ECS instance in the core group is restarted, and the ECS instance in the master group will not be restarted.
        -   If not selected, all nodes \(all instances in the cluster\) are restarted.
3.  During the restart process, the prompt in the following figure is displayed for the corresponding node group \(such as a core group\).

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17864/155704134634255_en-US.png)

4.  After the prompt in step 3 is no longer displayed, all data disks are expanded and available to use.

## Node data disk expansion {#section_rws_wx4_y2b .section}

1.  Log on to the E-MapReduce console, find the target cluster, and click **View Details** to go to the cluster overview page.
2.  View the ECS ID of the core node in the cluster you want to expand, such as i-bp1bsithym5hh9h93xxx. All the node disks in the cluster are expanded uniformly by default to make sure the disk spaces of all nodes in the cluster are consistent.
3.  Copy the ECS ID and go to the ECS console. Select **Instance Details** in the pane on the left, then select the instance ID and enter the ECS ID in the search box. Note that the same region must be selected.
4.  When the corresponding ECS node is found, click **Manage**, and click **Disks** in the navigation pane on the left.
5.  Expand the data disks. You must repeat the following steps to scale out each disk, because you cannot expand multiple disks in batches.
6.  Expand all the disks in the ECS console and then restart the nodes.

    See[LinuxResize a data disk](../../../../reseller.en-US/Block Storage/Block storage/Resize cloud disks/Extend the file system of a Linux data disk.md#)for more information on expanding data disks.

    **Note:** If the unmount operation fails, YARN and HDFS services must be disabled in the cluster. Disk1 operations may encounter unmount failures because of log writing by ilogtail. ilogtail must be temporarily killed using the command sudo pgrep ilogtail | sudo xargs kill. ilogtail can then be recovered by restarting the node.

7.  You can then see that all disks are expanded using the df -h command on the node.

## Node system disk expansion {#section_nzr_sy4_y2b .section}

Cluster disks of E-MapReduce 3.11.0 or earlier can be scaled out in the ECS console.

1.  In the E-MapReduce console, enter the details page of the cluster you want to expand.
2.  View the ECS ID of the master node in the cluster you want to expand, such as i-bp1bsithym5hh9h93xxx. All the node disks in the cluster are expanded uniformly by default to make sure the disk spaces of all nodes in the cluster are consistent.
3.  Copy the ECS ID and go to the ECS console. Select **Instance Details** in the pane on the left, then select the instance ID and enter the ECS ID in the search box. Note that the same region must be selected.
4.  When the corresponding ECS node is found, click **Manage**, and click **Disks** in the navigation pane on the left.
5.  Find the system disk \(there is only one\).
6.  See [Increase system disk size](../../../../reseller.en-US/Block Storage/Block storage/Resize cloud disks/Resize a cloud disk offline.md#) for more information on expanding system disks.

    **Note:** 

    -   For non-HA clusters, while you are expanding the system disk, the cluster becomes unavailable.
    -   ECS disk operations may have changed the /etc/hosts file in the node. After the disk has been expanded, the file must be repaired. SSH log-on-freeis also damaged, but the service remains unaffected and can be repaired manually.

