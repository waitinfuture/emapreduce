# Joins {#concept_t15_cpl_zgb .concept}

Joins merges data from multiple relations. `CROSS JOIN` returns the [Cartesian product](https://en.wikipedia.org/wiki/Cartesian_product) of two relations \(all combinations\). `CROSS JOIN` can be specified by using either of the following two methods:

-   Use the explicit `CROSS JOIN` syntax.
-   Specify multiple relations in the `FROM` clause.

The following two SQL statements are equivalent:

```
--- Use the explicit `CROSS JOIN` syntax
SELECT *
FROM nation
CROSS JOIN region;

--- Specify multiple relations in the `FROM` clause
SELECT *
FROM nation, region;

```

Examples:

The "nation" table contains 25 rows, and the "region" table contains 5 rows, so a CROSS JOIN between the two tables produces 125 rows.

```
SELECT n.name AS nation, r.name AS region
FROM nation AS n
CROSS JOIN region AS r
ORDER BY 1, 2;

     nation     |   region
----------------+-------------
 ALGERIA        | AFRICA
 ALGERIA        | AMERICA
 ALGERIA        | ASIA 
 ALGERIA        | EUROPE
 ALGERIA        | MIDDLE EAST
 ARGENTINA      | AFRICA
 ARGENTINA      | AMERICA 
...
(125 rows)

```

When the two tables in a join have columns with the same name, the column references must be qualified by using the table name \(or alias\).

Examples:

```
--- Correct
SELECT nation.name, region.name
FROM nation
CROSS JOIN region;

--- Correct
SELECT n.name, r.name
FROM nation AS n
CROSS JOIN region AS r;

--- Correct
SELECT n.name, r.name
FROM nation n
CROSS JOIN region r;

--- Incorrect. "Column 'name' is ambiguous" is thrown.
SELECT name
FROM nation
CROSS JOIN region;
```

