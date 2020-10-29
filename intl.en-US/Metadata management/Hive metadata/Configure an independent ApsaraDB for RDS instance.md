# Configure an independent ApsaraDB for RDS instance

This topic describes how to configure an ApsaraDB for RDS instance as the metadatabase of E-MapReduce \(EMR\).

An RDS instance is purchased.

**Note:** We recommend that you select 5.7 from the **MySQL** drop-down list for **Database Engine** and set **Edition** to **High-availability** in the RDS console when you create an RDS instance.

## Prepare a metadatabase

1.  Create a database named hivemeta.

    For more information, see the "Create a database" section of the [Create databases and accounts for an ApsaraDB RDS for MySQL instance](/intl.en-US/RDS MySQL Database/Quick start/Create accounts and databases for an ApsaraDB RDS for MySQL instance.md) topic.

    ![Create a database](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2211549951/p93349.png)

2.  Create a user and grant read-write permissions to this user.

    For more information, see the "Create a standard account" section of the [Create databases and accounts for an ApsaraDB RDS for MySQL instance](/intl.en-US/RDS MySQL Database/Quick start/Create accounts and databases for an ApsaraDB RDS for MySQL instance.md) topic.

    ![Create a user](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2211549951/p93347.png)

    Record the username and password of the account you created so you can use the information when you create a cluster. For information about how to create a cluster, see [Create a cluster](#section_13x_gol_ybr).

3.  Obtain the internal endpoint of the database.

    1.  Configure a whitelist.

    2.  In the left-side navigation pane of the instance details page, click **Database Connection**.

    3.  On the **Database Connection** page that appears, click the internal endpoint to copy it.

        ![Copy the internal endpoint](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2211549951/p93358.png)

        Record the internal endpoint so you can use it when you create a cluster. For information about how to create a cluster, see [Create a cluster](#section_13x_gol_ybr).


## Create a cluster

Configure parameters listed in the following table in the **Basic Settings** step of the EMR console. For information about the configuration of other parameters, see [Create a cluster](/intl.en-US/Cluster Management/Configure clusters/Create a cluster.md).

![Create a cluster](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2211549951/p93366.png)

|Parameter|Description|
|---------|-----------|
|**Cluster Name**|The name of the cluster. The name must be 1 to 64 characters in length and can contain only letters, digits, hyphens \(-\), and underscores \(\_\).|
|**Type**|Select **Independent ApsaraDB RDS for MySQL**.|
|**Connection URL**|Specify **Connection URL** in the format of jdbc:mysql://rm-xxxxxx.mysql.rds.aliyuncs.com/<Database name\>?createDatabaseIfNotExist=true&characterEncoding=UTF-8. -   rm-xxxxxx.mysql.rds.aliyuncs.com is the internal endpoint of the database obtained in [Prepare a metadatabase](#section_94v_jfe_16n).
-   Database name is the name of the database specified in [Prepare a metadatabase](#section_94v_jfe_16n). |
|**Database Username**|Set this parameter to the username of the account specified in [Prepare a metadatabase](#section_94v_jfe_16n).|
|**Database Password**|Set this parameter to the password of the account specified in [Prepare a metadatabase](#section_94v_jfe_16n).|

## Initialize the metastore service

1.  2.  In the top navigation bar, select the region where your cluster resides. Select the resource group as required. By default, all resources of the account appear.

3.  Click the **Cluster Management** tab.

4.  On the **Cluster Management** page that appears, find the target cluster and click **Details** in the Actions column.

5.  In the **Software Info** section of the **Cluster Overview** page that appears, check the Hive version and initialize the metastore service of Hive.

    -   For Hive V2.3.5:

        Log on to the master node in SSH mode and follow these steps to initialize the metastore service:

        1.  Enter the specified directory.

            ```
            cd /usr/lib/hive-current/scripts/metastore/upgrade/mysql/
            ```

        2.  Log on to the RDS for MySQL database.

            ```
            mysql -h {Private or public IP address of the RDS for MySQL database} -u{Username to log on to the RDS for MySQL database} -p{Password to log on to the RDS for MySQL database}
            ```

        3.  Run the following command on the command line of the RDS for MySQL database:

            ```
            use {Name of the RDS for MySQL database};
            source /usr/lib/hive-current/scripts/metastore/upgrade/mysql/hive-schema-2.3.0.mysql.sql;
            ```

        **Note:** `{Username to log on to the RDS for MySQL database}` is the username of the account specified in [Prepare a metadatabase](#section_94v_jfe_16n). `{Password to log on to the RDS for MySQL database}` is the password of the account specified in [Prepare a metadatabase](#section_94v_jfe_16n). `{Name of the RDS for MySQL database}` is the name of the database specified in [Prepare a metadatabase](#section_94v_jfe_16n).

    -   For Hive of another version:

        Log on to the master node in SSH mode and run the following commands to initialize the metastore service:

        ```
        su hadoop
        schematool -initSchema -dbType mysql
        ```

    After the initialization is complete, you can use the created RDS instance as the Hive metadatabase.

    **Note:** However, both the MetaStore and HiveServer2 processes for Hive and the ThriftServer process for Spark may encounter exceptions before initialization is complete. After the initialization is complete, these processes can run properly again.

    If an exception is reported during initialization, check the configurations of RDS for MySQL security groups, and make sure that a security group whitelist is enabled for EMR clusters.

    ![Initialization failure](../images/p93353.png)


