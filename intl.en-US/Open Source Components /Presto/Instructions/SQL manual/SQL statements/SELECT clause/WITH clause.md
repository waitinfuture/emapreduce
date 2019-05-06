# WITH clause {#concept_o2t_fhk_zgb .concept}

## Basic functions {#section_ext_4hk_zgb .section}

The `WITH` clause defines named relations for use within a query, which flattens nested queries or simplifies subqueries. For example, the following two queries are equivalent:

```
--- The WITH clause is not used.
SELECT a, b
FROM (
  SELECT a, MAX(b) AS b FROM t GROUP BY a
) AS x;

--- The WITH clause is used, and the query statement is clearer.
WITH x AS (SELECT a, MAX(b) AS b FROM t GROUP BY a)
SELECT a, b FROM x;

```

## Define multiple subqueries {#section_fxt_4hk_zgb .section}

Use the `WITH` clause to define multiple subqueries:

```
WITH
  t1 AS (SELECT a, MAX(b) AS b FROM x GROUP BY a),
  t2 AS (SELECT a, AVG(d) AS d FROM y GROUP BY a)
SELECT t1.*, t2. *
FROM t1
JOIN t2 ON t1.a = t2.a;

```

## Form a chain structure {#section_gxt_4hk_zgb .section}

Additionally, the relations within a `WITH` clause can form a chain structure.

```
WITH
  x AS (SELECT a FROM t),
  y AS (SELECT a AS b FROM x),
  z AS (SELECT b AS c FROM y)
SELECT c FROM z;
```

