# Disk scale-out {#concept_f3v_bw4_y2b .concept}

In case of insufficient data storage space, cluster disks of EMR 3.11.0 and later versions can be scaled out directly on the cluster overview page. Cluster disks of EMR 3.11.0 and earlier versions can be scaled out in the ECS console.

## Scale-out entry {#section_bp2_hjc_wfb .section}

On the Cluster Management page, find the target cluster. Click **View Details**, and in the upper right corner, click **Scale Up/Out**.

## Scale-out interface {#section_zjf_h4n_y2b .section}

As shown in the following figure:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17864/154293909332531_en-US.png)

**Note:** 

-   E-MapReduce currently supports scaling out, does not support scaling in.
-   Disk scaling out only supports data disk scaling out.

-   Configuration: data disk configurations of the current instance group.
-   Scale out to: data disk size after being scaled out.

## Node data disk scaling out {#section_rws_wx4_y2b .section}

1.  Log onto the EMR console, find the target cluster, click **View Details** to go to the cluster overview page.
2.  View the ECS ID of the core node in the cluster to be scaled out, for example, i-bp1bsithym5hh9h93xxx. All the node disks in the cluster are uniformly scaled out by default to make sure the disk spaces of all nodes in the cluster are consistent.
3.  Copy the ECS ID and go to the ECS console. Select **Instance Details** on the left, then select the instance ID and enter the ECS ID in the search box. Note that the same region must be selected.
4.  When the corresponding ECS node is found, click **Manage**, and in the left-side navigation pane, click **Disks**.
5.  Scale out data disks. You must repeat the following steps to scale out each disk, because multiple disks cannot be scaled out in batch.
6.  Scale out all the disks in the ECS console, and then restart nodes.
7.  See [Linux \_ Resize a data disk](../../../../intl.en-US/User Guide/Cloud disks/Resize cloud disks/Linux _ Resize a data disk.md#) for disk scaling out.

    **Note:** When the unmount operation fails, YARN, and HDFS services must be disabled on the cluster. Additionally, Disk1 operation may encounter unmount failure because of log writing by ilogtail. ilogtail must be temporarily killed using the command sudo pgrep ilogtail | sudo xargs kill. The ilogtail service can be recovered by restarting the node later.

8.  Then you can see that all disks are resized using the command df -h on the node.
9.  To guarantee that the subsequent EMR resize process is consistent with the disk after the resize, open a ticket to contact our team to perform cluster data updates.

## Node system disk resize {#section_nzr_sy4_y2b .section}

System disk resize is a complex operation. if not necessary, avoid this operation.

1.  In the EMR console, click to enter the details page of the cluster to be resized.
2.  View the ECS ID of the master node in the cluster to be scaled out, for example, i-bp1bsithym5hh9h93xxx. All the node disks in the cluster are uniformly resized by default to make sure the disk spaces of all nodes in the cluster are consistent.
3.  Copy the ECS ID and go to the ECS console. In the left-side navigation pane, select **Instance Details**, then select the instance ID and enter the ECS ID in the search box. Note that the same region must be selected.
4.  When the corresponding ECS node is found, click **Manage**, enter the instance details page, and click **Disks** on the left.
5.  Find the system disk \(only one system disk can be available\).
6.  See [Increase system disk size](../../../../intl.en-US/User Guide/Cloud disks/Resize cloud disks/Increase system disk size.md#) for system disk scaling out.

    **Note:** 

    -   For a non-HA cluster, scaling out the system disk makes the cluster unavailable while scaling out.
    -   After resizing, the file /etc/hosts in the node may be changed because of some disk operations by ECS, and the file must be repaired after the disk is scaled out. Also, SSH log-on-free is damaged, but the service remains unaffected and can be manually repaired.

