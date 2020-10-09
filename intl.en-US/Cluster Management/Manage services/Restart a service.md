# Restart a service

After you modify the configuration items of a service, you must restart the service to validate the modifications. This topic describes how to restart a service in the E-MapReduce \(EMR\) console.

We recommend that you restart a service in rolling mode. Otherwise, business operations may be affected. For instances deployed in primary/secondary mode, restart the service on the standby instance first and then the service on the primary instance.

**Note:** If you clear Rolling Execute, the service is restarted on all nodes at the same time. In this case, the service stops running during the restart. Proceed with caution.

1.  Log on to the [EMR console](https://emr.console.aliyun.com/).

2.  In the top navigation bar, select the region where your cluster resides. Select the resource group as required. By default, all resources of the account appear.

3.  Click the **Cluster Management** tab.

4.  On the **Cluster Management** page that appears, find the target cluster and click **Details** in the Actions column.

5.  In the left-side navigation pane of the Cluster Overview page, click **Cluster Service** and then **HDFS**.

    Take the HDFS service as an example.

6.  On the HDFS page, select **Restart All Components** from the **Actions** drop-down list in the upper-right corner.

7.  In the **Cluster Activities** dialog box, specify **Description** and click **OK**.

8.  In the **Confirm** message, click **OK**.


