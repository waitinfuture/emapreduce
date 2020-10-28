# Sqoop

Sqoop是一款用于在不同数据存储软件之间进行数据传输的开源软件，支持多种类型的数据储存软件。

## 数据传输

常见使用场景如下：

-   [将MySQL数据导入HDFS](#section_qah_lay_0u0)
-   [将HDFS数据导入MySQL](#section_607_zg8_nmn)
-   [将Hive数据导入MySQL](#section_9p0_b3o_cot)
-   [将MySQL数据导入Hive](#section_4vp_qq7_cgz)
-   [将MySQL数据导入OSS](#section_dov_9sa_qqm)
-   [将OSS数据导入MySQL](#section_s6h_qu7_wds)
-   [使用SQL作为导入条件](#section_brq_57f_r8j)

**说明：** 在数据迁移前，请切换您的用户为hadoop。

## 将MySQL数据导入HDFS

在Master节点上执行如下命令。

```
sqoop import --connect jdbc:mysql://<dburi>/<dbname> --username <username> --password <password> --table <tablename> --check-column <col> --incremental <mode> --last-value <value> --target-dir <hdfs-dir>
```

|参数|描述|
|--|--|
|`dburi`|数据库的访问链接。例如jdbc:mysql://192.168.xxx.xxx:3306/。|
|`dbname`|数据库的名称。例如user。|
|`username`|数据库登录用户名。|
|`password`|用户名密码。|
|`tablename`|MySQL表的名称。|
|`col`|迁移表中列的名称。|
|`mode`|-   append
-   lastmodified |
|`value`|前一个导入中检查列的最大值。|
|`hdfs-dir`|HDFS的写入目录。例如/user/hive/result。|

详细的参数信息请参见[Sqoop Import](http://sqoop.apache.org/docs/1.4.6/SqoopUserGuide.html#_syntax)。

## 将HDFS数据导入MySQL

创建好对应HDFS中的数据结构的MySQL表后，在集群的Master节点上执行如下命令。

```
sqoop export --connect jdbc:mysql://<dburi>/<dbname> --username <username> --password <password> --table <tablename> --export-dir <hdfs-dir>
```

|参数|描述|
|--|--|
|`dburi`|数据库的访问链接。例如jdbc:mysql://192.168.xxx.xxx:3306/。|
|`dbname`|数据库的名称。例如user。|
|`username`|数据库登录用户名。|
|`password`|用户名密码。|
|`tablename`|MySQL表的名称。|
|`hdfs-dir`|HDFS的写入目录。例如/user/hive/result。|

详细的参数信息请参见[Sqoop Export](http://sqoop.apache.org/docs/1.4.6/SqoopUserGuide.html#_syntax_4)。

## 将MySQL数据导入Hive

在集群的Master节点上执行如下命令。

```
sqoop import --connect jdbc:mysql://<dburi>/<dbname> --username <username> --password <password> --table <tablename> --check-column <col> --incremental <mode> --last-value <value> --fields-terminated-by "\t" --lines-terminated-by "\n" --hive-import --target-dir <hdfs-dir> --hive-table <hive-tablename>
```

|参数|描述|
|--|--|
|`dburi`|数据库的访问链接。例如jdbc:mysql://192.168.xxx.xxx:3306/。|
|`dbname`|数据库的名称。例如user。|
|`username`|数据库登录用户名。|
|`password`|用户名密码。|
|`tablename`|MySQL表的名称。|
|`col`|迁移表中列的名称。|
|`mode`|-   append
-   lastmodified |
|`value`|前一个导入中检查列的最大值。|
|`hdfs-dir`|HDFS的写入目录。例如/user/hive/result。|
|`hive-tablename`|Hive中的表名。|

详细的参数信息请参见[Sqoop Import](http://sqoop.apache.org/docs/1.4.6/SqoopUserGuide.html#_syntax)。

## 将Hive数据导入MySQL

执行命令与导入HDFS数据至MySQL一致，但需要指定Hive表对应的HDFS路径。详情请参见[将HDFS数据导入MySQL](#section_607_zg8_nmn)。

## 将MySQL数据导入OSS

在集群的Master节点上执行如下命令。

```
sqoop import --connect jdbc:mysql://<dburi>/<dbname> --username <username> --password <password> --table <tablename> --check-column <col> --incremental <mode> --last-value <value> --target-dir <oss-dir> --temporary-rootdir <oss-tmpdir>
```

**说明：**

-   OSS地址中的host有内网地址、外网地址和VPC网络地址之分。如果您使用经典网络，需要指定内网地址，杭州是oss-cn-hangzhou-internal.aliyuncs.com。如果使用VPC网络，杭州是vpc100-oss-cn-hangzhou.aliyuncs.com。
-   同步到OSS时不支持`—delete-target-dir`参数。如果需要覆盖以前目录的数据 ，可以在调用Sqoop前用，使用`hadoop fs -rm -r osspath`删除原来的OSS目录。

```
sqoop import --connect jdbc:mysql://<dburi>/<dbname> --username <username> --password <password> --table <tablename> --check-column <col> --incremental <mode> --last-value <value> --target-dir <oss-dir> --temporary-rootdir <oss-tmpdir>
```

|参数|描述|
|--|--|
|`dburi`|数据库的访问链接。例如jdbc:mysql://192.168.xxx.xxx:3306/。|
|`dbname`|数据库的名称。例如user。|
|`username`|数据库登录用户名。|
|`password`|用户名密码。|
|`tablename`|MySQL表的名称。|
|`mode`|-   append
-   lastmodified |
|`value`|前一个导入中检查列的最大值。|
|`oss-dir`|OSS的写入目录。例如oss://<accessid\>:<accesskey\>@<bucketname\>.oss-cn-hangzhou-internal.aliyuncs.com/result。|
|`oss-tmpdir`|临时写入目录。指定`mode`为append模式时，需要指定该参数。采用append模式后，Sqoop会先将数据导入临时目录，然后将文件重命名为正常目标目录。如果目标目录已经存在于HDFS中，则Sqoop拒绝导入并覆盖该目录的内容。 |

详细的参数信息请参见[Sqoop Import](http://sqoop.apache.org/docs/1.4.6/SqoopUserGuide.html#_syntax)。

## 将OSS数据导入MySQL

创建好对应OSS中数据结构的MySQL表后，在集群的Master节点上执行如下命令。

```
sqoop export --connect jdbc:mysql://<dburi>/<dbname> --username <username> --password <password> --table <tablename> --export-dir <oss-dir>
```

|参数|描述|
|--|--|
|`dburi`|数据库的访问链接。例如jdbc:mysql://192.168.xxx.xxx:3306/。|
|`dbname`|数据库的名称。例如user。|
|`username`|数据库登录用户名。|
|`password`|用户名密码。|
|`tablename`|MySQL表的名称。|
|`sp-column`|进行切分的条件。通常跟MySQL表的主键有关。|
|`oss-dir`|OSS的写入目录。例如oss://<accessid\>:<accesskey\>@<bucketname\>.oss-cn-hangzhou-internal.aliyuncs.com/result。|
|`oss-tmpdir`|临时写入目录。指定`mode`为append模式时，需要指定该参数。采用append模式后，Sqoop会先将数据导入临时目录，然后将文件重命名为正常目标目录。如果目标目录已经存在于HDFS中，则Sqoop拒绝导入并覆盖该目录的内容。 |

详细的参数信息请参见[Sqoop Export](http://sqoop.apache.org/docs/1.4.6/SqoopUserGuide.html#_syntax_4)。

## 使用SQL作为导入条件

命令和参数如下所示。

```
sqoop import --connect jdbc:mysql://<dburi>/<dbname> --username <username> --password <password> --query <query-sql> --split-by <sp-column> --hive-import --hive-table <hive-tablename> --target-dir <hdfs-dir>
```

|参数|描述|
|--|--|
|`dburi`|数据库的访问链接。例如jdbc:mysql://192.168.xxx.xxx:3306/。|
|`dbname`|数据库的名称。例如user。|
|`username`|数据库登录用户名。|
|`password`|用户名密码。|
|`query-sql`|使用的查询语句。例如`SELECT * FROM profile WHERE id>1 AND \$CONDITIONS`。|
|`sp-column`|进行切分的条件。通常跟MySQL表的主键有关。|
|`hdfs-dir`|HDFS的写入目录。例如/user/hive/result。|
|`hive-tablename`|Hive中的表名。|

详细的参数信息请参见[Sqoop Query Import](http://sqoop.apache.org/docs/1.4.6/SqoopUserGuide.html#_free_form_query_imports)。

