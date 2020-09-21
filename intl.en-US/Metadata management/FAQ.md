# FAQ

This topic provides answers to some commonly asked questions about metadata management in EMR.

-   [How do I fix the error "oss://yourbucket/xxx/xxx/xxx" or "hdfs://yourhost:9000/xxx/xxx/xxx"?](#section_8kc_5dn_ro5)
-   [What do I do if the error "java.lang.IllegalArgumentException: java.net.UnknownHostException: xxxxxxx" is reported when I delete a Hive database?](#section_05k_qv9_2y8)

## How do I fix the error "oss://yourbucket/xxx/xxx/xxx" or "hdfs://yourhost:9000/xxx/xxx/xxx"?

Cause: The data of a table in OSS has been deleted or moved to another path, but the schema of the table still exists.

Solution: Change the table location to an existing path and delete the table.

```
alter table test set location 'oss://your_bucket/your_folder'
```

**Note:** oss://your\_bucket/your\_folder in the command must be an existing OSS path.

## What do I do if the error "java.lang.IllegalArgumentException: java.net.UnknownHostException: xxxxxxx" is reported when I delete a Hive database?

Cause: A Hive database is created in HDFS of a cluster, and it is not deleted when the cluster is released. As a result, the database cannot be accessed after another cluster is created. When you release a cluster, you must delete the databases and tables that you manually created for the cluster from HDFS.

Solution:

1.  Log on to the master node of the cluster. Find the URL, username, and password of the Hive database in the $HIVE\_CONF\_DIR/hivemetastore-site.xml file.

    ```
    javax.jdo.option.ConnectionUserName // The username of the database
    javax.jdo.option.ConnectionPassword // The password of the database
    javax.jdo.option.ConnectionURL // The URL and name of the database
    ```

    ![delete_hive_database](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2311549951/p11097.png)

2.  Log on to the Hive database from the master node.

    ```
    mysql -h ${DBConnectionURL} -u ${ConnectionUserName} -p [Press Enter]
    [Enter the password]${ConnectionPassword}
    ```

3.  Change the value of the DB\_LOCATION\_URI parameter to an existing OSS path in the region.

    ![hive_meta_db](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/3311549951/p11102.png)


