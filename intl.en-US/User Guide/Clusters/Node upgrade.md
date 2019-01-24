# Node upgrade {#concept_pkz_l24_y2b .concept}

In real scenarios, the CPU or memory of a cluster node, especially master nodes, may be insufficient. We recommend that you upgrade nodes in the following way.

**Note:** Only Subscription clusters can be upgraded.

## Procedure {#section_uhx_qw4_y2b .section}

1.  In the Cluster Management panel, select a cluster, and click **View Details**.
2.  In the upper-right corner, click **Renew & Upgrade** \> **Upgrade**.
3.  Configure the nodes that you want to upgrade.
4.  Click **OK**.
5.  Pay for your order.
6.  Return to the Cluster Overview page, refresh the page to make sure that the node configuration has become the target specification. The following figure displays the upgraded node information.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17863/154831718337798_en-US.png)

7.  Click **升级配置已完成，重启机器组生效** to go to the following figure:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17863/154831718337818_en-US.png)

8.  Click **OK**.

    **Note:** 

    -   If you restart a cluster, you also restart ECS instances. ECS instance data is unavailable during restarting.
    -   **Rolling Start**
        -   If selected, only ECS instances you select is restarted. Instances are restarted one after another. Each restart takes about five minutes.
        -   If not selected, all ECS instances are restarted.
    -   **Only Restart Updated Nodes**
        -   Configuration-updated nodes are nodes whose disks or configurations have been expanded or upgraded.
        -   If selected, only configuration-updated nodes are restarted. For example, if you expanded a core group node, but did not expand a master group node's disk or upgrade its configurations, only the ECS instance in the core group is restarted, and the ECS instance in the master group will not be restarted.
        -   If not selected, all nodes \(all instances in the cluster\) are restarted.
9.  Modify cluster configurations so that YARN can use new resources.
    1.  Modify the yarn-site.xml file.
    2.  Change the value of yarn.nodemanager.resource.memory-mb to machine memory \* 0.8 and change the value of yarn.scheduler.maximum-allocation-mb to machine memory \* 0.8. The unit is MB. For example, in this new configuration, the memory is 32 GB:

        ```
        yarn.nodemanager.resource.memory-mb=26214
        yarn.scheduler.maximum-allocation-mb=26214
        ```

        If your cluster does not support page modification, you must log on to the node, and modify the corresponding configuration values in the /etc/emr/hadoop-conf/yarn-site.xml file for each node.

    3.  Restart YARN. Generally, you only need to restart the worker node. However, after the restart, the Node Manager port is changed. Therefore, we recommend that you restart the Resource Manager.
10. Submit a ticket to us providing information about the new node configuration. We will then synchronize the configuration.

