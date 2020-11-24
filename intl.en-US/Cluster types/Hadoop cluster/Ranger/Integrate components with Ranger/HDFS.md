# HDFS

This topic describes how to integrate HDFS with Ranger and how to configure related permissions.

An E-MapReduce \(EMR\) Hadoop cluster is created, and Ranger is selected from the optional services during the cluster creation. For more information about how to create a cluster, see [Create a cluster](/intl.en-US/Cluster Management/Configure clusters/Create a cluster.md).

## Integrate HDFS with Ranger

1.  Enable HDFS in Ranger.

    1.  Log on to the [EMR console](https://emr.console.aliyun.com/).

    2.  In the top navigation bar, select the region where your cluster residesand select a resource group based on your business requirements.

    3.  Click the **Cluster Management** tab.

    4.  On the **Cluster Management** page, find your cluster and click **Details** in the Actions column.

    5.  In the left-side navigation pane, choose **Cluster Service** \> **RANGER**.

    6.  On the page that appears, choose **Actions** \> **EnabledHDFS** in the upper-right corner.

        ![Enable HDFS PLUGIN](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5881027951/p11456.png)

    7.  In the **Cluster Activities** dialog box, configure relevant parameters and click **OK**. In the **Confirm** message, click **OK**.

        Click **History** in the upper-right corner to view the task progress.

2.  Add the HDFS service on the web UI of Ranger.

    1.  Log on to Ranger. For more information, see [Overview](/intl.en-US/Cluster types/Hadoop cluster/Ranger/Overview.md).

    2.  Add the HDFS service.

        ![Ranger UI](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3907409951/p11479.png)

    3.  Configure relevant parameters.

        ![hdfs](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6881027951/p112304.png)

        |Parameter|Description|
        |---------|-----------|
        |**Service Name**|Set the value to emr-hdfs.|
        |**Username**|Set the value to hadoop.|
        |**Password**|Enter a custom password.|
        |**Namenode URL**|        -   Enter hdfs://emr-header-1:9000 for a non-HA cluster.
        -   Enter hdfs://emr-header-1:8020 for an HA cluster. |
        |**Authorization Enabled**|Select **No** for a standard cluster and **Yes** for a high-security cluster.|
        |**Authentication Type**|        -   Select Simple for a standard cluster.
        -   Select Kerberos for a high-security cluster. |
        |**dfs.datanode.kerberos.principal**|These parameters are required only for a high-security cluster. Set the value to hdfs/\_HOST@EMR.$\{id\}.com. **Note:** You can log on to the host and run the `hostname` command to obtain the value of `${id}`. The number in `hostname` is the value of $\{id\}. |
        |**dfs.namenode.kerberos.principal**|
        |**dfs.secondary.namenode.kerberos.principal**|
        |**Add New Configurations**|        -   **Name**: Set the value to **policy.download.auth.users**.
        -   **Value**: Set the value to **hdfs**. |

    4.  Click **Add**.

3.  Restart HDFS.

    1.  In the left-side navigation pane, choose **Cluster Service** \> **HDFS**.

    2.  On the page that appears, choose **Actions** \> **Restart All Components** in the upper-right corner.

    3.  In the **Cluster Activities** dialog box, configure relevant parameters and click **OK**. In the **Confirm** message, click **OK**.

        Click **History** in the upper-right corner to view the task progress.


## Example of permission configuration

For example, you can perform the following steps to grant user test the Write or Execute permission on resources in the /user/foo directory:

1.  Log on to Ranger. For more information, see [Overview](/intl.en-US/Cluster types/Hadoop cluster/Ranger/Overview.md).

2.  Click **emr-hdfs**.

    ![Example of permission configuration](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6881027951/p11482.png)

3.  Click **Add New Policy** in the upper-right corner.

4.  Configure relevant parameters.

    |Parameter|Description|
    |---------|-----------|
    |**Policy Name**|The name of the policy. You can customize a name.|
    |**Resoure Path**|The path of the resources.|
    |**recursive**|Specifies whether the permissions take effect on subdirectories or files.|
    |**Select Group**|The user group to which you want to add this policy.|
    |**Select User**|The user to whom you want to add this policy.|
    |**Permissions**|The permissions to be granted.|

5.  Click **Add**.

    After the policy is added, authorization is completed. User test can access the /user/foo directory.

    **Note:** After you add, remove, or modify a policy, it takes about one minute for the configuration to take effect.


