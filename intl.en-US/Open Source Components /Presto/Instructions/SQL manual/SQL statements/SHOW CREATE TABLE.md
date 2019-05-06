# SHOW CREATE TABLE {#concept_q2d_fcm_zgb .concept}

## Overview {#section_p5k_ll3_ygb .section}

```
SHOW CREATE TABLE table_name
```

## Description {#section_fym_ml3_ygb .section}

Shows the SQL statement that creates the specified table.

## Examples {#section_phk_nl3_ygb .section}

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

