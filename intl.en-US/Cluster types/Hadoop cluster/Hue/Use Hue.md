---
keyword: [Hue, use Hue]
---

# Use Hue

This topic describes how to configure and access the Hue service in EMR. Hue allows you to interact with a Hadoop cluster for data analysis and processing from your browser.

-   A security group rule is configured. For more information.

    **Note:** When you set the authorization object in the security group rule, enter only the CIDR blocks or IP addresses that need to access Hue. Do not enter the CIDR block 0.0.0.0/0.

-   Port 8888 is enabled. For more information, see [Access open-source components](/intl.en-US/Cluster Management/Configure clusters/Access open-source components.md).

## View the initial password of the default administrator in Hue

If no administrator is configured, the first user who logs on to Hue becomes the administrator. To avoid the security risks caused by this mechanism, EMR creates an account named admin as the default administrator in Hue and generates a random password as the initial password of the admin account. To view the initial password of the admin account, perform the following steps:

1.  Log on to the [EMR console](https://emr.console.aliyun.com/).

2.  In the top navigation bar, select the region where your cluster resides. Select the resource group as required. By default, all resources of the account appear.

3.  Click the **Cluster Management** tab.

4.  On the **Cluster Management** page that appears, find the target cluster and click **Details** in the Actions column.

5.  In the left-side navigation pane, click **Cluster Service** and then **Hue**.

6.  Click the **Configure** tab and find the **admin\_pwd** parameter. The value of this parameter is the initial password of the admin account.

    **Note:** The initial password is fixed. To change the logon password of the admin account, log on to Hue with the admin account and the initial password, and go to the User Admin page to change the password. Alternatively, you can also perform the steps in the [Reset the password of a Hue account](#section_3j8_0ke_hse) section.


## Access Hue

1.  Log on to the [EMR console](https://emr.console.aliyun.com/).

2.  In the top navigation bar, select the region where your cluster resides. Select the resource group as required. By default, all resources of the account appear.

3.  Click the **Cluster Management** tab.

4.  On the **Cluster Management** page that appears, find the target cluster and click **Details** in the Actions column.

5.  In the left-side navigation pane, click **Connect Strings**.

6.  Click the link for Hue.

7.  On the page that appears, enter the username and password to log on to Hue.


## Create a Hue account

1.  Log on to the [EMR console](https://emr.console.aliyun.com/).

2.  In the top navigation bar, select the region where your cluster resides. Select the resource group as required. By default, all resources of the account appear.

3.  Click the **Cluster Management** tab.

4.  On the **Cluster Management** page that appears, find the target cluster and click **Details** in the Actions column.

5.  In the **Master Instance Group** section, find the public IP address of the master node.

6.  Log on to the master node. For more information, see [Connect to the master node of an EMR cluster in SSH mode](/intl.en-US/Cluster Management/Configure clusters/Connect to a cluster/Connect to the master node of an EMR cluster in SSH mode.md).

7.  Run the following command to create an account:

    ```
    /opt/apps/hue/build/env/bin/hue createsuperuser
    ```

8.  Enter the username, email address, password, and confirm password. Then, press **Enter**.

    If the message **Superuser created successfully** appears, the account is created. You can log on to Hue with the new account.


## Reset the password of a Hue account

1.  Log on to the master node of your cluster in SSH mode. For more information, see [Connect to the master node of an EMR cluster in SSH mode](/intl.en-US/Cluster Management/Configure clusters/Connect to a cluster/Connect to the master node of an EMR cluster in SSH mode.md).

2.  Run the following command to view the Hue path:

    ```
    ps aux | grep hue
    ```

    The following figure shows an example of the command output.

    ![check hue file](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6728593061/p131575.png)

    **Note:** In this example, the path is /opt/apps/hue/build/env/bin/hue.

3.  Run the following command to reset the password of a Hue account:

    ```
    from django.contrib.auth.models import User
    user = User.objects.get(username='your username')  //Enter the username whose password you want to change.
    user.set_password('your new password') //Enter the new password.
    user.save()
    ```

    **Note:** You can press Ctrl+D to exit the SSH session.

    Example:

    ![change password](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2679560061/p131577.png)


## Add custom configurations

1.  Log on to the [EMR console](https://emr.console.aliyun.com/).

2.  In the top navigation bar, select the region where your cluster resides. Select the resource group as required. By default, all resources of the account appear.

3.  Click the **Cluster Management** tab.

4.  On the **Cluster Management** page that appears, find the target cluster and click **Details** in the Actions column.

5.  In the left-side navigation pane, click **Cluster Service** and then **Hue**.

6.  Click the **Configure** tab.

7.  In the **Service Configuration** section, click the **hue** tab.

8.  Click **Custom Configuration** in the upper-right corner. In the Add Configuration Item dialog box, add the following parameters in the Keys column and configure values for them:

    ```
    $section_path.$real_key
    ```

    Parameter description:

    -   `$real_key`: the key that you want to add. Example: `hive_server_host`.
    -   `$section_path`: the section where the key resides. You can obtain the value from the hue.ini file.

        For example, the `hive_server_host` key resides in the `[beeswax]` section in the [hue.ini](https://github.com/cloudera/hue/blob/release-4.1.0/desktop/conf.dist/hue.ini) file. In this case, set `$section_path` to `beeswax`.

        **Note:** After you configure the parameters, the `beeswax.hive_server_host` key is added. If the path to a key involves multiple nested sections in the hue.ini file, for example, \[desktop\] -\> \[\[ldap\]\] -\> \[\[\[ldap\_servers\]\]\] -\> \[\[\[\[users\]\]\]\] -\>user\_name\_attr, the key you need to configure is `desktop.ldap.ldap_servers.users.user_name_attr`.


## Configure YARN queues

When you submit an SQL statement in Hue to perform an interactive query, Hue submits an application for computing resources to YARN. To manage and isolate computing resources for different Hive SQL or Spark SQL jobs, perform the following steps to configure YARN queues:

1.  Log on to the [EMR console](https://emr.console.aliyun.com/).

2.  In the top navigation bar, select the region where your cluster resides. Select the resource group as required. By default, all resources of the account appear.

3.  Click the **Cluster Management** tab.

4.  On the **Cluster Management** page that appears, find the target cluster and click **Details** in the Actions column.

5.  Add or modify the queue configuration.

    -   For Hive SQL, configure the queue parameter in HiveServer2 based on the engine type.

        **Note:** In this topic, replace QUEUENAME with the actual name of the queue to be configured.

        1.  In the left-side navigation pane, click **Cluster Service** and then **Hive**.
        2.  Click the **Configure** tab.
        3.  In the **Service Configuration** section, click the **hiveserver2-site** tab.
        4.  Click **Custom Configuration** in the upper-right corner. In the Add Configuration Item dialog box, add the queue parameter that corresponds to your engine based on the information in the following table.

            |Engine|Parameter|Value|
            |------|---------|-----|
            |Hive on MR|mapreduce.job.queuename|QUEUENAME|
            |Hive on Tez|tez.queue.name|
            |Hive on Spark|spark.yarn.queue|

            **Note:** You can change the value in the **Service Configuration** section.

    -   For Spark SQL, Spark Thrift Server is used to perform SQL queries. Perform the following steps to configure the queue parameter in Spark Thrift Server:
        1.  In the left-side navigation pane, click **Cluster Service** and then **Spark**.
        2.  Click the **Configure** tab.
        3.  In the **Service Configuration** section, click the **spark-thriftserver** tab.
        4.  Click **Custom Configuration** in the upper-right corner. In the Add Configuration Item dialog box, add the spark.yarn.queue parameter in the Key column and set it to the name of the queue that you want to configure.
6.  Restart HiveServer2 and Spark Thrift Server in the cluster where Hue resides.

    1.  In the left-side navigation pane of the **Cluster Overview** page, click **Cluster Service** and then **Hive**.

    2.  Click the **Component Development** tab. Find the **HiveServer2** component and click **Restart** in the Actions column.

        In the Cluster Activities dialog box, enter the required information and click **OK**.

    3.  In the left-side navigation pane of the **Cluster Overview** page, click **Cluster Service** and then **Spark**.

    4.  Click the **Component Development** tab. Find the **ThriftServer** component and click **Restart** in the Actions column.

        In the Cluster Activities dialog box, enter the required information and click **OK**.


