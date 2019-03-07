# WITH 子句 {#concept_o2t_fhk_zgb .concept}

## 基本功能 {#section_ext_4hk_zgb .section}

`WITH`子句可以定义一组命名关系，这些关系可以在查询中被使用，从而扁平化嵌套查询或简化子查询。如下两个查询语句是等价的：

```
--- 不使用WITH子句
SELECT a, b
FROM (
  SELECT a, MAX(b) AS b FROM t GROUP BY a
) AS x;

--- 使用WITH子句，查询语句看起来更加明了
WITH x AS (SELECT a, MAX(b) AS b FROM t GROUP BY a)
SELECT a, b FROM x;

```

## 定义多个子查询 {#section_fxt_4hk_zgb .section}

`WITH`子句中可以定义多个子查询：

```
WITH
  t1 AS (SELECT a, MAX(b) AS b FROM x GROUP BY a),
  t2 AS (SELECT a, AVG(d) AS d FROM y GROUP BY a)
SELECT t1.*, t2.*
FROM t1
JOIN t2 ON t1.a = t2.a;

```

## 组成链式结构 {#section_gxt_4hk_zgb .section}

`WITH`子句中的关系还可以组成链式结构：

```
WITH
  x AS (SELECT a FROM t),
  y AS (SELECT a AS b FROM x),
  z AS (SELECT b AS c FROM y)
SELECT c FROM z;
```

