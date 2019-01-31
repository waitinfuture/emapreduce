# Expand a cluster {#concept_jl4_d4n_y2b .concept}

If your cluster does not have enough resources, you can expand it horizontally. Only core and task nodes can be expanded. When expanding a cluster, configurations default to be consistent with the ECS instance purchased previously.

## Enter expansion interface {#section_yjf_h4n_y2b .section}

Select the cluster you want to expand from the list of clusters, click **More**, and select **Scale Up/Out**. You can also click **View Details** to the right of the cluster, and click **Scale Up/Out** in the upper right corner.

## Expansion interface {#section_zjf_h4n_y2b .section}

![Expansion interface](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17854/154891667210431_en-US.png)

**Note:** Although expansion is supported, reduction is not.

-   **Configuration**: Displays the configurations of the current instance.
-   **Billing Method**: Displays the payment method of the current cluster.
-   **Current Core Instances**: Displays the total number of your current core nodes.
-   **New Instances**: Enter the quantity of instances that you want to add.
-   **VSwitch**: Displays the VSwitch of the current cluster.

## Expansion status {#section_ckf_h4n_y2b .section}

In the following figure, the statuses of the cluster expansions are shown.

![Expansion status](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17854/154891667210432_en-US.jpg)

To view the expansion status of a cluster, click **Core Instance Group \(CORE\)** in the **Cluster Overview** panel. Nodes that are being expanded are displayed as **Scaling Up/Out**. When the status of an ECS instance changes to **Normal**, ECS has been added to the cluster and can now provide services.

## Change password {#section_kzc_yw5_vfb .section}

After you expand a cluster, you can log on to the expanded node with SSH and change your root password. To do so, follow these steps:

1.  Log on the master host with SHH using the following command and obtain the public IP of the master cluster in the [Cluster Overview](intl.en-US/User Guide/Clusters/Cluster details.md#) panel.

    ```
    ssh root@ip.of.master
    ```

2.  Switch to the hadoop user.

    ```
    su hadoop
    ```

3.  Log on to the expanded node and obtain the intranet IP of the expanded node in the [Cluster Overview](intl.en-US/User Guide/Clusters/Cluster details.md#) panel.

    ```
    ssh ip.of.worker
    ```

4.  Change your root password using the following command.

    ```
    sudo passwd root
    ```


