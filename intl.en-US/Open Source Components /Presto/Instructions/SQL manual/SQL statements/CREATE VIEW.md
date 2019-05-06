# CREATE VIEW {#concept_pdl_bp3_ygb .concept}

## Overview {#section_p5k_ll3_ygb .section}

```
CREATE [ OR REPLACE ] VIEW view_name AS query
```

## Description {#section_fym_ml3_ygb .section}

Creates a view. A view is a logic table that does not contain any data. It can be referenced by future queries. The statement for defining a view is executed each time the query references the view.

Use the `OR REPLACE` clause to suppress the exception that is thrown when the view exists.

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
)
```

