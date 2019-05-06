# CREATE TABLE {#concept_y2g_d43_ygb .concept}

## Overview {#section_p5k_ll3_ygb .section}

```
CREATE TABLE [ IF NOT EXISTS ]
table_name (
  { column_name data_type [ COMMENT comment ]
  | LIKE existing_table_name [ { INCLUDING | EXCLUDING } PROPERTIES ] }
  [, ...]
)
[ COMMENT table_comment ]
[ WITH ( property_name = expression [, ...] ) ]
```

## Description {#section_fym_ml3_ygb .section}

Creates an empty table. Use `CREATE TABLE AS` to create a table from an existing dataset.

-   Use the `IF NOT EXISTS` clause to suppress the exception that is thrown when the table to be created already exists.

-   Use the `WITH` clause to set properties for the new table. To list all available table properties, run the following statement:

    ```
    SELECT * FROM system.metadata.table_properties;
    
    ```

-   Use the `LIKE` clause to reference the column definitions of an existing table. You can specify multiple `LIKE` clauses.

-   Use `INCLUDING PROPERTIES` to reference the properties of an existing table when creating a table. If the `WITH` clause specifies the same property name as one of the properties that is specified by `INCLUDING PROPERTIES`, the value from the `WITH` clause is used. The default behavior is `EXCLUDING PROPERTIES`.


## Examples {#section_phk_nl3_ygb .section}

```
--- Create a table named "orders"
CREATE TABLE orders (
  orderkey bigint,
  orderstatus varchar,
  totalprice double,
  orderdate date
)
WITH (format = 'ORC')

--- Create a table named "orders" and add a comment
CREATE TABLE IF NOT EXISTS orders (
  orderkey bigint,
  orderstatus varchar,
  totalprice double COMMENT 'Price in cents.',
  orderdate date
)
COMMENT 'A table to keep track of orders.'

--- Create a table named "bigger_orders" and reference some column definitions from the table "orders"
CREATE TABLE bigger_orders (
  another_orderkey bigint,
  LIKE orders,
  another_orderdate date
)
```

