# SELECT {#concept_smb_4gk_zgb .concept}

## Overview {#section_p5k_ll3_ygb .section}

```
[ WITH with_query [, ...] ]
SELECT [ ALL | DISTINCT ] select_expr [, ...]
[ FROM from_item [, ...] ]
[ WHERE condition ]
[ GROUP BY [ ALL | DISTINCT ] grouping_element [, ...] ]
[ HAVING condition]
[ { UNION | INTERSECT | EXCEPT } [ ALL | DISTINCT ] select ]
[ ORDER BY expression [ ASC | DESC ] [, ...] ]
[ LIMIT [ count | ALL ] ]
```

`from_item` is in either of the following two formats:

```
table_name [ [ AS ] alias [ ( column_alias [, ...] ) ] ]

```

```
from_item join_type from_item [ ON join_condition | USING ( join_column [, ...] ) ]

```

`join_type` is in one of the following formats:

-   \[ INNER \] JOIN
-   LEFT \[ OUTER \] JOIN
-   RIGHT \[ OUTER \] JOIN
-   FULL \[ OUTER \] JOIN
-   CROSS JOIN

`grouping_element` is in one of the following formats:

-   \(\)
-   expression
-   GROUPING SETS \( \( column \[, ...\] \) \[, ...\] \)
-   CUBE \( column \[, ...\] \)
-   ROLLUP \( column \[, ...\] \)

## Description {#section_fym_ml3_ygb .section}

Retrieves rows from zero or more tables to get data sets. For more information, see the following sections:

-   [WITH clause](reseller.en-US/Open Source Components /Presto/Instructions/SQL manual/SQL statements/SELECT clause/WITH clause.md#)
-   [GROUP BY clause](reseller.en-US/Open Source Components /Presto/Instructions/SQL manual/SQL statements/SELECT clause/GROUP BY clause.md#)
-   [HIVING clause](reseller.en-US/Open Source Components /Presto/Instructions/SQL manual/SQL statements/SELECT clause/HAVING clause.md#)
-   [UNION|INTERSECT|EXCEPT clause](reseller.en-US/Open Source Components /Presto/Instructions/SQL manual/SQL statements/SELECT clause/Set operations.md#)
-   [ORDER BY clause](reseller.en-US/Open Source Components /Presto/Instructions/SQL manual/SQL statements/SELECT clause/ORDER BY clause.md#)
-   [LIMIT clause](reseller.en-US/Open Source Components /Presto/Instructions/SQL manual/SQL statements/SELECT clause/LIMIT clause.md#)
-   [TABLESAMPLE](reseller.en-US/Open Source Components /Presto/Instructions/SQL manual/SQL statements/SELECT clause/TABLESAMPLE.md#)
-   [UNNEST](reseller.en-US/Open Source Components /Presto/Instructions/SQL manual/SQL statements/SELECT clause/UNNEST.md#)
-   [Joins](reseller.en-US/Open Source Components /Presto/Instructions/SQL manual/SQL statements/SELECT clause/Joins.md#)
-   [Subquery](reseller.en-US/Open Source Components /Presto/Instructions/SQL manual/SQL statements/SELECT clause/Subquery.md#)

