# LIMIT clause {#concept_gvk_5ml_zgb .concept}

The `LIMIT` clause limits the number of rows in a result set. `LIMIT ALL` is the same as omitting the `LIMIT` clause.

Examples:

```
In this example, rows are returned arbitrarily because the query lacks an ORDER BY clause.
SELECT orderdate FROM orders LIMIT 5;

 orderdate
-------------
 1996-04-14
 1992-01-15
 1995-02-01
 1995-11-12
 1992-04-26
(5 rows)
```

