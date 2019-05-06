# CREATE TABLE AS {#concept_r5f_n43_ygb .concept}

## Overview {#section_p5k_ll3_ygb .section}

```
CREATE TABLE [ IF NOT EXISTS ] table_name [ ( column_alias, ... ) ]
[ COMMENT table_comment ]
[ WITH ( property_name = expression [, ...] ) ]
AS query
[ WITH [ NO ] DATA ]
```

## Description {#section_fym_ml3_ygb .section}

Creates a new table that contains data from a `SELECT` query.

-   Use the `IF NOT EXISTS` clause to suppress the exception that is thrown when the table to be created already exists.

-   Use the `WITH` clause to set properties for the new table. To list all available table properties, run the following statement:

    ```
    `SELECT * FROM system.metadata.table_properties;`
    ```


## Examples {#section_phk_nl3_ygb .section}

```
--- Select two columns from the table "orders" to create a table
CREATE TABLE orders_column_aliased (order_date, total_price)
AS
SELECT orderdate, totalprice
FROM orders

--- Create a table by using an aggregate function
CREATE TABLE orders_by_date
COMMENT 'Summary of orders by date'
WITH (format = 'ORC')
AS
SELECT orderdate, sum(totalprice) AS price
FROM orders
GROUP BY orderdate

--- Create a table by using the `IF NOT EXISTS` clause
CREATE TABLE IF NOT EXISTS orders_by_date AS
SELECT orderdate, sum(totalprice) AS price
FROM orders
GROUP BY orderdate

--- Create a table that has the same schema as the table "nation" but contains no data
CREATE TABLE empty_nation AS
SELECT *
FROM nation
WITH NO DATA

```

