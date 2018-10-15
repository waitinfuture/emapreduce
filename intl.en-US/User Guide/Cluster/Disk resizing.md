# Disk resizing {#concept_f3v_bw4_y2b .concept}

Currently, direct disk resizing on the product is not supported for EMR. In case of insufficient data storage space, you can directly resize the disk in the ECS console.

The procedure is as follows.

## Node data disk resize {#section_rws_wx4_y2b .section}

1.  Log on to the EMR console to enter the details page of the cluster to be resized.
2.  View the ECS ID of the core node in the cluster to be resized, for example, i-bp1bsithym5hh9h93xxx. All the node disks in the cluster are uniformly resized by default to make sure the disk spaces of all nodes in the cluster are consistent.
3.  Copy the ECS ID and go to the ECS console. Select **Instance** on the left, then select the instance ID and enter the ECS ID in the search box. Note that the same region must be selected.
4.  When the corresponding ECS node is found, click **Management**, enter the instance details page, and click **This Instance Disk** on the left.
5.  Resize the data disk. You must repeat the preceding steps individually to resize each disk, because multiple disks cannot be resized in batch.
6.  Resize all the disks in the ECS console, and then restart the node.
7.  See [ECS disk resize instructions](https://help.aliyun.com/document_detail/25452.html) for disk resizing.

    **Note:** When the unmount operation fails, YARN, and HDFS services must be disabled on the cluster. Additionally, Disk1 operation may encounter unmount failure because of log writing by ilogtail. ilogtail must be temporarily killed using the command sudo pgrep ilogtail | sudo xargs kill. The ilogtail service can be recovered by restarting the node later.

8.  Then you can see that all disks are resized using the command df -h on the node.
9.  To guarantee that the subsequent EMR resize process is consistent with the disk after the resize, open a ticket to contact our team to perform cluster data updates.

## Node system disk resize {#section_nzr_sy4_y2b .section}

System disk resize is a complex operation. if not necessary, avoid this operation.

1.  In the EMR console, click to enter the details page of the cluster to be resized.
2.  View the ECS ID of the master node in the cluster to be resized, for example, i-bp1bsithym5hh9h93xxx. All the node disks in the cluster are uniformly resized by default to make sure the disk spaces of all nodes in the cluster are consistent.
3.  Copy the ECS ID and go to the ECS console. Select the **Instance** on the left, then select the instance ID and enter the ECS ID in the search box. Note that the same region must be selected.
4.  When the corresponding ECS node is found, click **Management**, enter the instance details page, and click **This Instance Disk** on the left.
5.  Find the system disk \(only one system disk can be available\).
6.  See [ECS system disk resize instructions](https://help.aliyun.com/document_detail/44986.html) for system disk resizing.

    **Note:** 

    -   For a non-HA cluster, resizing the system disk makes the cluster unavailable while resizing.
    -   After resizing, the file /etc/hosts in the node may be changed because of some disk operations by ECS, and the file must be repaired after resizing. Also, SSH log-on-free is damaged, but the service remains unaffected and can be manually repaired.

