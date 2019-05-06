# GROUP BY clause {#concept_ag1_b3k_zgb .concept}

## Basic functions {#section_tmh_b3k_zgb .section}

The `GROUP BY` clause divides the output of a `SELECT` statement into groups. The `GROUP BY` clause may contain any expression that consists of column names or column SNs \(starting from 1\).

The following queries are equivalent \(the SN of the `nationkey` column is 2\).

```
--- Use the column SN
SELECT count(*), nationkey FROM customer GROUP BY 2;

--- Use the column name
SELECT count(*), nationkey FROM customer GROUP BY nationkey;

```

The `GROUP BY` clause can group columns that are not specified in the output list. Example:

```
--- The mktsegment column is not specified in the SELECT list.
--- The result set does not contain the content of the mktsegment column.
SELECT count(*) FROM customer GROUP BY mktsegment;

 _col0
-------
 29968
 30142
 30189
 29949
 29752
(5 rows)

```

When a `GROUP BY` clause is used in a `SELECT` statement, all the output expressions must be either aggregate functions or columns present in the `GROUP BY` clause.

## Complex grouping operations {#section_vmh_b3k_zgb .section}

Presto supports the following three complex aggregation syntaxes, allowing you to perform analysis that requires aggregation on multiple sets of columns in a single query:

-   GROUPING SETS
-   CUBE
-   ROLLUP

The complex grouping operations of Presto only support column names and column SNs, but do not support expressions.

## GROUPING SETS {#section_xmh_b3k_zgb .section}

`GROUPING SETS` performs grouping and aggregation of multiple columns in a single query statement. The columns not present in the group list are padded with `NULL`.

The "shipping" table contains five columns as follows:

```
SELECT * FROM shipping;

 origin_state | origin_zip | destination_state | destination_zip | package_weight
--------------+------------+-------------------+-----------------+----------------
 California   |      94131 | New Jersey        |            8648 |             13
 California   |      94131 | New Jersey        |            8540 |             42
 New Jersey   |       7081 | Connecticut       |            6708 |            225
 California   |      90210 | Connecticut       |            6927 |           1337
 California   |      94131 | Colorado          |           80302 |              5
 New York     |      10002 | New Jersey        |            8540 |              3
(6 rows)

```

To retrieve the following grouping results by using a single query statement, do as follows:

-   Group by origin\_state, and get the total package\_weight.
-   Group by origin\_state and origin\_zip, and get the total package\_weight.
-   Group by destination\_state, and get the total package\_weight.

`GROUPING SETS` retrieves the result set of the preceding three groups with a single query statement, which is shown as follows:

```
SELECT origin_state, origin_zip, destination_state, sum(package_weight)
FROM shipping
GROUP BY GROUPING SETS (
    (origin_state),
    (origin_state, origin_zip),
    (destination_state));
	
 origin_state | origin_zip | destination_state | _col0
--------------+------------+-------------------+-------
 New Jersey   | NULL       | NULL              |   225 
 California   | NULL       | NULL              |  1397
 New York     | NULL       | NULL              |     3
 California   |      90210 | NULL              |  1337
 California   |      94131 | NULL              |    60
 New Jersey   |       7081 | NULL              |   225 
 New York     |      10002 | NULL              |     3 
 NULL         | NULL       | Colorado          |     5
 NULL         | NULL       | New Jersey        |    58
 NULL         | NULL       | Connecticut       |  1562
(10 rows)

```

The preceding query may be considered logically equivalent to a `UNION ALL` of multiple `GROUP BY` queries:

```
SELECT origin_state, NULL, NULL, sum(package_weight)
FROM shipping GROUP BY origin_state

UNION ALL

SELECT origin_state, origin_zip, NULL, sum(package_weight)
FROM shipping GROUP BY origin_state, origin_zip

UNION ALL

SELECT NULL, NULL, destination_state, sum(package_weight)
FROM shipping GROUP BY destination_state;

```

However, the `GROUPING SETS` query performs better because it reads the underlying table data only once, whereas the `UNION ALL` query reads the underlying table data three times. This is why queries with `UNION ALL` may return inconsistent results when the underlying table data is not deterministic during the query period.

## CUBE {#section_bnh_b3k_zgb .section}

The `CUBE` operator generates all possible grouping sets for a given set of columns. See the following code:

```
SELECT origin_state, destination_state, sum(package_weight)
FROM shipping
GROUP BY CUBE (origin_state, destination_state);

 origin_state | destination_state | _col0
--------------+-------------------+-------
 California   | New Jersey        |    55
 California   | Colorado          |     5
 New York     | New Jersey        |     3 
 New Jersey   | Connecticut       |   225 
 California   | Connecticut       |  1337
 California   | NULL              |  1397
 New York     | NULL              |     3 
 New Jersey   | NULL              |   225
 NULL         | New Jersey        |    58
 NULL         | Connecticut       |  1562
 NULL         | Colorado          |     5
 NULL         | NULL              |  1625
(12 rows)

```

This query is equivalent to:

```
SELECT origin_state, destination_state, sum(package_weight)
FROM shipping
GROUP BY GROUPING SETS (
    (origin_state, destination_state),
    (origin_state),
    (destination_state),
    ());

```

## ROLLUP {#section_dnh_b3k_zgb .section}

The `ROLLUP` operator generates all possible subtotals for a given set of columns. See the following code:

