# VALUES {#concept_v2m_d3m_zgb .concept}

## Overview {#section_p5k_ll3_ygb .section}

```
VALUES row [, ...]

`row` is a single expression or an expression list in the following format:

( column_expression [, ...] )
```

## Description {#section_fym_ml3_ygb .section}

Defines a literal inline table.

-   `VALUE` can be used anywhere a query statement can be used, such as next to the `FROM` clause of a `SELECT` statement, in an `INSERT` statement, and at the top layer.
-   By default, `VALUE` creates an anonymous table without column names. The table and columns can be named by using an `AS` clause.

## Examples {#section_phk_nl3_ygb .section}

```
--- Return a table with one column and three rows
VALUES 1, 2, 3

--- Return a table with two columns and three rows
VALUES
    (1, 'a'),
    (2, 'b'),
    (3, 'c')
	
--- Use VALUES in a query statement
SELECT * FROM (
    VALUES
        (1, 'a'),
        (2, 'b'),
        (3, 'c')
) AS t (id, name)

--- Create a table
CREATE TABLE example AS
SELECT * FROM (
    VALUES
        (1, 'a'),
        (2, 'b'),
        (3, 'c')
) AS t (id, name)
```

