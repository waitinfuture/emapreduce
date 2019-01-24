# Node upgrade {#concept_pkz_l24_y2b .concept}

In real scenarios, the CPU or memory of a cluster node, especially master nodes, may be insufficient. We recommend that you upgrade nodes in the following way.

**Note:** Only Subscription clusters can be upgraded.

## Procedure {#section_uhx_qw4_y2b .section}

1.  In the Cluster Management panel, select a cluster, and click **View Details** to go to the **Cluster Overview** pane.
2.  In the upper-right corner, click **Renew & Upgrade** \> **Upgrade**.
3.  Configure the nodes that you want to upgrade.
4.  Click **OK**.
5.  Pay for your order.
6.  Return to the Cluster Management page, refresh the page to make sure that the node configuration has become the target specification. The following figure displays the upgraded node information.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17863/154832624737798_en-US.png)

7.  Click **The specification upgrades are complete. Restart the server for the upgrades to take effect** to view to the following figure.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17863/154832624737818_en-US.png)

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
9.  During the restart process, the prompt **Restarting Servers** in the following figure is displayed for the corresponding node group \(such as a core group\).

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17863/154832624737825_en-US.png)

10. Log on to the EMR cluster to check upgrades. After the prompt in step 9 is not displayed, all upgraded configurations take effect.
11. If you just upgraded the CPU and did not upgrade the memory, ignore this step. If you upgraded the **memory**, you need to modify cluster configurations so that YARN can use new resources.
    1.  On the Clusters and Services pane, click **YARN**.
    2.  Click the **Configuration** tab, locate yarn.nodemanager.resource.memory-mb and yarn.scheduler.maximum-allocation-mb two configuration items, and change the value of the two configuration items to machine memory \* 0.8. The unit is MB. For example, in this new configurations, the memory is 32 GB, configure the following value 26214 \(32\*1024\*0.8\).

        ```
        yarn.nodemanager.resource.memory-mb=26214
        yarn.scheduler.maximum-allocation-mb=26214
        ```

    3.  In the upper right corner, click **Save**.
    4.  In the upper right corner, click **Actions** \> **CONFIGURE All Components**.
    5.  In the upper right corner, click **View Operation Logs** and wait for the status of the CONFIGURE YARN operation type to **Successful**.
    6.  In the upper right corner, click **Actions** \> **RESTART All Components**.
    7.  Click**View Operation Logs**, wait for the status of the RESTART YARN operation type to **Successful**, and new resources can be used by YARN.

