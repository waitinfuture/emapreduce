# Hive configuration {#concept_ej3_jn3_bfb .concept}

## Integrate Hive into Ranger {#section_l5r_443_bfb .section}

[Ranger introduction](intl.en-US/User Guide/Component authorization/RANGER/Ranger introduction.md#) introduced how to create and start a cluster of the Ranger service in E-MapReduce and some preparation work. This section describes the step-by-step process for integrating Ranger into Hive.

-   Hive access model

    You can access Hive data in three ways: HiveServer2, Hive Client and HDFS.

    -   HiveServer2
        -   Scenario: You can access Hive data only through HiveServer2, not through Hive client or HDFS.
        -   Mode: Use the Beeline client or the JDBC code to run the related Hive script through HiveServer2.
        -   Permission settings:

            The [SQL Standards Based Authorization](intl.en-US/User Guide/Component authorization/Hive authorization.md#) in Hive is used to control the permissions for the usage scenarios of HiveServer2.

            The table or column level permission control for Hive in Ranger is also the usage scenario for HiveServer2. If you are still able to access Hive data though Hive client or HDFS, table or column level control is insufficient. Further permission control for the following two ways is required.

    -   Hive client
        -   Scenario: Users can access Hive data from Hive Clients.
        -   Mode: Access from Hive clients.
        -   **Permission settings:**

            The Hive client requests HiveMetaStore to perform some DDL operations such as alter table, add columns, and read and process data in HDFS by submitting MapReduce jobs.

            The [Storage Based Authorization](intl.en-US/User Guide/Component authorization/Hive authorization.md#) in Hive is used to control the permissions for the usage scenarios of Hive client. It will determine whether this user can perform relevant DDL and DML operations based on the read and write permissions of the HDFS path where the table involved in SQL is located, such as `ALTER TABLE test ADD COLUMNS(b STRING)`.

            You can control the permissions of the HDFS path in Hive tables in Ranger. This, in combination with the HiveMetaStore configuration \(Storage Based Authorization\) enables you to implement permission control over the access scenario of Hive client.

            **Note:** The DDL operation permissions in Hive client scenarios are actually controlled by the underlying HDFS. If you have HDFS permissions, you will also have the DDL operation permissions for the tables \(such as drop tables and alter tables\).

    -   HDFS Mode
        -   Scenario: Users have direct access to HDFS.
        -   Mode: HDFS client and code.
        -   Permission settings:

            To have direct access to HDFS, you need to add permission control for HDFS on the underlying HDFS data for Hive tables.

            You can perform [Permission control](intl.en-US/User Guide/Component authorization/RANGER/Hive configuration.md#) for the underlying HDFS path of Hive tables through Ranger.

-   Enable Hive Plugin

    1.  On Cluster Management page, click **Manage** after the cluster you want to operate in the **Actions** column.
    2.  Click **Ranger** in the service list to enter the Ranger management page...
    3.  On the ranger configuration page, click the **Actions** drop-down menu in the upper-right corner, and select**Enable Hive PLUGIN**, then click **OK**.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17950/154294491011501_en-US.png)

    4.  Enter record information in the prompt box, and then click**OK**.

        You can check the progress by clicking **View Operation Logs** at the upper right corner of the page.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17950/154294491011502_en-US.png)

    **Note:** After you enable Hive Plugin and restart Hive, HiveServer2 scenario and HiveClient scenario \(Storage Based Authorization\) are configured accordingly. For information about HDFS permissions, see [HDFS configuration](intl.en-US/User Guide/Component authorization/RANGER/HDFS configuration.md#).

-   Restart Hive

    After the preceding task is completed, it is necessary to restart Hive to make it effect.

    1.  In the Ranger management page, click the inverted triangle icon behind RANGER in the upper left corner to switch to Hive.
    2.  Click **Actions** in the upper-right corner, Select **RESTART All Components** from the drop-down menu, and then click **OK**.
    3.  You can check the progress by clicking **View Operation Logs** in the upper right corner of the page.
-   Add the Hive service to the Ranger WebUI

    About how to enter the Ranger UI page, see [Ranger introdcution](https://help.aliyun.com/document_detail/66410.html?spm=a2c4g.11186623.2.11.63f75952TKxSqc).

    Add the Hive service to the Ranger WebUI

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17950/154294491011506_en-US.png)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17950/154294491011507_en-US.png)

    -   Instructions

        Enter a fixed value for the following configuration items:

        |name|value|
        |----|-----|
        |Service Name|emr-hive|
        |jdbc.driverClassName|org.apache.hive.jdbc.HiveDriver|

    -   Enter a variable value for the following configuration items:

        |name|value|
        |----|-----|
        |jdbc.url|standard cluster: jdbc:hive2://emr-header-1:10000/ high-security mode cluster: jdbc:hive2://$\{master1\_fullhost\}:10000/;principal=hive/$\{master1\_fullhost\}@EMR.$id.COM|
        |policy.download.auth.users|standard cluster: hadoop high-security mode cluster: hive|

        $\{master1\_fullhost\} is a long domain name of master1, you can log on to master 1 and run `hostname` command to obtain this name.The number in $\{master1\_fullhost\} is the value of $id.


## Permission configuration example {#section_yww_vv3_bfb .section}

Ranger is integrated into Hive in the previous section, which allows you to set relevant permissions. For example, authorize the user foo the Select permission for column a of the table testdb.test.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17950/154294491011509_en-US.png)

Click **emr-hive** in the preceding figure to configure relevant permissions.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17950/154294491011510_en-US.png)

In terms of the preceding settings, the permissions are granted to the user foo. The user foo can access the table testdb.test.

**Note:** The policy takes effect in 1 minute after it is added.

