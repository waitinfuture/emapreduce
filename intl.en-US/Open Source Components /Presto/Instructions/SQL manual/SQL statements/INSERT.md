# INSERT {#concept_eks_cck_zgb .concept}

## Overview {#section_p5k_ll3_ygb .section}

```
INSERT INTO table_name [ ( column [, ... ] ) ] query
```

## Description {#section_fym_ml3_ygb .section}

Inserts data. If a list of column names is specified, they must exactly match the list of columns returned by the `query`. Each column in the table not present in the column list is filled with a `null` value.

## Examples {#section_phk_nl3_ygb .section}

```
INSERT INTO orders SELECT * FROM new_orders; --- Inserts the SELECT results into the table "orders".
INSERT INTO cities VALUES (1, 'San Francisco'); --- Inserts a data row.
INSERT INTO cities VALUES (2, 'San Jose'), (3, 'Oakland'); --- Inserts multiple rows.
INSERT INTO nation (nationkey, name, regionkey, comment) VALUES (26, 'POLAND', 3, 'no comment'); --- Inserts a single row.
INSERT INTO nation (nationkey, name, regionkey) VALUES (26, 'POLAND', 3); --- Inserts a single row (which only includes some columns).
```

