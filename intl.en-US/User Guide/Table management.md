# Table management {#concept_ukp_lxk_z2b .concept}

E-MapReduce 2.4.0 and later support the central management of metadata in the highly reliable Hive metastore. In earlier versions, clusters use the local MySQL database.

## Introduction {#section_fkz_hyk_z2b .section}

![central management of metadata](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17932/154814609211067_en-US.png)

When you create a cluster, you can enable the central metastore function so that the cluster uses an external metastore.

**Note:** 

-   The current metastore needs to be connected using the public IP address. The cluster must therefore have a public IP address. Do not change this IP address, otherwise the corresponding database whitelist becomes invalid.
-   The table management function can only be used if the central metastore function was enabled when a cluster was created. Local metastores do not currently support table management. If you have a local metastore, use the Hue tool in the cluster to manage your tables.

The central metadata management function performs the following:

1.  Provides long-term metadata storage.

    If metadata is stored in the local MySQL database of the cluster, it is lost when the cluster is released. Clusters can be created and released at any time as required, especially when E-MapReduce supports flexible creation. To retain metadata, you must log on to the cluster and manually export the data. This issue is resolved with the central metadata management function.

2.  Separates computing and storage.

    E-MapReduce supports storing data in Alibaba Cloud OSS, which reduces usage costs, especially when dealing with high volumes of data. Meanwhile, E-MapReduce clusters are mainly used as computing resources and can be released at any time after use. Since data is stored in OSS, problems with metadata migration are non-existent.

3.  Implements data sharing.

    With the central metastore, if all data is stored in OSS, all clusters can access the data without migrating or restructuring it. This enables E-MapReduce clusters to provide different services, while still ensuring direct data sharing.


**Note:** 

Before central metadata management is supported, metadata is stored in the local MySQL database of each cluster and is lost when the cluster is released. With central metadata management, releasing clusters does not clean up metadata. Before you delete the data in OSS or in the HDFS of a cluster or you release a cluster, make sure that the corresponding metadata is already deleted. That means the tables and database that store the data have been dropped. This prevents dirty metadata in the database.

## Table management operations {#section_izq_41l_z2b .section}

Before E-MapReduce clusters can support metadata management, you have to first log on to the internal environment of a cluster to check, add, or delete tables. If more than one cluster exists, you have to log on to each, one by one. This is inconvenient. With the central metadata management function, E-MapReduce enables table management on the console. This includes checking the list of databases and tables, checking table details, creating and deleting databases and tables, and previewing data.

-   Create a database or table list
-   View table details
-   Preview data
-   Create a database
-   Create a table

    There are two ways of creating a table: manually and from a file.

    -   Manual creation: If no service data exists, you can manually input the table structure to create an empty table.
    -   Creation from a file: If service data already exists, you can use it as a table by parsing the table interface from the file. Make sure that the separators used for creating a table correspond to those used in the data file. This guarantees a proper table structure.
    The separators can be common characters such as commas and spaces, or special characters TAB, ^A, ^B, and ^C.

    **Note:** 

    1.  Databases and tables can only be created and deleted in E-MapReduce clusters.
    2.  The HDFS is the internal file system of each cluster and does not support cross-cluster communication without special network settings. Therefore, the E-MapReduce table management function only supports creating databases and tables based on the OSS file system.
    3.  The location of a database or table must be in a directory under the OSS bucket, rather than the OSS bucket.

## Common issues {#section_jkt_hdl_z2b .section}

1.  Wrong FS: oss://yourbucket/xxx/xxx/xxx.

    This error occurs when the table data on OSS is deleted but the table metadata is not. The table schema continues to exist, but the actual data does not or is moved to another location. In this case, you can change the table location to an existing path and delete the table again.

    `alter table test set location 'oss://your_bucket/your_folder'`

    You can complete this operation on the E-MapReduce interactive console.

    **Note:** oss://your\_bucket/your\_folder must be an existing OSS path.

2.  Wrong FS: hdfs://yourhost:9000/xxx/xxx/xxx.

    This error occurs when the table data in the HDFS is deleted but the table schema is not. The error can be removed by following the preceding solution.

3.  The message **java.lang.IllegalArgumentException: java.net.UnknownHostException: xxxxxxx** is displayed when the Hive database is deleted.

    This error occurs because the Hive database is created in the HDFS of a cluster and is not cleaned when the cluster is released. As a result, its data in the HDFS of the released cluster cannot be accessed after a new cluster is created. Therefore, when releasing a cluster, remember to clean the databases and tables that are manually created in the HDFS of the cluster.

    To resolve this problem, log on to the master node of the cluster using the command line, and find the address, user name, and password used to access the Hive metastore in $HIVE\_CONF\_DIR/hive-site.xml.

    ```
    javax.jdo.option.ConnectionUserName //Username for accessing the database;
    javax.jdo.option.ConnectionPassword //Password for accessing the database;
    javax.jdo.option.ConnectionURL //Address and name of the database ;
    ```

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17932/154814609211097_en-US.png)

    Log on to the Hive metastore on the master node of the cluster:

    ```
    mysql -h ${DBConnectionURL} -u ${ConnectionUserName} -p [Press Enter]
    [Enter the password]${ConnectionPassword}
    ```

    After logging on, change its location to an existing OSS path in the region:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17932/154814609311102_en-US.png)


