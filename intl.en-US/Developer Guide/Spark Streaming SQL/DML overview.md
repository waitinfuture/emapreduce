# DML overview {#concept_1062482 .concept}

This topic describes how to use the INSERT INTO statement in Spark Streaming SQL.

## Syntax {#section_g7z_are_yf4 .section}

``` {#codeblock_ebt_vxm_7dh}
INSERT INTO tbName[(columnName[,columnName]*)]
queryStatement;
```

## Example {#section_6g3_b5k_5qi .section}

``` {#codeblock_br3_sdv_cxz}
INSERT INTO LargeOrders
SELECT * FROM Orders WHERE units > 1000;
```

## Note {#section_neb_4r4_iu0 .section}

-   Spark Streaming SQL does not allow you to use a separate SELECT statement for query. A SELECT statement must be used together with a CTAS statement or be contained in an INSERT INTO statement.
-   For a single job, an SQL file can contain multiple Data Manipulation Language \(DML\) operations, and multiple data sources and sinks.

