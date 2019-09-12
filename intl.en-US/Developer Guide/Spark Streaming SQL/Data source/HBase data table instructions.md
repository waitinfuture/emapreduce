# HBase data table instructions {#concept_1181265 .concept}

This topic describes how to use an HBase data table.

## Syntax {#section_d2y_nof_5hv .section}

``` {#codeblock_fih_sbg_m5t}
CREATE TABLE tbName
USING hbase
OPTIONS(propertyName=propertyValue[,propertyName=propertyValue]*);
```

## Configuration parameters {#section_tis_yg9_sss .section}

-   |Parameter|Description|Required|
|---------|-----------|--------|
|catalog|The description of fields in the HBase data table, in JSON format.|Yes|
|hbaseConfiguration|The HBase configuration, in JSON format. For example, set the HBase connection address to '\{"hbase.zookeeper.quorum":"a.b.c.d:2181"\}'.|Yes|

-   A catalog configuration example is as follows:

    ``` {#codeblock_l9e_8qb_0ly}
    {
      "table":{"namespace":"default", "name":"table1"},
      "rowkey":"key",
      "columns":{
        "col0":{"cf":"rowkey", "col":"key", "type":"string"},
        "col1":{"cf":"cf1", "col":"col1", "type":"boolean"},
        "col2":{"cf":"cf2", "col":"col2", "type":"double"},
        "col3":{"cf":"cf3", "col":"col3", "type":"float"},
        "col4":{"cf":"cf4", "col":"col4", "type":"int"},
        "col5":{"cf":"cf5", "col":"col5", "type":"bigint"},
        "col6":{"cf":"cf6", "col":"col6", "type":"smallint"},
        "col7":{"cf":"cf7", "col":"col7", "type":"string"},
        "col8":{"cf":"cf8", "col":"col8", "type":"tinyint"}
      }
    }
    ```

    The preceding example describes the schema of HBase table1. In the example, the rowkey field is set to key, and related information is defined for columns 1 to 8.

    **Note:** You also need to set the cf field to rowkey in column 0.


## Table schema {#section_scq_txs_v2g .section}

When creating an HBase data table, you do not need to explicitly define the fields in the data table. For example, a valid table creation statement is as follows:

``` {#codeblock_ab7_cw2_8ym}
spark-sql> CREATE DATABASE IF NOT EXISTS default;
spark-sql> USE default;
spark-sql> DROP TABLE IF EXISTS hbase_table_test;
spark-sql> CREATE TABLE hbase_table_test
         > USING hbase
         > OPTIONS(
         > catalog='{"table":{"namespace":"default","name":"test"},"rowkey":"key", "columns":{"key":{"cf":"rowkey", "col":"key", "type":"string"},"data":{"cf":"info", "col":"data", "type":"string"}}}',
         > hbaseConfiguration='{"hbase.zookeeper.quorum":"a.b.c.d:2181"}');

spark-sql> DESC hbase_table_test;
key string  NULL
data  string  NULL
Time taken: 0.436 seconds, Fetched 2 row(s)
```

