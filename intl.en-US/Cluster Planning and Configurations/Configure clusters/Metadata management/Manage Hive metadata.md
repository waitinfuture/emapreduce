# Manage Hive metadata {#concept_ukp_lxk_z2b .concept}

E-MapReduce has supported unified metadata management since V2.4.0. Before V2.4.0, clusters use local MySQL databases to store the metadata for Hive. E-MapReduce V2.4.0 and later versions will support unified and high-reliability Hive metadatabases.

## Overview {#section_fkz_hyk_z2b .section}

![Hive metadatabases](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17932/156135848711067_en-US.png)

With unified metadata management, the following features are supported.

-   Provides persistent metadata storage.

    Before, metadata is stored in MySQL databases that are deployed on clusters. Metadata is deleted when clusters are released. You can release a Pay-As-You-Go cluster after using it. For retaining the metadata, you need to log on to the cluster and export the metadata manually. Unified metadata management helps you avoid the issue.

-   You can separate computing and storage.

    E-MapReduce supports storing data in Alibaba Cloud OSS, which reduces the usage cost especially when the data volume is large. Meanwhile, E-MapReduce clusters are mainly used as computing resources and can be released anytime after use. Since data is stored in OSS, the metadata migration issue does not exist.

-   Implement data sharing

    With the central metadatabase, if all data is stored in OSS, all clusters can directly access data without migrating or restructuring metadata. This enables E-MapReduce clusters to provide different services while still ensuring direct data sharing.


**Note:** 

Before unified metadata management is supported, metadata is stored in the local MySQL database of each cluster and is lost when the cluster is released. With unified metadata management, releasing clusters does not clean up metadata. Before you delete the data in OSS or in the HDFS of a cluster or you release a cluster, make sure that the corresponding metadata is already deleted. That means the tables and database that store the data have been dropped. This prevents dirty metadata in the database.

## Create a cluster using unified metadata {#section_rpg_fgj_wgb .section}

-   Use webpages

    On the Basic Settings page, turn on **Unified Metabases**.

-   Use APIs

    See the description of the [CreateClusterV2](../../../../reseller.en-US/API Reference/Cluster/CreateClusterV2.md#) operation. Set the value of the useLocalMetaDb parameter to false.


## Operate metadata {#section_t2k_hhj_wgb .section}

-   Prerequisites

    The EMR console unified metadata management page only supports clusters that use unified metadata.

-   Operate databases

    You can search for databases by name. Click a database name to view all tables stored in the database.

    **Note:** Before deleting a database, you need to delete all tables that are stored in the database.

-   Operate tables

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17932/156135848811078_en-US.png)

    Create a table: currently, only creating external tables and partition tables is supported. The separators can be common characters such as commas and spaces, special characters such as TAB, ^A, ^B, and ^C, and custom separators.

-   Table details

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17932/156135848811079_en-US.png)

    View partitions: For partition tables, you can go to **Table Details** \> **Partitions** to view the partition lists. A maximum of 10,000 partitions can be displayed.

    **Note:** 

    -   Databases and tables can be created and deleted only in E-MapReduce clusters.
    -   The HDFS is the internal file system of each cluster and does not support cross-cluster communication without special network settings. Therefore, the E-MapReduce table management function only supports creating databases and tables based on the OSS file system.
    -   The location of a database or table must be in a directory under the OSS bucket, rather than the OSS bucket.
-   Metadatabase information

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17932/156135848811080_en-US.png)

    On the metatabase information page, you can view the usage and restrictions of the current RDS instance. Submit a ticket if you need to modify the information.


## Migration {#section_b2g_nkj_wgb .section}

