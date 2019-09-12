# JDBC data table instructions {#concept_1181285 .concept}

This topic describes how to use a JDBC data table.

## Syntax {#section_st3_3t2_djz .section}

``` {#codeblock_8wy_lg0_wig}
CREATE TABLE tbName
USING jdbc2
OPTIONS(propertyName=propertyValue[,propertyName=propertyValue]*);
```

**Note:** In the preceding syntax, jdbc2 is used.

## Configuration parameters {#section_hb0_au4_4wv .section}

|Parameter|Description|Required|
|---------|-----------|--------|
|url|The URL of the database.|Yes|
|driver|The driver used to establish a database connection. You can set this parameter to "com.mysql.jdbc.Driver"eper.quorum":"a.b.c.d:2181"\}'.|Yes|
|dbtable|The name of the data table.|Yes|
|user|The username used to connect to the database.|Yes|
|password|The password used to connect to the database.|Yes|
|batchsize|The number of pieces of data updated to the database at a time. This parameter takes effect when data is written to the database.|No|
|isolationLevel|The level of transaction isolation. Default value: READ\_UNCOMMITTED.|No|

-   Levels of transaction isolation

    |Level of transaction isolation|Dirty read|Non-repeatable read|Phantom read|
    |------------------------------|----------|-------------------|------------|
    |READ\_UNCOMMITTED|Yes|Yes|Yes|
    |READ\_COMMITTED|No|Yes|Yes|
    |REPEATABLE\_READ|No|No|Yes|
    |SERIALIZABLE|No|No|No|
    |NONE|-|-|-|


## Table schema {#section_b8f_33y_280 .section}

When creating a JDBC data table, you do not need to explicitly define the fields in the data table. For example, a valid table creation statement is as follows:

``` {#codeblock_dvj_3yq_n0a}
spark-sql> CREATE DATABASE IF NOT EXISTS default;
spark-sql> USE default;
spark-sql> DROP TABLE IF EXISTS rds_table_test;
spark-sql> CREATE TABLE rds_table_test
         > USING jdbc2
         > OPTIONS (
         > url="jdbc:mysql://rm-bp11*********i7w9.mysql.rds.aliyuncs.com:3306/default? useSSL=true",
         > driver="com.mysql.jdbc.Driver",
         > dbtable="test",
         > user="root",
         > password="thisisapassword",
         > batchsize="100",
         > isolationLevel="NONE");

spark-sql> DESC rds_table_test;
id  int NULL
name  string  NULL
Time taken: 0.413 seconds, Fetched 2 row(s)
```

## Data write {#section_bq3_1h3_2vj .section}

You need to create the following additional SQL statement to express how to write data to the database:

``` {#codeblock_qlx_idi_018}
spark-sql> SET streaming.query.${queryName}.sql=insert into `test` (`id`,`name`) values(?, ?) ;
spark-sql> SET ...
spark-sql> INSERT INTO rds_table_test SELECT ... 
```

