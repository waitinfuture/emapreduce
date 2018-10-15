# Node upgrade {#concept_pkz_l24_y2b .concept}

In actual use, the CPU or memory of cluster nodes, especially master nodes, may be insufficient. Currently, E-MapReduce does not support direct upgrade configuration. We recommend that you upgrade nodes configuration in the following way.

**Note:** Subscription clusters can be upgraded.

## Procedure {#section_uhx_qw4_y2b .section}

1.  Confirm the cluster to be upgraded.
2.  Go to the EMR console, click **Manage** in the dashboard to enter the Cluster Management page.
3.  Click **Upgrade Configuration** on the right corner.
4.  Modify the upgrade configuration of nodes to be upgraded.
5.  Click **OK**. Wait for a while, the order is generating.
6.  Pay for the order.

    Return to the Cluster Management page, refresh the page to make sure that the node configuration has become the target specification, for example, CPU is 4 core and memory is 16G.

7.  Go to the ECS console, find the upgraded instances and restart them one by one.
8.  Modify the cluster configuration so that Yarn can use the new resources.
    1.  Modify the yarn-site.xml file.
    2.  Change the value of yarn.nodemanager.resource.memory-mb to machine memory \* 0.8, the unit is MB. Change the value of yarn.scheduler.maximum-allocation-mb to machine memory \* 0.8, the unit is MB. For example, in my new configuration, the memory is 32 GB:

        ```
        yarn.nodemanager.resource.memory-mb=26214
        yarn.scheduler.maximum-allocation-mb=26214
        ```

        If your cluster does not support page modification, you must log on to the node, and modify the corresponding configuration values in the `/etc/emr/hadoop-conf/yarn-site.xml` file for each node.

    3.  Restart the Yarn service. Generally, you only must restart the worker node. However, after the restart, the Node Manger port is changed. Therefore, we recommend that you restart the Resource Manager.
9.  pen a ticket to the E-MapReduce team for providing information about the new node configuration. The E-MapReduce team will synchronize the configuration to guarantee normal cluster operating.

