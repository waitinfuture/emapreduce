# Integrate Ranger into Hive {#concept_ej3_jn3_bfb .concept}

## Procedure {#section_l5r_443_bfb .section}

This section describes the step-by-step process for integrating Ranger into Hive.

-   Hive access model

    You can access Hive data in three ways: HiveServer2, Hive client, and HDFS.

    -   HiveServer2
        -   Mode: Use the Beeline client or the JDBC code to run the related Hive script through HiveServer2.
        -   Permission settings:

            Hive's [SQL Standard Based Authorization](intl.en-US/User Guide/Component authorization/Hive authorization.md#) is used to control the permissions of HiveServer2.

            Hive's table- and column-level permission control in Ranger is also used for HiveServer2. However, if you are still able to access Hive data though a Hive client or HDFS, table- or column-level permission control is insufficient and further control is required.

    -   Hive client
        -   Mode: Access from a Hive client.
        -   Permission settings:

            The Hive client requests the metastore to perform DDL operations, such as altering tables, adding columns, and reading and processing data in HDFS, by submitting MapReduce jobs.

            Hive's [Storage Based Authorization](intl.en-US/User Guide/Component authorization/Hive authorization.md#) is used to control the permissions of Hive clients. It determines whether a user can perform DDL and DML operations based on the read and write permissions of the HDFS path where the table involved in SQL is located, such as `ALTER TABLE test ADD COLUMNS(b STRING)`.

            In Ranger, you can control the permissions of the HDFS path in Hive tables. This, in combination with the Hive metastore which is configured with storage-based authorization, enables you to implement permission control over the access of the Hive client.

            **Note:** The DDL operation permissions of a Hive client are actually controlled by the underlying HDFS permissions. If you have HDFS permissions, you also have DDL permissions for tables, such as dropping and altering tables.

    -   HDFS
        -   Mode: HDFS client and code.
        -   Permission settings:

            To have direct access to HDFS, you need to add permission control for HDFS on the underlying HDFS data of the Hive tables.

            You can use Ranger to perform permission control for the underlying HDFS path of Hive tables.

-   Enable the Hive plug-in

    1.  On the Cluster Management page, click **Manage** next to the cluster you want to operate in the **Actions** column.
    2.  Click **Ranger** in the service list to enter the Ranger Management page.
    3.  On the Ranger Configuration page, click the **Actions** drop-down menu in the upper-right corner, select **Enable Hive PLUGIN**, and click **OK**.

        ![Enable Hive PLUGIN](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17950/155065538711501_en-US.png)

    4.  Enter the record information in the prompt box and click**OK**.

        You can check the progress by clicking **View Operation Logs** in the upper-right corner of the page.

        ![View Operation Logs](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17950/155065538711502_en-US.png)

    **Note:** After you enable the Hive plug-in and restart Hive, HiveServer2 and Hive client scenarios are configured accordingly. For more information about HDFS permissions, see [Integrate Ranger into HDFS](intl.en-US/User Guide/Component authorization/Ranger/Integrate Ranger into HDFS.md#).

-   Restart Hive

    After enabling the Hive plug-in, you need to restart Hive. To do so, complete the following steps:

    1.  In the Ranger Management page, click the Ranger drop-down menu in the upper-left corner, and select Hive.
    2.  Click **Actions** in the upper-right corner, select **RESTART All Components** from the drop-down menu, and click **OK**.
    3.  You can check the progress by clicking **View Operation Logs** in the upper-right corner of the page.
-   Add the Hive service to the Ranger UI

    For more information about how to access the Ranger UI page, see [Introduction to Ranger](EN-US_TP_17948.dita#concept_gpl_jrc_z2b).

    Add the Hive service.

    ![Ranger UI](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17950/155065538711506_en-US.png)

    ![Add the Hive service](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17950/155065538711507_en-US.png)

    -   Instructions

        Enter a fixed value for the following configuration items:

        |Name|Value|
        |----|-----|
        |Service Name|emr-hive|
        |jdbc.driverClassName|org.apache.hive.jdbc.HiveDriver|

    -   Enter a variable value for the following configuration items:

        |Name|Value|
        |----|-----|
        |jdbc.url|Standard cluster: jdbc:hive2://emr-header-1:10000/ high-High-security cluster: jdbc:hive2://$\{master1\_fullhost\}:10000/;principal=hive/$\{master1\_fullhost\}@EMR.$id.COM|
        |policy.download.auth.users|Standard cluster: hadoop High-security cluster: hive|

        $\{master1\_fullhost\} is the long domain name of master1. To obtain this name, log on to master1 and run the `hostname` command. The number in $\{master1\_fullhost\} is the value of $id.


## Permission configuration {#section_yww_vv3_bfb .section}

After integrating Ranger into Hive, you can set permissions, such as granting user foo the Select permission for column A in the testdb.test table.

![Permission configuration](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17950/155065538811509_en-US.png)

In the preceding figure, click **emr-hive** to enter the policy configuration page.

![Policy configuration](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17950/155065538811510_en-US.png)

Permissions are granted to user foo. They can now access the testdb.test table.

**Note:** The policy takes effect one minute after it is added.

