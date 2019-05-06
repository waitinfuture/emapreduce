# Set operations {#concept_rtc_jhl_zgb .concept}

## Overview {#section_ypz_cjl_zgb .section}

Presto supports three set operations: `UNION`, `INTERSECT`, and `EXCEPT`. These clauses merge the results of more than one query statement into a single result set. The usage is as follows:

```
query UNION [ALL | DISTINCT] query
query INTERSECT [DISTINCT] query
query EXCEPT [DISTINCT] query

```

The arguments `ALL` and `DISTINCT` control which rows are included in the final result set. The default value is `DISTINCT`.

-   `ALL`: Duplicate rows may be returned.
-   `DISTINCT`: The result set is deduplicated.

`ALL` is not supported by `INTERSECT` and `EXCEPT`.

The three preceding set operations are processed from left to right. `INTERSECT` has the highest priority. That means `A UNION B INTERSECT C EXCEPT D` is the same as `A UNION (B INTERSECT C) EXCEPT D`.

## UNION {#section_cqz_cjl_zgb .section}

`UNION` combines two query result sets and uses the `ALL` and `DISTINCT` arguments to control whether to remove duplicates.

Example 1:

```
SELECT 13
UNION
SELECT 42;

 _col0
-------
    13
    42
(2 rows)

```

Example 2:

```
SELECT 13
UNION
SELECT * FROM (VALUES 42, 13);

 _col0
-------
    13
    42
(2 rows)

```

Example 3:

```
SELECT 13
UNION ALL
SELECT * FROM (VALUES 42, 13);

 _col0
-------
    13
    42
    13
(3 rows)

```

## INTERSECT {#section_fqz_cjl_zgb .section}

`INTERSECT` returns only the rows that are in both query result sets.

Examples:

```
SELECT * FROM (VALUES 13, 42)
INTERSECT
SELECT 13;

 _col0
-------
   13
(1 rows)

```

## EXCEPT {#section_gqz_cjl_zgb .section}

`EXCEPT` returns the complement of the result sets of two queries.

```
SELECT * FROM (VALUES 13, 42)
EXCEPT
SELECT 13;

 _col0
-------
   42
(1 rows)
```

