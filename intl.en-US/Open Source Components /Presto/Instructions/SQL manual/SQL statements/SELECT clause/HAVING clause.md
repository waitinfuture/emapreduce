# HAVING clause {#concept_egg_xjk_zgb .concept}

The `HAVING` clause is used in conjunction with aggregate functions and the `GROUP BY` clause to control which groups are selected during a query. The `HAVING` clause is executed after grouping and aggregation to eliminate groups that do not satisfy the given conditions.

The following example selects users with an account balance greater than 5,700,000:

```
SELECT count(*), mktsegment, nationkey,
       CAST(sum(acctbal) AS bigint) AS totalbal
FROM customer
GROUP BY mktsegment, nationkey
HAVING sum(acctbal) > 5700000
ORDER BY totalbal DESC;
```

The output is as follows:

```
_col0 | mktsegment | nationkey | totalbal
-------+------------+-----------+----------
  1272 | AUTOMOBILE |        19 |  5856939
  1253 | FURNITURE  |        14 |  5794887
  1248 | FURNITURE  |         9 |  5784628 
  1243 | FURNITURE  |        12 |  5757371
  1231 | HOUSEHOLD  |         3 |  5753216
  1251 | MACHINERY  |         2 |  5719140 
  1247 | FURNITURE  |         8 |  5701952
(7 rows)
```

