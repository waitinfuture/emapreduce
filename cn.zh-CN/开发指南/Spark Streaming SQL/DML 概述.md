# DML 概述 {#concept_1062482 .concept}

本文为您介绍如何在 Spark SQL 流式处理中使用 INSERT INTO 语句。

## 语法 {#section_g7z_are_yf4 .section}

``` {#codeblock_ebt_vxm_7dh}
INSERT INTO tbName[(columnName[,columnName]*)]
queryStatement;
```

## 示例 {#section_6g3_b5k_5qi .section}

``` {#codeblock_br3_sdv_cxz}
INSERT INTO LargeOrders
SELECT * FROM Orders WHERE units > 1000;
```

## 说明 {#section_neb_4r4_iu0 .section}

-   不支持单独的 SELECT 查询，必须有 CTAS 或这是在 INSERT INTO ············内才能操作。
-   单个作业支持在一个 SQL 文件里面包含多个 DML 操作，允许包含多个数据源和多个数据目标端。

