# LIMIT 子句 {#concept_gvk_5ml_zgb .concept}

`LIMIT`子句用来控制结果集的规模（即行数）。使用`LIMIT ALL`等价于不使用`LIMIT`子句。

示例：

```
--- 本例中，因为没有使用ORDER BY子句，返回结果是随机的
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

