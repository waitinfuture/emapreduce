# Presto

This topic describes how to integrate Presto with Ranger and how to configure Presto permissions.

An E-MapReduce \(EMR\) cluster is created, and Ranger and Presto are selected from the optional services during the cluster creation. For more information, see [Create a cluster](/intl.en-US/Cluster Management/Configure clusters/Create a cluster.md).

## Integrate Presto with Ranger

1.  Enable Presto in Ranger.

    1.  Log on to the [EMR console](https://emr.console.aliyun.com/).

    2.  In the top navigation bar, select the region where your cluster residesand select a resource group based on your business requirements.

    3.  Click the **Cluster Management** tab.

    4.  On the **Cluster Management** page, find your cluster and click **Details** in the Actions column.

    5.  In the left-side navigation pane, choose **Cluster Service** \> **RANGER**.

    6.  Choose **Actions** \> **EnabledPresto** in the upper-right corner.

    7.  In the **Cluster Activities** dialog box, configure relevant parameters and click **OK**. In the **Confirm** message, click **OK**.

        Click **History** in the upper-right corner to view the task progress.

2.  Add the Presto service on the web UI of Ranger.

    1.  Log on to the Ranger web UI. For more information, see [Overview](/intl.en-US/Cluster types/Hadoop cluster/Ranger/Overview.md).

    2.  Add the Presto service.

        ![presto](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9044027951/p81570.png)

    3.  Configure parameters.

        ![Presto](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9044027951/p112352.png)

        |Parameter|Description|
        |---------|-----------|
        |**Service Name**|Set the value to emr-presto.|
        |**Username**|Set the value to hadoop.|
        |**Password**|Leave this parameter empty.|
        |**jdbc.driverClassName**|Set the value to io.prestosql.jdbc.PrestoDriver.|
        |**jdbc.url**|Set the value to jdbc:presto://emr-header-1:9090.|
        |**Add New Configurations**|        -   **Name**: Set the value to policy.download.auth.users.
        -   **Value**: Set the value to hadoop. |

    4.  Click **Add**.

3.  Restart Presto Master.

    1.  In the left-side navigation pane, choose **Cluster Service** \> **Presto**.

    2.  Choose **Actions** \> **Restart PrestoMaster** in the upper-right corner.

    3.  In the **Cluster Activities** dialog box, configure relevant parameters and click **OK**. In the **Confirm** message, click **OK**.

        Click **History** in the upper-right corner to view the task progress.


## Permission configuration examples

Different from Hive and HBase permissions, you must configure Presto permissions based on a hierarchical permission control policy.

**Note:**

-   Make sure that the hierarchical levels you specify match the permissions you want to configure. Otherwise, the permissions do not take effect.
-   Presto checks the permissions of a user twice. It first checks whether the user has access permissions on a catalog and then checks the permissions involved in the current access.

Example 1: Grant user liu the Select permission on column a in the Hive table testdb.test

1.  Log on to the Ranger web UI. For more information, see [Overview](/intl.en-US/Cluster types/Hadoop cluster/Ranger/Overview.md).
2.  Click **emr-presto**.

    ![shili1](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9044027951/p81621.png)

3.  Click **Add New Policy** in the upper-right corner.

    Create a policy to control access to a **catalog**. Only access permissions on a **catalog** are required. You do not need to specify the levels lower than **catalog**. In the Allow Conditions section, set Select User to liu and Permissions to Use. This way, you can grant user liu the Use permission on the specified **catalog**.

    ![catelog](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3258055061/p139688.png)

4.  Click **Add**.
5.  Click **Add New Policy** in the upper-right corner.

    Configure the Select permission on column a of table testdb.test for user liu. The Select permission on tables is a **column-level** permission. You must click the ![Add a policy](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7254326061/p88629.png) icon to specify **schema**, **table**, and **column** in sequence.

    ![Configure permissions on a column for user liu](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0144027951/p88630.png)

    |Parameter|Description|
    |---------|-----------|
    |**Policy Name**|The name of the policy. You can customize a policy name, such as testdb.|
    |**catalog**|The name of the **catalog**. You can customize a catalog name, such as hive.|
    |**schema**|The name of the **schema**, such as testdb. You can set this parameter to an asterisk \(\*\) to indicate all schemas.|
    |**table**|The name of the **table**, such as test. You can set this parameter to an asterisk \(\*\) to indicate all tables.|
    |**column**|The name of the **column**, such as a. You can set this parameter to an asterisk \(\*\) to indicate all columns.|
    |**Select User**|The user to whom you want to add this policy, such as liu.|
    |**Permissions**|The permission you want to grant, such as Select.|

6.  Click **Add**.

    After the policy is configured, the authorization is complete. User liu can access column a of table testdb.test.

    **Note:** After you add, remove, or modify a policy, it takes about one minute for the configuration to take effect.


Example 2: Grant user chen the Create permission on Hive table testdb.test

1.  Log on to the Ranger web UI. For more information, see [Overview](/intl.en-US/Cluster types/Hadoop cluster/Ranger/Overview.md).
2.  Create a policy to control access to a catalog. In this example, the catalog is hive.
    -   If such a policy already exists, you only need to add user chen in the Select User column.
        1.  Click **emr-presto**.
        2.  Find the **catalog\_hive** policy and click the ![edit](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7254326061/p139695.png) icon in the Action column.

            ![chen](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7254326061/p139694.png)

        3.  In the Allow Conditions section, add chen in the Select User column. This way, you can grant user chen the Use permission on the specified **catalog**.

            ![add_condition](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7254326061/p85657.png)

        4.  Click **Add**.
    -   If no catalog-level policy is available, perform the operations in [Example 1](#p_1uo_eyy_3d9) to grant user chen the Use permission on the specified **catalog**.
3.  Click **Add New Policy** in the upper-right corner.

    The Create permission on tables is a **schema-level** permission. You only need to specify **schema**. Click the ![Add a policy](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7254326061/p88629.png) icon to specify **schema**.

    ![add_table](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0144027951/p85660.png)


Example 3: Configure the permissions to run the `show schemas` and `show tables` commands

-   EMR versions earlier than V3.28.1

    Configure the Select permission with a hierarchy of `schema=information_schema,table=schemate,column=*`. This allows you to obtain the execution results of the `show schemas` command. Configure the Select permission with a hierarchy of `schema=information_schema,table=tables,column=*`. This allows you to obtain the execution results of the `show tables` command. You can configure the two permissions in the same policy, as shown in the following figure.

    ![eg3](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4258055061/p165714.png)

-   EMR V3.28.1 and later 3.X versions, and EMR V4.4.1

    To run the `show schemas` command, you must be granted the Show permission on the catalog to which the schemas belong. To run the `show tables` command, you must be granted the Show permission on the schema to which the tables belong.

    After Presto authentication is completed, Presto filters the obtained schemas and tables to display only the schemas and tables on which you have the Select permission. Therefore, you must also be granted the Select permission on schemas and tables. When you run a show command, only the schemas or tables on which you have the Select permission are displayed.

    ![eg3-2](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7254326061/p165731.png)


## Share Hive permissions configured in Ranger with Presto

In some business scenarios, you may need to use the same permissions for Presto and Hive. In this case, you do not need to separately configure the permissions for Presto because EMR allows you to share the permissions for Hive in Ranger.

**Note:** Hive permissions can be shared with Presto in Ranger only when the catalog is hive.

Take note of the following items before you enable permission sharing:

-   Make sure that you have integrated Hive with Ranger. For more information, see [Hive](/intl.en-US/Cluster types/Hadoop cluster/Ranger/Integrate components with Ranger/Hive.md).
-   Make sure that you have added the Presto service on the Ranger web UI.
-   To allow users to use the `show schemas` or `show tables` command, you must grant users the Select permission with a hierarchy of `database=information_schema,table=*,column=*` for the Hive service. This operation is similar to that in the Presto service.

Perform the following steps to enable permission sharing:

1.  Log on to the master node of the cluster, and change the value of the `ranger.plugin.hive.authorization.enable` parameter to `true` in the /etc/ecm/presto-conf/ranger-presto-security.xml configuration file.

2.  In the left-side navigation pane, choose **Cluster Service** \> **Presto**.

3.  Choose **Actions** \> **Restart PrestoMaster** in the upper-right corner.

    In the **Cluster Activities** dialog box, configure relevant parameters and click **OK**. In the **Confirm** message, click **OK**.


