# Configure an independent ApsaraDB RDS instance

This topic describes how to configure an ApsaraDB RDS instance as the metadatabase of E-MapReduce \(EMR\).

An ApsaraDB RDS instance is purchased. For more information, see [Create an ApsaraDB RDS for MySQL instance](/intl.en-US/RDS MySQL Database/Quick start/Create an ApsaraDB RDS for MySQL instance.md).

**Note:** We recommend that you select 5.7 from the **MySQL** drop-down list for **Database Engine** and set **Edition** to **High-availability** when you create an ApsaraDB RDS instance.

## Prepare a metadatabase

1.  Create a database named hivemeta.

    For more information, see the "Create a database" section of the [Create accounts and databases for an ApsaraDB RDS for MySQL instance](/intl.en-US/RDS MySQL Database/Quick start/Create accounts and databases for an ApsaraDB RDS for MySQL instance.md) topic.

    ![create_database](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2211549951/p93349.png)

2.  Create a user and grant read and write permissions to this user.

    For more information, see the "Create a standard account" section of the [Create accounts and databases for an ApsaraDB RDS for MySQL instance](/intl.en-US/RDS MySQL Database/Quick start/Create accounts and databases for an ApsaraDB RDS for MySQL instance.md) topic.

    ![create_user](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2211549951/p93347.png)

    Record the username and password of the account you created so that you can use the information when you create a cluster. For information about how to create a cluster, see [Create a cluster](#section_13x_gol_ybr).

3.  Obtain the internal endpoint of the database.

    1.  Configure a whitelist. For more information, see [Control access to an ApsaraDB RDS for MySQL instance](/intl.en-US/RDS MySQL Database/Quick start/Control access to an ApsaraDB RDS for MySQL instance.md).

    2.  In the left-side navigation pane of the instance details page, click **Database Connection**.

    3.  On the **Database Connection** page, click the internal endpoint to copy it.

        ![net_inter](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2211549951/p93358.png)

        Record the internal endpoint so that you can use it when you create a cluster. For information about how to create a cluster, see [Create a cluster](#section_13x_gol_ybr).


## Create a cluster

Configure the parameters described in the following table in the **Basic Settings** step of the EMR console. For information about the configuration of other parameters, see [Create a cluster](/intl.en-US/Cluster Management/Configure clusters/Create a cluster.md).

![create_cluster](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2211549951/p93366.png)

|Parameter|Description|
|---------|-----------|
|**Cluster Name**|The name of the cluster. The name must be 1 to 64 characters in length and can contain only letters, digits, hyphens \(-\), and underscores \(\_\).|
|**Type**|Select **Independent ApsaraDB RDS for MySQL**.|
|**Connection URL**|Specify **Connection URL** in the format of jdbc:mysql://rm-xxxxxx.mysql.rds.aliyuncs.com/<Database name\>?createDatabaseIfNotExist=true&characterEncoding=UTF-8. -   rm-xxxxxx.mysql.rds.aliyuncs.com is the internal endpoint of the database obtained in [Prepare a metadatabase](#section_94v_jfe_16n).
-   <Database name\> is the name of the database specified in [Prepare a metadatabase](#section_94v_jfe_16n). |
|**Database Username**|Set this parameter to the username of the account specified in [Prepare a metadatabase](#section_94v_jfe_16n).|
|**Database Password**|Set this parameter to the password of the account specified in [Prepare a metadatabase](#section_94v_jfe_16n).|

## Initialize the metastore service

1.  Log on to the [EMR console](https://emr.console.aliyun.com/).

2.  In the top navigation bar, select the region where your cluster residesand select a resource group based on your business requirements.

3.  Click the **Cluster Management** tab.

4.  On the **Cluster Management** page, find your cluster and click **Details** in the Actions column.

5.  In the **Software Info** section of the **Cluster Overview** page, check the Hive version and initialize the metastore service of Hive.

    -   For Hive V2.3.5:

        Log on to the master node by using SSH and perform the following steps to initialize the metastore service:

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

        Parameter description:

        -   `{Username to log on to the ApsaraDB RDS for MySQL database}` is the username of the account specified in [Prepare a metadatabase](#section_94v_jfe_16n).
        -   `{Password to log on to the ApsaraDB RDS for MySQL database}` is the password of the account specified in [Prepare a metadatabase](#section_94v_jfe_16n).
        -   `{Name of the ApsaraDB RDS for MySQL database}` is the name of the database specified in [Prepare a metadatabase](#section_94v_jfe_16n).
    -   For Hive of another version:

        Log on to the master node by using SSH and run the following commands to initialize the metastore service:

        ```
        su hadoop
        schematool -initSchema -dbType mysql
        ```

    After the initialization is completed, you can use the created ApsaraDB RDS instance as the Hive metadatabase.

    **Note:** However, the MetaStore and HiveServer2 processes for Hive and the ThriftServer process for Spark may encounter exceptions before the initialization is completed. After the initialization is completed, these processes can run properly again.

    If an exception is reported during the initialization, check the configurations of ApsaraDB RDS for MySQL security groups, and make sure that a security group whitelist is enabled for EMR clusters.

    ![initialize_fail](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7523204061/p93353.png)

    If Hive metadata contains information such as column comments and partition names, you can run the following commands one at a time in the specific ApsaraDB RDS instance to configure fields in the UTF-8 format:

    ```
    mysql> alter table <your_table> modify column <COMMENT> varchar(256) character set utf8;
    ```

    ```
    mysql> alter table TABLE_PARAMS modify column PARAM_VALUE varchar(4000) character set utf8;
    ```

    ```
    mysql> alter table PARTITION_PARAMS modify column PARAM_VALUE varchar(4000) character set utf8;
    ```

    ```
    mysql> alter table PARTITION_KEYS modify column PKEY_COMMENT varchar(4000) character set utf8;
    ```

    ```
    mysql> alter table INDEX_PARAMS modify column PARAM_VALUE varchar(4000) character set utf8;
    ```


