# ORDER BY clause {#concept_erj_gml_zgb .concept}

The `ORDER BY` clause sorts query results. Syntax:

```
ORDER BY expression [ ASC | DESC ] [ NULLS { FIRST | LAST } ] [, ...]

```

Specifically:

-   Each `expression` consists of column names or column SNs \(starting from 1\).
-   The `ORDER BY` clause is executed after `GROUP BY` and `HAVING`.
-   `NULLS { FIRST | LAST }` controls the sorting method of the `NULL` values \(regardless of `ASC` or `DESC`\). The default NULL order is `LAST`.

