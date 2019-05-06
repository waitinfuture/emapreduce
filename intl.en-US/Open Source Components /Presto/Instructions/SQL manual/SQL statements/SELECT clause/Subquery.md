# Subquery {#concept_fth_ppl_zgb .concept}

A subquery is an expression that consists a query. The subquery is correlated with external queries when it references columns beyond the subquery. Presto has limited support for correlated subqueries.

## EXISTS {#section_mgk_tpl_zgb .section}

The `EXISTS` predicate determines whether a subquery returns all rows. If the subquery returns rows, the WHERE expression is TRUE. If no rows are returned, the WHERE expression is FALSE.

Examples:

```
SELECT name
FROM nation
WHERE EXISTS (SELECT * FROM region WHERE region.regionkey = nation.regionkey);

```

## IN {#section_ngk_tpl_zgb .section}

The IN predicate determines whether any columns specified by `WHERE` are included in the result set produced by the subquery. If yes, results are returned. If not, no results are returned. A subquery returns only one column.

Examples:

```
SELECT name
FROM nation
WHERE regionkey IN (SELECT regionkey FROM region);

```

## Scalar subquery {#section_pgk_tpl_zgb .section}

A scalar subquery is a non-correlated subquery that returns zero or one row. The subquery cannot return more than one row. The returned value is `NULL` if the subquery returns no rows.

Examples:

```
SELECT name
FROM nation
WHERE regionkey = (SELECT max(regionkey) FROM region)
```

