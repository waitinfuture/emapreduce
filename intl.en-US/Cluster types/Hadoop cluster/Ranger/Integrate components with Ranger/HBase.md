# HBase

This topic describes how to integrate HBase with Ranger and how to configure related permissions.

An E-MapReduce \(EMR\) Hadoop cluster is created, and HBase and Ranger are selected from the optional services during the cluster creation. For more information about how to create a cluster, see [Create a cluster](/intl.en-US/Cluster Management/Configure clusters/Create a cluster.md).

## Integrate HBase with Ranger

1.  Enable HBase in Ranger.

    1.  Log on to the [EMR console](https://emr.console.aliyun.com/).

    2.  In the top navigation bar, select the region where your cluster residesand select a resource group based on your business requirements.

    3.  Click the **Cluster Management** tab.

    4.  On the **Cluster Management** page, find your cluster and click **Details** in the Actions column.

    5.  In the left-side navigation pane, choose **Cluster Service** \> **RANGER**.

    6.  On the page that appears, choose **Actions** \> **EnabledHBase** in the upper-right corner.

        ![Enable HBase PLUGIN](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7283027951/p11513.png)

    7.  In the **Cluster Activities** dialog box, configure relevant parameters and click **OK**. In the **Confirm** message, click **OK**.

        Click **History** in the upper-right corner to view the task progress.

2.  Add the HBase service on the web UI of Ranger.

    1.  Log on to Ranger. For more information, see [Overview](/intl.en-US/Cluster types/Hadoop cluster/Ranger/Overview.md).

    2.  Add the HBase service.

        ![add_hbase](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7283027951/p81500.png)

    3.  Configure relevant parameters.

        ![add_hbase](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7283027951/p81312.png)

        |Parameter|Description|
        |---------|-----------|
        |**Service Name**|Set the value to **emr-hbase**.|
        |**Username**|Set the value to **hbase**.|
        |**Password**|Enter a custom password.|
        |**hadoop.security.authentication**|        -   Select **Simple** for a standard cluster.
        -   Select **Kerberos** for a high-security cluster. |
        |**hbase.master.kerberos.principal**|This parameter is required only for a high-security cluster. Set the value to **hbase/\_HOST@EMR.$\{id\}.COM**. **Note:** You can log on to the host and run the `hostname` command to obtain the value of `${id}`. The number in `hostname` is the value of $\{id\}. |
        |**hbase.security.authentication**|        -   Select **Simple** for a standard cluster.
        -   Select **Kerberos** for a high-security cluster. |
        |**hbase.zookeeper.property.clientPort**|Set the value to **2181**.|
        |**hbase.zookeeper.quorum**|Set the value to **emr-header-1,emr-worker-1**.|
        |**zookeeper.znode.parent**|Set the value to **/hbase**.|
        |**Add New Configurations**|        -   **Name**: Set the value to **policy.download.auth.users**.
        -   **Value**: Set the value to **hbase**. |

    4.  Click **Add**.

3.  Restart HBase.

    1.  In the left-side navigation pane, choose **Cluster Service** \> **HBase**.

    2.  On the page that appears, choose **Actions** \> **Restart All Components** in the upper-right corner.

    3.  In the **Cluster Activities** dialog box, configure relevant parameters and click **OK**. In the **Confirm** message, click **OK**.

        Click **History** in the upper-right corner to view the task progress.


## Configure administrator accounts

1.  On the **Service Manager** page, click **emr-hbase**.

    ![view hbase](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1592126951/p130024.png)

2.  Configure administrator accounts.

    Administrator accounts are used to run administrative commands, such as the balance, compaction, flush, and split commands.

    Click the ![edit](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7283027951/p81341.png) icon in the Action column for a specific policy and add accounts in the Users column. You can also modify the permissions. For example, retain only the Admin permission. You must set hbase as an administrator account.

    ![Configure administrator accounts](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7283027951/p11523.png)

    If you use Phoenix, you must add an additional policy in Ranger. The following table describes the parameters for the policy.

    |Parameter|Description|
    |---------|-----------|
    |**HBase Column-family**|**\***|
    |**HBase Column**|**\***|
    |**Select Group**|**public**|
    |**Permissions**|**Read**, **Write**, **Create**, and **Admin**|


## Example of permission configuration

For example, you can perform the following steps to grant user test the Create, Write, and Read permissions on the foo\_ns:test table:

1.  Log on to Ranger. For more information, see [Overview](/intl.en-US/Cluster types/Hadoop cluster/Ranger/Overview.md).

2.  Click **emr-hbase**.

    ![emr-habse](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7283027951/p11524.png)

3.  Click **Add New Policy** in the upper-right corner.

4.  Configure permissions.

    |Parameter|Description|
    |---------|-----------|
    |**Policy Name**|The name of the policy. You can customize a name.|
    |**HBase Table**|The table on which permissions are configured. The value must be in the format of **$\{namespace\}:$\{tablename\}**. You can specify multiple tables. Press Enter each time you enter a table name. If a table belongs to the default namespace, you do not need to prefix the table name with default. You can enter an asterisk \(\*\) as a wildcard for $\{tablename\}. For example, **foo\_ns:\*** indicates all tables in the foo\_ns namespace.

**Note:** **default:\*** is not supported. |
    |**HBase Column-family**|The column family.|
    |**HBase Column**|The name of the column.|
    |**Select Group**|The user group to which you want to add this policy.|
    |**Select User**|The user to whom you want to add this policy.|
    |**Permissions**|The permissions to be granted.|

5.  Click **Add**.

    After the policy is added, authorization is completed. User test can access the foo\_ns:test table.

    **Note:** After you add, remove, or modify a policy, it takes about one minute for the configuration to take effect.


