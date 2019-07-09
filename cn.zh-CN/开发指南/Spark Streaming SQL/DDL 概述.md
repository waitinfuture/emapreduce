# DDL 概述 {#concept_1062454 .concept}

本文为您介绍 Spark SQL DDL 语法。

## 语法 {#section_itn_ng4_3b7 .section}

``` {#codeblock_txd_5sb_gvj}
CREATE TABLE tbName[(columnName dataType [,columnName dataType]*)]
USING providerName
OPTIONS(propertyName=propertyValue[,propertyName=propertyValue]*);
```

``` {#codeblock_vkg_mny_umi}
-- CTAS

CREATE TABLE tbName[(columnName dataType [,columnName dataType]*)]
USING providerName
OPTIONS(propertyName=propertyValue[,propertyName=propertyValue]*)
AS
queryStatement;
```

CTAS：将创建表和将查询结果写入到表的语句合并到一起，当执行完操作，将创建出表并实际生成一个 StreamQuery 实例，将查询结果写入到结果表中。

## 说明 {#section_am7_2lm_a7t .section}

从语法中可以看出，表的字段信息是可选的，也就是说数据源实现不一样，对于表 schema 定义的要求也可能不一样，具体来说：

-   创建表不指定 schema：会自动识别数据源的 schema 信息。
-   创建表指定 schema：需要是数据源 schema 信息的子集，要求数据类型一致。

举例说明：

-   ``` {#codeblock_50d_q3d_g5m}
CREATE TABLE kafka_table 
USING kafka 
OPTIONS (
kafka.bootstrap.servers = "${BOOTSTRAP_SERVERS}",
subscribe = "${TOPIC_NAME}",
output.mode = "${OUTPUT_MODE}",
kafka.schema.registry.url = "${SCHEMA_REGISTRY_URL}",
kafka.schema.record.name = "${SCHEMA_RECORD_NAME}",
kafka.schema.record.namespace = "${SCHEMA_RECORD_NAMESPACE}");
```

    对于 Kafka 数据源，当不指定表的 schema 信息时，系统会自动从 Kafka Schema Registry 服务中检索对应 topic 的 schema 定义。

-   ``` {#codeblock_298_uo4_sjf}
CREATE TABLE kafka_table(col1 string, col2 double)
USING kafka 
OPTIONS (
kafka.bootstrap.servers = "${BOOTSTRAP_SERVERS}",
subscribe = "${TOPIC_NAME}",
output.mode = "${OUTPUT_MODE}",
kafka.schema.registry.url = "${SCHEMA_REGISTRY_URL}",
kafka.schema.record.name = "${SCHEMA_RECORD_NAME}",
kafka.schema.record.namespace = "${SCHEMA_RECORD_NAMESPACE}");
```

    对于 Kafka 数据源，当指定表的 schema 信息后，系统会自动从 Kafka Schema Registry 服务中检索对应 topic 的 schema 定义，并且和表定义的 schema 信息进行校验，包括字段名和字段类型。建议您在SQL的声明中保持和外部数据存储schema定义一致，包括字段名称和字段类型。


## 数据源 {#section_kor_00x_fce .section}

以下列出了 EMR 支持的数据源：

|数据源|建表带 schema|建表不带 schema|
|---|----------|-----------|
|Kafka|✅|✅|
|Loghub|✅|✅|
|TableStore| |✅|
|HBase| |✅|
|JDBC| |✅|
|Druid| |✅|