```
SELECT origin_state, origin_zip, sum(package_weight)
FROM shipping
GROUP BY ROLLUP (origin_state, origin_zip);

 origin_state | origin_zip | _col2
--------------+------------+-------
 California   |      94131 |    60
 California   |      90210 |  1337
 New Jersey   |       7081 |   225
 New York     |      10002 |     3 
 California   | NULL       |  1397
 New York     | NULL       |     3
 New Jersey   | NULL       |   225
 NULL         | NULL       |  1625
(8 rows)

```

This query is equivalent to:

```
SELECT origin_state, origin_zip, sum(package_weight)
FROM shipping
GROUP BY GROUPING SETS ((origin_state, origin_zip), (origin_state), ());
```

## Combine multiple grouping expressions {#section_enh_b3k_zgb .section}

The following three statements are equivalent:

```
SELECT origin_state, destination_state, origin_zip, sum(package_weight)
FROM shipping
GROUP BY
    GROUPING SETS ((origin_state, destination_state)),
    ROLLUP (origin_zip);

```

```
SELECT origin_state, destination_state, origin_zip, sum(package_weight)
FROM shipping
GROUP BY
    GROUPING SETS ((origin_state, destination_state)),
    GROUPING SETS ((origin_zip), ());

```

```
SELECT origin_state, destination_state, origin_zip, sum(package_weight)
FROM shipping
GROUP BY GROUPING SETS (
    (origin_state, destination_state, origin_zip),
    (origin_state, destination_state));

```

The output is follows:

```
 origin_state | destination_state | origin_zip | _col3
--------------+-------------------+------------+-------
 New York     | New Jersey        |      10002 |     3
 California   | New Jersey        |      94131 |    55
 New Jersey   | Connecticut       |       7081 |   225
 California   | Connecticut       |      90210 |  1337
 California   | Colorado          |      94131 |     5
 New York     | New Jersey        | NULL       |     3 
 New Jersey   | Connecticut       | NULL       |   225
 California   | Colorado          | NULL       |     5
 California   | Connecticut       | NULL       |  1337
 California   | New Jersey        | NULL       |    55
(10 rows)

```

In a `GROUP BY` clause, the `ALL` and `DISTINCT` quantifiers determine whether duplicate grouping sets each produce distinct output rows. See the following code:

```
SELECT origin_state, destination_state, origin_zip, sum(package_weight)
FROM shipping
GROUP BY ALL
    CUBE (origin_state, destination_state),
    ROLLUP (origin_state, origin_zip);

```

The preceding code is equivalent to:

```
SELECT origin_state, destination_state, origin_zip, sum(package_weight)
FROM shipping
GROUP BY GROUPING SETS (
    (origin_state, destination_state, origin_zip),
    (origin_state, origin_zip),
    (origin_state, destination_state, origin_zip),
    (origin_state, origin_zip),
    (origin_state, destination_state),
    (origin_state),
    (origin_state, destination_state),
    (origin_state),
    (origin_state, destination_state),
    (origin_state),
    (destination_state),
    ());

```

Multiple duplicate grouping sets are available. If the query uses the `DISTINCT` quantifier, only unique grouping sets are generated.

```
SELECT origin_state, destination_state, origin_zip, sum(package_weight)
FROM shipping
GROUP BY DISTINCT
    CUBE (origin_state, destination_state),
    ROLLUP (origin_state, origin_zip);

```

The preceding code is equivalent to:

```
SELECT origin_state, destination_state, origin_zip, sum(package_weight)
FROM shipping
GROUP BY GROUPING SETS (
    (origin_state, destination_state, origin_zip),
    (origin_state, origin_zip),
    (origin_state, destination_state),
    (origin_state),
    (destination_state),
    ());

```

**Note:** The default qualifier for `GROUP BY` is ALL.

## GROUPING function {#section_knh_b3k_zgb .section}

Presto provides the `GROUPING` function that returns a bit set, and each bit indicates whether the corresponding column is present in a grouping condition. Syntax:

```
grouping(col1, ..., colN) -> bigint

```

`GROUPING` is typically used in conjunction with `GROUPING SETS`, `ROLLUP`, `CUBE`, or `GROUP BY`. The columns in `GROUPING` must exactly map the columns that are referenced in the corresponding `GROUPING SETS`, `ROLLUP`, `CUBE`, or `GROUP BY` clause.

```
SELECT origin_state, origin_zip, destination_state, sum(package_weight),
       grouping(origin_state, origin_zip, destination_state)
FROM shipping
GROUP BY GROUPING SETS (
        (origin_state),
        (origin_state, origin_zip),
        (destination_state));
		
origin_state | origin_zip | destination_state | _col3 | _col4
--------------+------------+-------------------+-------+-------
California   | NULL       | NULL              |  1397 |     3   --- 011
New Jersey   | NULL       | NULL              |   225 |     3   --- 011
New York     | NULL       | NULL              |     3 |     3   --- 011 
California   |      94131 | NULL              |    60 |     1   --- 001
New Jersey   |       7081 | NULL              |   225 |     1   --- 001
California   |      90210 | NULL              |  1337 |     1   --- 001
New York     |      10002 | NULL              |     3 |     1   --- 001
NULL         | NULL       | New Jersey        |    58 |     6   --- 100
NULL         | NULL       | Connecticut       |  1562 |     6   --- 100 
NULL         | NULL       | Colorado          |     5 |     6   --- 100
(10 rows)

```

As shown in the preceding table, bits are assigned to the parameter columns with the rightmost column being the least significant bit. For a given `GROUPING`, a bit is set to `0` if the corresponding column is included in the grouping, and set to `1` if the corresponding column is not included in the grouping.

