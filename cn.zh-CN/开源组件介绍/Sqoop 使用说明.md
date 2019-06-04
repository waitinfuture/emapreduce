# Sqoop 使用说明 {#concept_p22_qkp_y2b .concept}

Sqoop 是一款用来在不同数据存储软件之间进行数据传输的开源软件，它支持多种类型的数据储存软件。

## 安装 Sqoop {#section_clb_zgj_kfb .section}

**说明：** 

目前，E-MapReduce 从版本 1.3 开始都会默认支持 Sqoop 组件，您无需再自行安装，可以跳过本节。

若您使用的 E-MapReduce 的版本低于 1.3，则还没有集成 Sqoop，如果需要使用请参考如下的安装方式。

1.  从官网下载 [Sqoop 1.4.6 版本](http://www.apache.org/dyn/closer.lua/sqoop/1.4.6) 

    若下载的 sqoop-1.4.6.bin\_\_hadoop-2.0.4-alpha.tar.gz 无法打开，也可以使用镜像地址 http://mirror.bit.edu.cn/apache/sqoop/1.4.6/sqoop-1.4.6.bin\_\_hadoop-2.0.4-alpha.tar.gz 进行下载，请执行如下命令：

    ``` {#codeblock_pnd_w58_sqj}
    wget http://mirror.bit.edu.cn/apache/sqoop/1.4.6/sqoop-1.4.6.bin__ha doop-2.0.4-alpha.tar.gz
    ```

2.  执行如下命令，将下载下来的 sqoop-1.4.6.bin\_\_hadoop-2.0.4-alpha.tar.gz 解压到 Master 节点上：

    ```
    tar zxf sqoop-1.4.6.bin__hadoop-2.0.4-alpha.tar.gz
    ```

3.  要从 MySQL 导数据需要安装 MySQL driver。请从官网下载[最新版本](https://dev.mysql.com/downloads/connector/j/)，或执行如下命令进行下载（此处以版本 5.1.38 为例）：

    ```
    wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.38.tar.gz
    ```

4.  解压后将 jar 包放到 Sqoop 目录下的 lib 目录下。

## 数据传输 {#section_hjw_c3j_kfb .section}

常见的使用场景如下所示：

-   MySQL -\> HDFS
-   HDFS -\> MySQL
-   MySQL -\> Hive
-   Hive -\> MySQL
-   使用 SQL 作为导入条件

**说明：** 在执行如下的命令前，请先切换你的用户为 Hadoop。

```
su hadoop
```

-   从 MySQL 到 HDFS

    在集群的 Master 节点上执行如下命令：

    ```
    sqoop import --connect jdbc:mysql://<dburi>/<dbname> --username <username> --password <password> --table <tablename> --check-column <col> --incremental <mode> --last-value <value> --target-dir <hdfs-dir>
    ```

    参数说明：

    -   dburi：数据库的访问连接，例如： jdbc:mysql://192.168.1.124:3306/ 如果您的访问连接中含有参数,那么请用单引号将整个连接包裹住，例如 ：jdbc:mysql://192.168.1.124:3306/mydatabase?useUnicode=true
    -   dbname：数据库的名字，例如：user。
    -   username：数据库登录用户名。
    -   password：用户对应的密码。
    -   tablename：MySQL 表的名字。
    -   col：要检查的列的名称。
    -   mode：该模式决定Sqoop如何定义哪些行为新的行。取值为 append 或 lastmodified。
    -   value：前一个导入中检查列的最大值。
    -   hdfs-dir：HDFS 的写入目录，例如：/user/hive/result。
    更加详细的参数使用请参考 [Sqoop Import](http://sqoop.apache.org/docs/1.4.6/SqoopUserGuide.html#_syntax)。

-   从 HDFS 到 MySQL

    需要先创建好对应 HDFS 中的数据结构的 MySQL 表，然后在集群的 Master 节点上执行如下命令，指定要导的数据文件的路径：

    ```
    sqoop export --connect jdbc:mysql://<dburi>/<dbname> --username <username> --password <password> --table <tablename> --export-dir <hdfs-dir>
    ```

    参数说明：

    -   dburi：数据库的访问连接，例如： jdbc:mysql://192.168.1.124:3306/。如果您的访问连接中含有参数,那么请用单引号将整个连接包裹住，例如：jdbc:mysql://192.168.1.124:3306/mydatabase?useUnicode=true
    -   dbname：数据库的名字，例如：user。
    -   username：数据库登录用户名。
    -   password：用户对应的密码。
    -   tablename：MySQL 的表的名字。
    -   hdfs-dir：要导到 MySQL 去的 HDFS 的数据目录，例如：/user/hive/result。
    更加详细的参数使用请参考 [Sqoop Export](http://sqoop.apache.org/docs/1.4.6/SqoopUserGuide.html#_syntax_4)。

-   从 MySQL 到 Hive

    在集群的 Master 节点上执行如下命令后，从MySQL数据库导入数据的同时，也会新建一个 Hive 表：

    ```
    sqoop import --connect jdbc:mysql://<dburi>/<dbname> --username <username> --password <password> --table <tablename> --check-column <col> --incremental <mode> --last-value <value> --fields-terminated-by "\t" --lines-terminated-by "\n" --hive-import --target-dir <hdfs-dir> --hive-table <hive-tablename>
    ```

    参数说明：

    -   dburi：数据库的访问连接，例如： jdbc:mysql://192.168.1.124:3306/ 如果您的访问连接中含有参数,那么请用单引号将整个连接包裹住，例如：jdbc:mysql://192.168.1.124:3306/mydatabase?useUnicode=true
    -   dbname：数据库的名字，例如：user。
    -   username：据库登录用户名。
    -   password：用户对应的密码。
    -   tablename：MySQL 的表的名字。
    -   col：要检查的列的名称。
    -   mode：该模式决定Sqoop如何定义哪些行为新的行。取值为append或lastmodified。由Sqoop导入数据到Hive不支持append模式。
    -   value：前一个导入中检查列的最大值。
    -   hdfs-dir：要导到 MySQL 去的 HDFS 的数据目录，例如：/user/hive/result。
    -   hive-tablename：对应的 Hive 中的表名，可以是 xxx.yyy。
    更加详细的参数使用请参考 [Sqoop Import](http://sqoop.apache.org/docs/1.4.6/SqoopUserGuide.html#_syntax)。

-   从 Hive 到 MySQL

    请参考上面的从 HDFS 到 MySQL 的命令，只需要指定 Hive 表对应的 HDFS 路径就可以了。

-   从 MySQL 到 OSS

    类似从 MySQL 到 HDFS，只是 —target-dir 不同。在集群的 Master 节点上执行如下命令：

    ```
    sqoop import --connect jdbc:mysql://<dburi>/<dbname> --username <username> --password <password> --table <tablename> --check-column <col> --incremental <mode> --last-value <value> --target-dir <oss-dir> --temporary-rootdir <oss-tmpdir>
    ```

    **说明：** 

    -   OSS 地址中的 host 有内网地址、外网地址和 VPC 网络地址之分。如果用经典网络，需要指定内网地址，杭州是 oss-cn-hangzhou-internal.aliyuncs.com，VPC 要指定 VPC 内网，杭州是 vpc100-oss-cn-hangzhou.aliyuncs.com。
    -   目前同步到 OSS 不支持—delete-target-dir，用这个参数会报错 Wrong FS。如果要覆盖以前目录的数据 ，可以在调用 sqoop 前用hadoop fs -rm -r osspath先把原来的 OSS 目录删了。
    ```
    sqoop import --connect jdbc:mysql://<dburi>/<dbname> --username <username> --password <password> --table <tablename> --check-column <col> --incremental <mode> --last-value <value> --target-dir <oss-dir> --temporary-rootdir <oss-tmpdir>
    ```

    参数说明：

    -   dburi：数据库的访问连接，例如： jdbc:mysql://192.168.1.124:3306/如果您的访问连接中含有参数,那么请用单引号将整个连接包裹住，例如：jdbc:mysql://192.168.1.124:3306/mydatabase?useUnicode=true
    -   dbname：数据库的名字，例如：user。
    -   username：数据库登录用户名。
    -   password：用户对应的密码。
    -   tablename：MySQL 表的名字。
    -   col：要检查的列的名称。
    -   mode：该模式决定Sqoop如何定义哪些行为新的行。取值为append或lastmodified。
    -   value：前一个导入中检查列的最大值。
    -   oss-dir：OSS 的写入目录，例如：oss://<accessid\>:<accesskey\>@<bucketname\>.oss-cn-hangzhou-internal.aliyuncs.com/result。
    -   oss-tmpdir：临时写入目录。指定 append 模式的同时，需要指定该参数。如果目标目录已经存在于 HDFS 中，则 Sqoop 将拒绝导入并覆盖该目录的内容。采用append模式后，Sqoop 会将数据导入临时目录，然后将文件重命名为正常目标目录。
    更加详细的参数使用请参见 [Sqoop Import](http://sqoop.apache.org/docs/1.4.6/SqoopUserGuide.html#_syntax)。

-   从 OSS 到 MySQL

    类似 MySQL 到 HDFS，只是 —export-dir 不同。需要创建好对应 OSS 中的数据结构的 MySQL 表

    然后在集群的 Master 节点上执行如下：指定要导的数据文件的路径：

    ```
    sqoop export --connect jdbc:mysql://<dburi>/<dbname> --username <username> --password <password> --table <tablename> --export-dir <oss-dir>
    ```

    参数说明：

    -   dburi：数据库的访问连接，例如： jdbc:mysql://192.168.1.124:3306/如果您的访问连接中含有参数,那么请用单引号将整个连接包裹住，例如：jdbc:mysql://192.168.1.124:3306/mydatabase?useUnicode=true
    -   dbname：数据库的名字，例如：user。
    -   username：数据库登录用户名。
    -   password：用户对应的密码。
    -   tablename：MySQL 表的名字。
    -   oss-dir：OSS 的写入目录，例如：oss://<accessid\>:<accesskey\>@<bucketname\>.oss-cn-hangzhou-internal.aliyuncs.com/result。
    -   oss-tmpdir：临时写入目录。指定 append 模式的同时，需要指定该参数。如果目标目录已经存在于 HDFS 中，则Sqoop将拒绝导入并覆盖该目录的内容。采用 append 模式后，Sqoop 会将数据导入临时目录，然后将文件重命名为正常目标目录。
    **说明：** OSS 地址 host 有内网地址，外网地址，VPC 网络地址之分。如果 用经典网络，需要指定内网地址，杭州是 oss-cn-hangzhou-internal.aliyuncs.com，VPC 需要指定 VPC 内网，杭州是 vpc100-oss-cn-hangzhou.aliyuncs.com。

    更加详细的参数使用请参见，[Sqoop Export](http://sqoop.apache.org/docs/1.4.6/SqoopUserGuide.html#_syntax_4)

-   使用 SQL 作为导入条件

    除了指定 MySQL 的全表导入，还可以写 SQL 来指定导入的数据，如下所示：

    ```
    sqoop import --connect jdbc:mysql://<dburi>/<dbname> --username <username> --password <password> --query <query-sql> --split-by <sp-column> --hive-import --hive-table <hive-tablename> --target-dir <hdfs-dir>
    ```

    参数说明：

    -   dburi：数据库的访问连接，例如： jdbc:mysql://192.168.1.124:3306/如果您的访问连接中含有参数,那么请用单引号将整个连接包裹住，例如：jdbc:mysql://192.168.1.124:3306/mydatabase?useUnicode=true
    -   dbname：数据库的名字，例如：user。
    -   username：数据库登录用户名。
    -   password：用户对应的密码。
    -   query-sql：使用的查询语句，例如：`SELECT * FROM profile WHERE id>1 AND \$CONDITIONS`。记得要用引号包围，最后一定要带上 `AND \$CONDITIONS`。
    -   sp-column：进行切分的条件,一般跟 MySQL 表的主键。
    -   hdfs-dir：要导到 MySQL 去的 HDFS 的数据目录，例如：/user/hive/result。
    -   hive-tablename：对应的 Hive 中的表名，可以是 xxx.yyy。
    更加详细的参数使用请参见 [Sqoop Query Import](http://sqoop.apache.org/docs/1.4.6/SqoopUserGuide.html#_free_form_query_imports)。


