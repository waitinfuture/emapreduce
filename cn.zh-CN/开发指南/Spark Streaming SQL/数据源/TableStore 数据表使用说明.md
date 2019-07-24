# TableStore 数据表使用说明 {#concept_1198101 .concept}

本文为您介绍如何使用 TableStore 数据表。

## 建表语法 {#section_p2x_8ji_own .section}

``` {#codeblock_p95_ndu_7sy}
CREATE TABLE tbName
USING tablestore
OPTIONS(propertyName=propertyValue[,propertyName=propertyValue]*);
```

## 配置参数说明 {#section_egg_qd0_7gs .section}

|参数名|说明|是否必选|
|---|--|----|
|access.key.id|阿里云 AccessKeyId|是|
|access.key.secret|阿里云 AccessKeySecret|是|
|endpoint|TableStore API endpoint|是|
|table.name|TableStore 的表名|是|
|instance.name|TableStore 的实例名|是|
|batch.update.size|向数据库写入数据时生效，表示每次向 TableStore 批量更新多少条数据，默认为 0 表示逐条更新，建议不要设置的太小。|否|
|catalog|TableStore 表字段说明，JSON 格式。|是|

-   catalog 配置示例如下：

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

    以上定义了一个 TableStore 表 table1 的 schema，有 col1~col8 等定义的 8 列信息。


## Table Schema {#section_iwn_ijh_plg .section}

创建 TableStore 表时，无须显式地定义表的字段信息，例如：

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

