# SHOW PARTITIONS {#concept_p2t_cdm_zgb .concept}

## Overview {#section_p5k_ll3_ygb .section}

```
SHOW PARTITIONS FROM table [ WHERE ... ] [ ORDER BY ... ] [ LIMIT ... ]
```

## Description {#section_fym_ml3_ygb .section}

Lists the partitions of a table. You can use the `WHERE` clause to filter conditions, use `ORDER BY` to sort results, and use LIMIT to limit the size of a result set. These clauses are used in the same way as `SELECT`.

## Examples {#section_phk_nl3_ygb .section}

```
--- List all the partitions of the table "orders"
SHOW PARTITIONS FROM orders;

--- List all the partitions of the table "orders" starting from year 2013 until now and sort the partitions by year
SHOW PARTITIONS FROM orders WHERE ds >= '2013-01-01' ORDER BY ds DESC; 

--- List the recent partitions of the table "orders"
SHOW PARTITIONS FROM orders ORDER BY ds DESC LIMIT 10;
```

