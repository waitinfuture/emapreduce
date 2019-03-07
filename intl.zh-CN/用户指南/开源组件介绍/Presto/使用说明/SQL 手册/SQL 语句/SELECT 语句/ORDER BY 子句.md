# ORDER BY 子句 {#concept_erj_gml_zgb .concept}

在查询中可以使用`ORDER BY`子句对查询结果进行排序。语法如下：

```
ORDER BY expression [ ASC | DESC ] [ NULLS { FIRST | LAST } ] [, ...]

```

其中：

-   `expression`由列名或列位置序号（从 1 开始）组成；
-   `ORDER BY`在`GROUP BY`和`HAVING`之后执行；
-   `NULLS { FIRST | LAST }`可以控制`NULL`值的排序方式（与`ASC`/`DESC`无关），默认为`LAST`。

