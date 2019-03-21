# SELECT {#concept_smb_4gk_zgb .concept}

## 概要 {#section_p5k_ll3_ygb .section}

```
[ WITH with_query [, ...] ]
SELECT [ ALL | DISTINCT ] select_expr [, ...]
[ FROM from_item [, ...] ]
[ WHERE condition ]
[ GROUP BY [ ALL | DISTINCT ] grouping_element [, ...] ]
[ HAVING condition]
[ { UNION | INTERSECT | EXCEPT } [ ALL | DISTINCT ] select ]
[ ORDER BY expression [ ASC | DESC ] [, ...] ]
[ LIMIT [ count | ALL ] ]
```

其中，`from_item`有如下两种形式：

```
table_name [ [ AS ] alias [ ( column_alias [, ...] ) ] ]

```

```
from_item join_type from_item [ ON join_condition | USING ( join_column [, ...] ) ]

```

这里的`join_type`可以是如下之一：

-   \[ INNER \] JOIN
-   LEFT \[ OUTER \] JOIN
-   RIGHT \[ OUTER \] JOIN
-   FULL \[ OUTER \] JOIN
-   CROSS JOIN

而`grouping_element`可以是如下之一：

-   \(\)
-   expression
-   GROUPING SETS \( \( column \[, ...\] \) \[, ...\] \)
-   CUBE \( column \[, ...\] \)
-   ROLLUP \( column \[, ...\] \)

## 描述 {#section_fym_ml3_ygb .section}

检索 0 到多张数据表，获取结果集。详细内容请参考如下小节：

-   [WITH 子句](intl.zh-CN/开源组件介绍/Presto/使用说明/SQL 手册/SQL 语句/SELECT 语句/WITH 子句.md#)
-   [GROUP BY 子句](intl.zh-CN/开源组件介绍/Presto/使用说明/SQL 手册/SQL 语句/SELECT 语句/GROUP BY 子句.md#)
-   [HIVING 子句](intl.zh-CN/开源组件介绍/Presto/使用说明/SQL 手册/SQL 语句/SELECT 语句/HAVING 子句.md#)
-   [UNION|INTERSECT|EXCEPT 子句](intl.zh-CN/开源组件介绍/Presto/使用说明/SQL 手册/SQL 语句/SELECT 语句/集合运算.md#)
-   [ORDER BY子句](intl.zh-CN/开源组件介绍/Presto/使用说明/SQL 手册/SQL 语句/SELECT 语句/ORDER BY 子句.md#)
-   [LIMIT 子句](intl.zh-CN/开源组件介绍/Presto/使用说明/SQL 手册/SQL 语句/SELECT 语句/LIMIT 子句.md#)
-   [TABLESAMPLE](intl.zh-CN/开源组件介绍/Presto/使用说明/SQL 手册/SQL 语句/SELECT 语句/TABLESAMPLE.md#)
-   [UNNEST](intl.zh-CN/开源组件介绍/Presto/使用说明/SQL 手册/SQL 语句/SELECT 语句/UNNEST.md#)
-   [Joins](intl.zh-CN/开源组件介绍/Presto/使用说明/SQL 手册/SQL 语句/SELECT 语句/Joins.md#)
-   [子查询](intl.zh-CN/开源组件介绍/Presto/使用说明/SQL 手册/SQL 语句/SELECT 语句/子查询.md#)

