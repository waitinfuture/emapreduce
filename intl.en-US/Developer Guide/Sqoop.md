# Sqoop {#concept_p22_qkp_y2b .concept}

Sqoop is an open-source application that is used to transfer data between different data stores. It supports various data stores.

## Install Sqoop {#section_clb_zgj_kfb .section}

**Note:** 

Sqoop has been integrated with E-MapReduce since E-MapReduce 1.3. If you are using E-MapReduce 1.3 or later, you can skip this section.

If the version you are using is earlier than E-MapReduce 1.3, you can install Sqoop as follows:

1.  Download Sqoop 1.4.6 from the official site \([Click to download](http://www.apache.org/dyn/closer.lua/sqoop/1.4.6)\). If you cannot open the sqoop-1.4.6.bin\_\_hadoop-2.0.4-alpha.tar.gz file that you downloaded, try to download the file from the mirror site http://mirror.bit.edu.cn/apache/sqoop/1.4.6/sqoop-1.4.6.bin\_\_hadoop-2.0.4-alpha.tar.gz by executing the following command.

    ```
    wget http://mirror.bit.edu.cn/apache/sqoop/1.4.6/sqoop-1.4.6.bin__ha doop-2.0.4-alpha.tar.gz
    ```

2.  Execute the following command to extract the sqoop-1.4.6.bin\_\_hadoop-2.0.4-alpha.tar.gz file to the Master node.

    ```
    tar zxf sqoop-1.4.6.bin__hadoop-2.0.4-alpha.tar.gz
    ```

3.  Install the MySQL driver to import data from MySQL. Download the latest version from the official site \([Click to download](https://dev.mysql.com/downloads/connector/j/)\). In addition, you can execute the following command to download the latest version \(take version 5.1.38 as an example\).

    ```
    wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.38.tar.gz
    ```

4.  Extract the jar file to the lib folder in the Sqoop folder.

## Transfer data {#section_hjw_c3j_kfb .section}

Scenarios:

-   MySQL -\> HDFS
-   HDFS -\> MySQL
-   MySQL -\> Hive
-   Hive -\> MySQL
-   Free-form query imports

**Note:** You must switch your user account to hadoop before executing commands in later sections.

```
su hadoop
```

-   Import data from MySQL into HDFS

    Execute the following command on the Master node of the cluster:

    ```
    sqoop import --connect jdbc:mysql://<dburi>/<dbname> --username <username> --password <password> --table <tablename> --check-column <col> --incremental <mode> --last-value <value> --target-dir <hdfs-dir>
    ```

    Parameters:

    -   dburi: the connection string of a database such as jdbc:mysql://192.168.1.124:3306/. When a connection string includes any parameter, you must enclose the connection string within single quotation marks \('\) such as jdbc:mysql://192.168.1.124:3306/mydatabase? useUnicode=true
    -   dbname: the name of a database such as user.
    -   username: the username that is used to log on to a database.
    -   password: the password that username with the username.
    -   tablename: the name of a MySQL table.
    -   col: the name of a column to be queried.
    -   mode: specifies how Sqoop determines which rows are new. Valid values: append and lastmodified.
    -   value : specifies the maximum value of a column to be checked from the previous import.
    -   hdfs-dir: the HDFS directory that you import data into such as/user/hive/result.
    For more information about parameters, see [Sqoop import](http://sqoop.apache.org/docs/1.4.6/SqoopUserGuide.html#_syntax).

-   Import data from HDFS into MySQL

    You must create MySQL tables that comply with the data structure of HDFS in advance. Then you can execute the following command on the Master node of a cluster to specify a directory that you import data into.

    ```
    sqoop export --connect jdbc:mysql://<dburi>/<dbname> --username <username> --password <password> --table <tablename> --export-dir <hdfs-dir>
    ```

    Parameters:

    -   dburi: the connection string of a database such as jdbc:mysql://192.168.1.124:3306/. When any parameter is included in a connection string, enclose the connection string within single quotation marks \('\) such as jdbc:mysql://192.168.1.124:3306/mydatabase? useUnicode=true
    -   dbname: the name of a database, such as user.
    -   username: the username that is used to log on to a database.
    -   password: the password that is associated with the username.
    -   tablename: the name of a MySQL table.
    -   hdfs-dir: the directory of HDFS from which you import data into MySQL such as /user/hive/result.
    For more information about parameters, see [Sqoop export](http://sqoop.apache.org/docs/1.4.6/SqoopUserGuide.html#_syntax_4)

-   Import data from MySQL into Hive

    When you execute the following command on the Master node of a cluster to import data from MySQL, a Hive table will be created as follows.

    ```
    sqoop import --connect jdbc:mysql://<dburi>/<dbname> --username <username> --password <password> --table <tablename> --check-column <col> --incremental <mode> --last-value <value> --fields-terminated-by "\t" --lines-terminated-by "\n" --hive-import --target-dir <hdfs-dir> --hive-table <hive-tablename>
    ```

    Parameters:

    -   dburi: the connection string of a database such as jdbc:mysql://192.168.1.124:3306/. When a connection string includes any parameter, you must enclose the connection string within single quotation marks \('\) such as jdbc:mysql://192.168.1.124:3306/mydatabase? useUnicode=true
    -   dbname: the name of a database such as user.
    -   username: the username that is used to log on to a database.
    -   password: the password that is associated with the username.
    -   tablename: the name of a MySQL table.
    -   col: the name of a column to be queried.
    -   mode: specifies how Sqoop determine which rows are new. Valid values: append and lastmodified. When you import data into Hive by Sqoop, you cannot use the append mode.
    -   value: specifies the maximum value of a column to be queried from the previous import.
    -   hdfs-dir: the directory of HDFS from which you import data into MySQL such as /user/hive/result.
    -   hive-tablename: the table name of Hive such as xxx.yyy.
    For more information about how to use parameters, see [Sqoop import](http://sqoop.apache.org/docs/1.4.6/SqoopUserGuide.html#_syntax).

-   Import data from Hive into MySQL

    You can refer to the previous command that is used to import data from HDFS into MySQL. In addition, you must specify the HDFS directory of Hive tables from which you import data into MySQL.

-   Import data from MySQL into OSS

    The process is similar to importing data from MySQL into HDFS, except for the configuration of the target-dir parameter. You can execute the following command on the Master node of a cluster:

    ```
    sqoop import --connect jdbc:mysql://<dburi>/<dbname> --username <username> --password <password> --table <tablename> --check-column <col> --incremental <mode> --last-value <value> --target-dir <oss-dir> --temporary-rootdir <oss-tmpdir>
    ```

    **Note:** 

    -   The endpoint of an OSS host can be: intranet endpoint, Internet endpoint, or VPC endpoint. For a classic network, you must specify the intranet endpoint. For example, the OSS intranet endpoint of the China \(Hangzhou\) region is oss-cn-hangzhou-internal.aliyuncs.com. For a VPC, you must specify the VPC endpoint. For example, the OSS VPC endpoint of the China \(Hangzhou\) region isvpc100-oss-cn-hangzhou.aliyuncs.com.
    -   Currently, when you import data into OSS, you cannot specify the delete-target-dir parameter. Otherwise, the error message Wrong FS occurs. When you want to overwrite a directory, you can execute the hadoop fs -rm -r osspath command to remove this OSS directory before using Sqoop.
    ```
    sqoop import --connect jdbc:mysql://<dburi>/<dbname> --username <username> --password <password> --table <tablename> --check-column <col> --incremental <mode> --last-value <value> --target-dir <oss-dir> --temporary-rootdir <oss-tmpdir>
    ```

    Parameters:

    -   dburi: the connection string of a database such as jdbc:mysql://192.168.1.124:3306/. When a connection string includes any parameter, enclose the connection string within single quotation marks \('\) such as jdbc:mysql://192.168.1.124:3306/mydatabase? useUnicode=true
    -   dbname: the name of a database such as user.
    -   username: the username that is used to log on to a database.
    -   password: the password that is associated with the username.
    -   tablename: the name of a MySQL table.
    -   col: the name of a column to be queried.
    -   mode: used by Sqoop to determine which rows are new rows. Valid values: append and lastmodified.
    -   value: specifies the maximum value of a column to be queried from the previous import.
    -   oss-dir: the OSS directory that you import data into oss://<accessid\>:<accesskey\>@<bucketname\>.oss-cn-hangzhou-internal.aliyuncs.com/result。
    -   oss-tmpdir: the temporary target folder. You must specify this parameter when you specify the append mode. If the destination directory already exists in HDFS, Sqoop will stop to import and overwrite that directory’s contents. If you specify the append mode, Sqoop will import data to a temporary directory and then rename the files to the normal target directory in a manner that does not conflict with the existing filenames in that directory.
    For more information about available parameters, see [Sqoop import](http://sqoop.apache.org/docs/1.4.6/SqoopUserGuide.html#_syntax).

-   Import data from OSS into MySQL

    The process is similar to importing data from MySQL to HDFS, except for the configuration of the export-dir parameter. You must create MySQL tables that comply with the data structure of OSS in advance.

    Then you can execute the following command on the Master node of a cluster to specify the directory from which you want to import data.

    ```
    sqoop export --connect jdbc:mysql://<dburi>/<dbname> --username <username> --password <password> --table <tablename> --export-dir <oss-dir>
    ```

    Parameters:

    -   dburi: the connection string of a database such as jdbc:mysql://192.168.1.124:3306/. When a connection string includes any parameter, you must enclose this connection string within single quotation marks \('\) such as jdbc:mysql://192.168.1.124:3306/mydatabase? useUnicode=true
    -   dbname: the name of a database such as user.
    -   username: the username that is used to log on to a database.
    -   password: the password that is associated with the username.
    -   tablename: the name of a MySQL table.
    -   oss-dir: the OSS directory that you import data into such asoss://<accessid\>:<accesskey\>@<bucketname\>.oss-cn-hangzhou-internal.aliyuncs.com/result.
    -   oss-tmpdir: the temporary directory that you import data into. You must specify this parameter when you specify the append mode. If the destination directory already exists in HDFS, Sqoop will stop to import and overwrite that directory’s contents. If you specify the append mode, Sqoop will import data to a temporary directory and then rename the files into the normal target directory in a manner that does not conflict with existing filenames in that directory.
    **Note:** The endpoint of an OSS host can be: intranet endpoint, Internet endpoint, or VPC endpoint. For a classic network, you must specify an intranet endpoint. For example, the OSS intranet endpoint of the China \(Hangzhou\) region is oss-cn-hangzhou-internal.aliyuncs.com. For a VPC, you must specify a VPC endpoint. For example, the OSS VPC endpoint of the China \(Hangzhou\) region isvpc100-oss-cn-hangzhou.aliyuncs.com.

    For more information about available parameters, see [Sqoop export](http://sqoop.apache.org/docs/1.4.6/SqoopUserGuide.html#_syntax_4).

-   Free-form query imports

    In addition to importing a set of MySQL tables, you can also import the result set of an arbitrary SQL query as follows:

    ```
    sqoop import --connect jdbc:mysql://<dburi>/<dbname> --username <username> --password <password> --query <query-sql> --split-by <sp-column> --hive-import --hive-table <hive-tablename> --target-dir <hdfs-dir>
    ```

    Parameters:

    -   dburi: the connection string of a database, such as jdbc:mysql://192.168.1.124:3306/. When a connection string includes any parameter, you must enclose this connection string within single quotation marks \('\) such as jdbc:mysql://192.168.1.124:3306/mydatabase? useUnicode=true
    -   dbname: the name of a database such as user.
    -   username: the username that is used to log on to a database.
    -   password: the password that is associated with the username.
    -   query-sql: the query statement such as `SELECT * FROM profile WHERE id>1 AND \$CONDITIONS`. You must enclose the query statement that ends with `AND \$CONDITIONS` within double quotation marks \("\).
    -   sp-column: specifies the name of a column to be split. In general, the value of this parameter is the primary key of the MySQL table.
    -   hdfs-dir: the directory of HDFS from which you import data into MySQL such as /user/hive/result.
    -   hive-tablename: the name of a table that is used to import data to Hive such as xxx.yyy.
    For more information about available parameters, see [Sqoop query import](http://sqoop.apache.org/docs/1.4.6/SqoopUserGuide.html#_free_form_query_imports).


