# Table mangement {#concept_ukp_lxk_z2b .concept}

## Introduction {#section_fkz_hyk_z2b .section}

E-MapReduce 2.4.0 and later versions support central metadata management. In the versions earlier than E-MapReduce 2.4.0, all clusters use the local MySQL database as the Hive metadatabase. In E-MapReduce 2.4.0 and later versions, E-MapReduce supports the central highly-reliable Hive metadatabase.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17932/153965764611067_en-US.png)

You can enable the central metadatabase function when creating a cluster to use the external metadatabase.

**Note:** 

-   The current metadatabase needs to be connected using the public IP address. Therefore, the cluster must have a public IP address. Do not change the public IP address. Otherwise, the corresponding database whitelist is invalid.
-   The table management function can be used only when the central metadatabase function is enabled when a cluster is created. A local metadatabase does not support table management currently. In that case, you may use the Hue tool in the cluster for table management.

The central metadata management function can:

1.  Provide long-term metadata storage.

    When metadata is stored in the local MySQL database of the cluster, metadata is lost when the cluster is released. Especially when E-MapReduce supports the flexible creation mode, clusters can be created and released anytime upon requirements. To retain the metadata, you must log on to the cluster and manually export the metadata. This issue can be resolved with the central metadata management function.

2.  Separate computing and storage.

    E-MapReduce supports storing data in Alibaba Cloud OSS, which reduces the usage cost especially when the data volume is large. Meanwhile, E-MapReduce clusters are mainly used as computing resources and can be released anytime after use. Since data is stored in OSS, the metadata migration issue does not exist.

3.  Implement data sharing.

    With the central metadatabase, if all data is stored in OSS, all clusters can directly access data without migrating or restructuring metadata. This enables E-MapReduce clusters to provide different services while still ensuring direct data sharing.


**Note:** 

Before central metadata management is supported, metadata is stored in the local MySQL database of each cluster and is lost when the cluster is released. With central metadata management, releasing clusters does not clean up metadata. Before you delete the data in OSS or in the HDFS of a cluster or you release a cluster, make sure that the corresponding metadata is already deleted. That means the tables and database that store the data have been dropped. This prevents dirty metadata in the database.

## Table management operations {#section_izq_41l_z2b .section}

Before E-MapReduce clusters support metadata management, you have to log on to the internal environment of a cluster to check, add, or delete tables in the cluster. If more than one clusters exist, you have to log on to the clusters one by one, which is inconvenient. With the central metadata management function, E-MapReduce enables table management on the console. This includes checking the list of databases and tables, checking table details, creating and deleting databases and tables, and previewing data.

-   Database and table list
-   Table details
-   Data preview
-   Create a database
-   Create a table

    Two methods to create a table:Manual creation and Creation from a file

    -   Manual creation, When no service data exists, you can manually input the table structure to create an empty table;
    -   Creation from a file, When service data already exists, you can use the service data as a table directly by parsing the table interface from the file. Make sure that the separators used for creating a table must correspond to those used in the data file to have a proper table structure.
    The separators can be common characters such as commas and spaces, or special characters TAB, ^A, ^B, and ^C.

    **Note:** 

    1.  Databases and tables can be created and deleted only in E-MapReduce clusters.
    2.  The HDFS is the internal file system of each cluster and does not support cross-cluster communication without special network settings. Therefore, the E-MapReduce table management function only supports creating databases and tables based on the OSS file system.
    3.  The location of a database or table must be in a directory under the OSS bucket, rather than the OSS bucket.

## FAQs {#section_jkt_hdl_z2b .section}

1.  Wrong FS: oss://yourbucket/xxx/xxx/xxx

    This error occurs when the table data on OSS is deleted while the metadata of the table is not. The table schema persists, while the actual data does not exist or is moved to another location. In this case, you can change the table location to an existing path and delete the table again.

    `alter table test set location 'oss://your_bucket/your_folder'`

    This can be completed on the E-MapReduce interactive console.

    **Note:** oss://your\_bucket/your\_foldermust be an existing OSS path.

2.  Wrong FS: hdfs://yourhost:9000/xxx/xxx/xxx

    This error occurs when the table data in the HDFS is deleted while the table schema persists. The error can be removed by using the preceding solution.

3.  The message “java.lang.IllegalArgumentException: java.net.UnknownHostException: xxxxxxx” is displayed when the Hive database is deleted.

    This error occurs because the Hive database is created in the HDFS of a cluster and it is not cleaned up when the cluster is released. As a result, its data in the HDFS of the released cluster cannot be accessed after a new cluster is created. Therefore, when releasing a cluster, remember to clean up the databases and tables that are manually created in the HDFS of the cluster.

    Solution

    Log on to the master node of the cluster using the command line, and find the address, username, and password for accessing the Hive metadatabase in $HIVE\_CONF\_DIR/hive-site.xml.

    ```
    javax.jdo.option.ConnectionUserName //Username for accessing the database;
    javax.jdo.option.ConnectionPassword //Password for accessing the database;
    javax.jdo.option.ConnectionURL //Address and name of the database ;
    ```

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17932/153965764611097_en-US.png)

    Log on to the Hive metadatabase on the master node of the cluster:

    ```
    mysql -h ${DBConnectionURL} -u ${ConnectionUserName} -p [Press Enter]
    [Enter the password]${ConnectionPassword}
    ```

    After logging on to the Hive metadatabase, change its location to an existing OSS path in the region:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17932/153965764611102_en-US.png)


