# Table Store data table instructions {#concept_1198101 .concept}

This topic describes how to use a Table Store data table.

## Syntax {#section_p2x_8ji_own .section}

``` {#codeblock_p95_ndu_7sy}
CREATE TABLE tbName
USING tablestore
OPTIONS(propertyName=propertyValue[,propertyName=propertyValue]*);
```

## Configuration parameters {#section_egg_qd0_7gs .section}

|Parameter|Description|Required|
|---------|-----------|--------|
|access.key.id|The AccessKey ID provided by Alibaba Cloud.|Yes|
|access.key.secret|The AccessKey secret provided by Alibaba Cloud.|Yes|
|endpoint|The endpoint of the Table Store API.|Yes|
|table.name|The name of the Table Store data table.|Yes|
|instance.name|The name of the Table Store instance.|Yes|
|batch.update.size|The number of pieces of data updated to Table Store at a time. This parameter takes effect when data is written to the database. The default value is 0, which indicates that data is updated piece by piece. We recommend that you do not set the parameter to a small number.|No|
|catalog|The description of fields in the Table Store data table, in JSON format.|Yes|

-   A catalog configuration example is as follows:

    ``` {#codeblock_dsj_lqv_mil}
    {"columns":{
      "col0":{"cf":"cf0", "col":"col0", "type":"string"},
      "col1":{"cf":"cf1", "col":"col1", "type":"boolean"},
      "col2":{"cf":"cf2", "col":"col2", "type":"double"},
      "col3":{"cf":"cf3", "col":"col3", "type":"float"},
      "col4":{"cf":"cf4", "col":"col4", "type":"int"},
      "col5":{"cf":"cf5", "col":"col5", "type":"bigint"},
      "col6":{"cf":"cf6", "col":"col6", "type":"smallint"},
      "col7":{"cf":"cf7", "col":"col7", "type":"string"},
      "col8":{"cf":"cf8", "col":"col8", "type":"tinyint"}
    }
    ```

    The preceding example describes the schema of Table Store table1. In the example, related information is defined for columns 1 to 8.


## Table schema {#section_iwn_ijh_plg .section}

When creating a Table Store data table, you do not need to explicitly define the fields in the data table. For example, a valid table creation statement is as follows:

``` {#codeblock_s1f_n4m_wfl}
spark-sql> CREATE DATABASE IF NOT EXISTS default;
spark-sql> USE default;
spark-sql> DROP TABLE IF EXISTS ots_table_test;
spark-sql> CREATE TABLE ots_table_test
         > USING tablestore
         > OPTIONS(
         > endpoint="http://xxx.cn-hangzhou.vpc.ots.aliyuncs.com",
         > access.key.id="yHiu*******BG2s",
         > access.key.secret="ABctuw0M***************iKKljZy",
         > table.name="test",
         > instance.name="myInstance",
         > batch.update.size="100",
         > catalog='{"columns":{"pk":{"col":"pk","type":"string"},"data":{"col":"data","type":"string"}}}');

spark-sql> DESC ots_table_test;
pk  string  NULL
data  string  NULL
Time taken: 0.501 seconds, Fetched 2 row(s)
```