-   Migrate data from an EMR unified metadatabase to a user-created RDS instance
    1.  Purchase an RDS instance and make sure the RDS instance can connect to the master node of the cluster. We recommend that you add the RDS instance to the security group of the ECS instances that are in the EMR cluster. By doing this, you can use a private address to connect to the RDS instance.

        **Note:** For security reasons, you need to add the IP addresses of the EMR nodes that are allowed to access the RDS instances to the whitelist.

    2.  Create a database in the RDS instance and name it hivemeta. Create a user and grant the user the read/write permission to hivemeta.
    3.  Export the unified metadata. We recommend that you stop the Hive metastore service before exporting the data to avoid the changes of metadata during the export progress.
    4.      5.  Log on to the [Alibaba Cloud E-MapReduce console](https://partners-intl.console.aliyun.com/#/emr). Click the **Cluster Management** tab to go to the Cluster Management page.
    6.  Click the cluster ID to go to the Clusters and Services page. On the Services list, click **Hive** \> **Configure** to go to the Hive configuration page. Locate the values of the following configuration items. For earlier versions of EMR clusters that do not support configuration management, locate the configuration items in the $HIVE\_CONF\_DIR/hive-site.xml file.

        ``` {#codeblock_gdg_zjf_bvi}
        javax.jdo.option.ConnectionUserName //the database username;
        javax.jdo.option.ConnectionPassword //the database password;
        javax.jdo.option.ConnectionURL //the database connection URL and database name;
        ```

    7.  Run the following command to log on to the master node of the cluster using [SSH](reseller.en-US/Cluster Planning and Configurations/Configure clusters/Connect to a cluster using SSH.md#).

        ``` {#codeblock_k65_327_7zv}
        mysqldump　-t　hivemeta -h <unified metadatabase connection URL >　-u <unified metadatabase username>　-p　>　/tmp/metastore.sql
        ```

        Enter the value of javax.jdo.option.ConnectionPassword as the password.

    8.  In the usr/local/emr/emr-agent/run/meta\_db\_info.json file that is stored on the master node of the cluster, set the value of use\_local\_meta\_db to false. Set the connection URL, username, and password of the RDS instance for the metadatabase.
    9.  On the Hive configuration page, set the connection URL, username, and password of the RDS instance for the metadatabase. For earlier versions of EMR clusters that do not support configuration management, locate the configuration items in the $HIVE\_CONF\_DIR/hive-site.xml file.
    10. On the master node, set the connection URL, username, and password of the RDS instance for the metadatabase in the hive-site.xml file. Run the init schema command.

        ``` {#codeblock_e7h_kcl_jxw}
        cd /usr/lib/hive-current/bin
        ./schematool -initSchema -dbType mysql
        ```

    11. Import the exported metadata to the RDS instance. Use the CLI to log on to MySQL.

        ``` {#codeblock_whx_jp0_09f}
        mysql -h {The url of rds} -u {RDS username} -p
        ```

        In the MySQL CLI, run the source /tmp/metastore.sql file.

    12. On the Clusters and Services page, click **RESTART ALL COMPONENTS** to restart all Hive components. Run the hive cli command on the master node to check whether the???

## FAQ {#section_jkt_hdl_z2b .section}

-   Wrong FS:oss://yourbucket/xxx/xxx/xxx or Wrong FS: hdfs://yourhost:9000/xxx/xxx/xxx 

    This error occurs when the table data on OSS is deleted while the metadata of the table is not. The table schema persists, while the actual data does not exist or is moved to another location. In this case, you can change the table location to an existing path and delete the table again.

    `alter table test set location 'oss://your_bucket/your_folder'`

    This can be completed on the E-MapReduce interactive console.

    **Note:** oss://your\_bucket/your\_folder is required to an existing OSS path.

-   The message “java.lang.IllegalArgumentException: java.net.UnknownHostException: xxxxxxx” is displayed when the Hive database is deleted.

    This error occurs because the Hive database is created in the HDFS of a cluster and it is not cleaned up when the cluster is released. As a result, its data in the HDFS of the released cluster cannot be accessed after a new cluster is created. Therefore, when releasing a cluster, remember to clean up the databases and tables that are manually created in the HDFS of the cluster.

    Solutions:

    Log on to the master node of the cluster using the command line, and find the address, username, and password for accessing the Hive metadatabase in $HIVE\_CONF\_DIR/hive-site.xml.

    ``` {#codeblock_v6n_gmd_gsy}
    javax.jdo.option.ConnectionUserName //the database username;
    javax.jdo.option.ConnectionPassword //the database password;
    javax.jdo.option.ConnectionURL //the database connection URL and database name;
    ```

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17932/156135848811097_en-US.png)

    Connect to the Hive metadatabase on the master node.

    ``` {#codeblock_q7d_f4j_a6t}
    mysql -h ${DBConnectionURL} -u ${ConnectionUserName} -p [Press Enter]
    [Enter the password]${ConnectionPassword}
    ```

    After logging on to the Hive metadatabase, change its location to an existing OSS path in the region:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17932/156135848811102_en-US.png)


