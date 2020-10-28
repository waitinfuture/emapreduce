# Sqoop

Sqoop is open source software that is used to transfer data between various data storage software.

## Data transfer scenarios

Sqoop is commonly used to transfer data in the following scenarios:

-   [Import data from MySQL to HDFS](#section_qah_lay_0u0)
-   [Import data from HDFS to MySQL](#section_607_zg8_nmn)
-   [Import data from Hive to MySQL](#section_9p0_b3o_cot)
-   [Import data from MySQL to Hive](#section_4vp_qq7_cgz)
-   [Import data from MySQL to OSS](#section_dov_9sa_qqm)
-   [Import data from OSS to MySQL](#section_s6h_qu7_wds)
-   [Use an SQL statement to import data](#section_brq_57f_r8j)

**Note:** Before you start data transfer, switch to the hadoop user.

## Import data from MySQL to HDFS

Run the following command on the master node of a cluster:

```
sqoop import --connect jdbc:mysql://<dburi>/<dbname> --username <username> --password <password> --table <tablename> --check-column <col> --incremental <mode> --last-value <value> --target-dir <hdfs-dir>
```

|Parameter|Description|
|---------|-----------|
|`dburi`|The access URL of the database. Example: jdbc:mysql://192.168.xxx.xxx:3306/.|
|`dbname`|The name of the database. Example: user.|
|`username`|The username that is used to log on to the database.|
|`password`|The password that is used to log on to the database.|
|`tablename`|The name of the MySQL table.|
|`col`|The name of a column in the table.|
|`mode`|-   append
-   lastmodified |
|`value`|The maximum value of a column to be checked from the previous import task.|
|`hdfs-dir`|The HDFS directory. Example: /user/hive/result.|

For more information about the parameters, see the [Sqoop Import](http://sqoop.apache.org/docs/1.4.6/SqoopUserGuide.html#_syntax) section of Sqoop User Guide.

## Import data from HDFS to MySQL

After you create MySQL tables that comply with the data structure of HDFS, run the following command on the master node of the cluster:

```
sqoop export --connect jdbc:mysql://<dburi>/<dbname> --username <username> --password <password> --table <tablename> --export-dir <hdfs-dir>
```

|Parameter|Description|
|---------|-----------|
|`dburi`|The access URL of the database. Example: jdbc:mysql://192.168.xxx.xxx:3306/.|
|`dbname`|The name of the database. Example: user.|
|`username`|The username that is used to log on to the database.|
|`password`|The password that is used to log on to the database.|
|`tablename`|The name of the MySQL table.|
|`hdfs-dir`|The HDFS directory. Example: /user/hive/result.|

For more information about the parameters, see the [Sqoop Export](http://sqoop.apache.org/docs/1.4.6/SqoopUserGuide.html#_syntax_4) section of Sqoop User Guide.

## Import data from MySQL to Hive

Run the following command on the master node of a cluster:

```
sqoop import --connect jdbc:mysql://<dburi>/<dbname> --username <username> --password <password> --table <tablename> --check-column <col> --incremental <mode> --last-value <value> --fields-terminated-by "\t" --lines-terminated-by "\n" --hive-import --target-dir <hdfs-dir> --hive-table <hive-tablename>
```

|Parameter|Description|
|---------|-----------|
|`dburi`|The access URL of the database. Example: jdbc:mysql://192.168.xxx.xxx:3306/.|
|`dbname`|The name of the database. Example: user.|
|`username`|The username that is used to log on to the database.|
|`password`|The password that is used to log on to the database.|
|`tablename`|The name of the MySQL table.|
|`col`|The name of a column in the table.|
|`mode`|-   append
-   lastmodified |
|`value`|The maximum value of a column to be checked from the previous import task.|
|`hdfs-dir`|The HDFS directory. Example: /user/hive/result.|
|`hive-tablename`|The name of the Hive table.|

For more information about the parameters, see the [Sqoop Import](http://sqoop.apache.org/docs/1.4.6/SqoopUserGuide.html#_syntax) section of Sqoop User Guide.

## Import data from Hive to MySQL

You can refer to the previous command that is used to import data from HDFS to MySQL. You must specify the HDFS directory of the Hive tables from which you import data to MySQL. For more information, see [Import data from HDFS to MySQL](#section_607_zg8_nmn).

## Import data from MySQL to OSS

Run the following command on the master node of a cluster:

```
sqoop import --connect jdbc:mysql://<dburi>/<dbname> --username <username> --password <password> --table <tablename> --check-column <col> --incremental <mode> --last-value <value> --target-dir <oss-dir> --temporary-rootdir <oss-tmpdir>
```

**Note:**

-   The endpoint of OSS can be an internal endpoint, public endpoint, or VPC endpoint. If the classic network is used, you must specify an internal endpoint. For example, the internal endpoint in the China \(Hangzhou\) region is oss-cn-hangzhou-internal.aliyuncs.com. If a VPC is used, the endpoint in the China \(Hangzhou\) region is vpc100-oss-cn-hangzhou.aliyuncs.com.
-   When you import data into OSS, you cannot specify the `-delete-target-dir` parameter. If you want to overwrite an OSS directory, you can run the `hadoop fs -rm -r osspath` command to remove this directory before you use Sqoop.

```
sqoop import --connect jdbc:mysql://<dburi>/<dbname> --username <username> --password <password> --table <tablename> --check-column <col> --incremental <mode> --last-value <value> --target-dir <oss-dir> --temporary-rootdir <oss-tmpdir>
```

|Parameter|Description|
|---------|-----------|
|`dburi`|The access URL of the database. Example: jdbc:mysql://192.168.xxx.xxx:3306/.|
|`dbname`|The name of the database. Example: user.|
|`username`|The username that is used to log on to the database.|
|`password`|The password that is used to log on to the database.|
|`tablename`|The name of the MySQL table.|
|`mode`|-   append
-   lastmodified |
|`value`|The maximum value of a column to be checked from the previous import task.|
|`oss-dir`|The OSS directory. Example: oss://<accessid\>:<accesskey\>@<bucketname\>.oss-cn-hangzhou-internal.aliyuncs.com/result.|
|`oss-tmpdir`|The temporary OSS directory. If `mode` is set to append, you must specify this parameter.If you specify the append mode, Sqoop imports data to a temporary directory and renames the files into the destination directory in a manner that does not conflict with existing filenames in that directory. If the destination directory already exists in HDFS, Sqoop does not import data into the directory or overwrite data in the directory. |

For more information about the parameters, see the [Sqoop Import](http://sqoop.apache.org/docs/1.4.6/SqoopUserGuide.html#_syntax) section of Sqoop User Guide.

## Import data from OSS to MySQL

After you create MySQL tables that comply with the data structure of OSS, run the following command on the master node of the cluster:

```
sqoop export --connect jdbc:mysql://<dburi>/<dbname> --username <username> --password <password> --table <tablename> --export-dir <oss-dir>
```

|Parameter|Description|
|---------|-----------|
|`dburi`|The access URL of the database. Example: jdbc:mysql://192.168.xxx.xxx:3306/.|
|`dbname`|The name of the database. Example: user.|
|`username`|The username that is used to log on to the database.|
|`password`|The password that is used to log on to the database.|
|`tablename`|The name of the MySQL table.|
|`sp-column`|The name of a column to be split. In most cases, the value of this parameter is the primary key of the MySQL table.|
|`oss-dir`|The OSS directory. Example: oss://<accessid\>:<accesskey\>@<bucketname\>.oss-cn-hangzhou-internal.aliyuncs.com/result.|
|`oss-tmpdir`|The temporary OSS directory. If `mode` is set to append, you must specify this parameter.If you specify the append mode, Sqoop imports data to a temporary directory and renames the files into the destination directory in a manner that does not conflict with existing filenames in that directory. If the destination directory already exists in HDFS, Sqoop does not import data into the directory or overwrite data in the directory. |

For more information about the parameters, see the [Sqoop export](http://sqoop.apache.org/docs/1.4.6/SqoopUserGuide.html#_syntax_4) section of Sqoop User Guide.

## Use an SQL statement to import data

Run the following command:

```
sqoop import --connect jdbc:mysql://<dburi>/<dbname> --username <username> --password <password> --query <query-sql> --split-by <sp-column> --hive-import --hive-table <hive-tablename> --target-dir <hdfs-dir>
```

|Parameter|Description|
|---------|-----------|
|`dburi`|The access URL of the database. Example: jdbc:mysql://192.168.xxx.xxx:3306/.|
|`dbname`|The name of the database. Example: user.|
|`username`|The username that is used to log on to the database.|
|`password`|The password that is used to log on to the database.|
|`query-sql`|The SQL statement that is used to select the data that you want to import. Example: `SELECT * FROM profile WHERE id>1 AND \$CONDITIONS`.|
|`sp-column`|The name of a column to be split. In most cases, the value of this parameter is the primary key of the MySQL table.|
|`hdfs-dir`|The HDFS directory. Example: /user/hive/result.|
|`hive-tablename`|The name of the Hive table.|

For more information about the parameters, see the [Sqoop Query Import](http://sqoop.apache.org/docs/1.4.6/SqoopUserGuide.html#_free_form_query_imports) section of Sqoop User Guide.

