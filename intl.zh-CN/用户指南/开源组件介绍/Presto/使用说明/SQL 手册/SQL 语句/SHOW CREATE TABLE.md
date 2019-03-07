# SHOW CREATE TABLE {#concept_q2d_fcm_zgb .concept}

## 概要 {#section_p5k_ll3_ygb .section}

```
SHOW CREATE TABLE table_name
```

## 描述 {#section_fym_ml3_ygb .section}

显示定义给定表格的 SQL 语句。

## 示例 {#section_phk_nl3_ygb .section}

```
SHOW CREATE TABLE sf1.orders;

-----------------------------------------
 CREATE TABLE tpch.sf1.orders (
    orderkey bigint,
    orderstatus varchar,
    totalprice double,
    orderdate varchar
 )
 WITH (
    format = 'ORC',
    partitioned_by = ARRAY['orderdate']
 )
(1 row)
```

