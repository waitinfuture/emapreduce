# VALUES {#concept_v2m_d3m_zgb .concept}

## 概要 {#section_p5k_ll3_ygb .section}

```
VALUES row [, ...]

其中， `row`是一个表达式，或者如下形式的表达式列表：

( column_expression [, ...] )
```

## 描述 {#section_fym_ml3_ygb .section}

定义一张字面内联表。

-   任何可以使用查询语句的地方都可以使用`VALUE`， 如放在`SELECT`语句的`FROM`后面，放在`INSERT`里，甚至放在最顶层；
-   `VALUE`默认创建一张匿名表，并且没有列名。表名和列名可以通过`AS`进行命名。

## 示例 {#section_phk_nl3_ygb .section}

```
--- 返回一个表对象，包含1列，3行数据
VALUES 1, 2, 3

--- 返回一个表对象，包含2列，3行数据
VALUES
    (1, 'a'),
    (2, 'b'),
    (3, 'c')
	
--- 在查询语句中使用
SELECT * FROM (
    VALUES
        (1, 'a'),
        (2, 'b'),
        (3, 'c')
) AS t (id, name)

--- 创建一个表
CREATE TABLE example AS
SELECT * FROM (
    VALUES
        (1, 'a'),
        (2, 'b'),
        (3, 'c')
) AS t (id, name)
```

