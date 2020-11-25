# Use Hive to query data in JindoFS

Apache Hive is a distributed SQL query engine that is widely used in the Hadoop ecosystem. Hive manages data in databases, tables, and partitions. You can specify the storage location to query data at the storage backend.

## JindoFS configuration

For example, a namespace named emr-jfs is created with the following configuration:

-   jfs.namespaces=emr-jfs
-   jfs.namespaces.emr-jfs.uri=oss://oss-bucket/oss-dir
-   jfs.namespaces.emr-jfs.mode=block

## Specify the storage location for data warehouses, databases, tables, or partitions

-   Specify the storage location for a data warehouse

    The hive-site configuration file contains the hive.metastore.warehouse.dir parameter. This parameter specifies the directory in which a Hive data warehouse stores data. For example, set this parameter to `jfs://emr-jfs/user/hive/warehouse`.

-   Specify the storage location for a database

    A Hive database has a storage location. The location is also used as the default storage location of tables in the database. The parameter that specifies the storage location is optional when you create a database. By default, the storage location of a database is the value of the hive.metastore.warehouse.dir parameter in the hive-site file added with the database name. You can execute the following SQL statements to specify the storage location:

    -   Set the storage location to a directory in JindoFS when you create a database:

        ```
        CREATE DATABASE database_name
        LOCATION
        'jfs://namespace/database_dir';
        ```

        For example, to create a Hive database named database\_on\_jindofs and set the storage location to `jfs://emr-jfs/warehouse/database_on_jindofs`, execute the following statement:

        ```
        CREATE DATABASE database_on_jindofs
        LOCATION
        'jfs://emr-jfs/hive/warehouse/database_on_jindofs';
        ```

    -   Change the storage location of a database to a directory in JindoFS:
        1.  Execute the following SHOW CREATE statement to query the storage location of a database:

            ```
            SHOW CREATE DATABASE database_name;
            ```

        2.  View the returned query result. By default, the storage location of a database is that of its data warehouse added with the database name.

            ```
            CREATE DATABASE `database_name`
            LOCATION
            'hdfs://emr-jfs/user/hive/warehouse/database_name.db'
            ```

        3.  Change the storage location to a directory in JindoFS. This operation does not affect existing tables. If you do not specify the storage location for a new table, the changed location is used as the default storage location of the table.

            For example, to query data in a partition in the jfs\_table\_name table, execute the following statement:

            ```
            ALTER DATABASE database_name SET LOCATION jfs_path;
            ```

-   Specify the storage location for a table or partition

    Similar to the storage location of a database, the storage location of a table or partition is also specified based on the upper-level storage location. Data in a non-partitioned table is stored in the storage location of the table. Data in a partitioned table is stored in the storage location of respective partitions. You can execute the following SQL statements to specify the storage location:

    -   Set the storage location to a directory in JindoFS when you create a table:

        ```
        CREATE [EXTERNAL] TABLE table_name
          [(col_name data_type,...)]
        LOCATION 'jfs://emr-jfs/database_dir/table_dir';
        ```

    -   Change the storage location of a table or partition to a directory in JindoFS:
        1.  Execute the following DESCRIBE statement to query the storage location of a table or partition:

            ```
            DESCRIBE FORMATTED [PARTITION partition_spec] table_name;
            ```

        2.  Change the storage location to a directory in JindoFS:

            ```
            ALTER TABLE table_name [PARTITION partition_spec] SET LOCATION "jfs_path";
            ```

            For example, to query data in a partition in the jfs\_table\_name table, execute the following statement:

            ```
            DESCRIBE FORMATTED jfs_table_name PARTITION (partition_key1=123,partition_key2='xxxx');
            ```


## Query data in a scratch directory

Hive stores temporary output files and job plans to a scratch directory. You can set the hive.exec.scratchdir parameter in the hive-site configuration file to a directory in JindoFS. You can also run one of the following commands to specify this parameter:

```
bin/hive --hiveconf hive.exec.scratchdir=jfs://emr-jfs/scratch_dir
```

and

```
set hive.exec.scratchdir=jfs://emr-jfs/scratch_dir;
```

