# PREPARE {#concept_rrk_qck_zgb .concept}

## 概要 {#section_p5k_ll3_ygb .section}

```
PREPARE statement_name FROM statement
```

## 描述 {#section_fym_ml3_ygb .section}

创建一个预处理语句，以备后续使用。预处理语句就是一组保存在会话中的命名查询语句集合，可以在这些语句中定义参数，在实际执行时填充实际的值。定义的参数使用`?`占位。

## 示例 {#section_phk_nl3_ygb .section}

```
--- 不带参数的预处理查询语句
PREPARE my_select1 FROM
SELECT * FROM nation;

--- 带参数的预处理查询语句
PREPARE my_select2 FROM
SELECT name FROM nation WHERE regionkey = ? AND nationkey < ?;

--- 不带参数的预处理插入语句
PREPARE my_insert FROM
INSERT INTO cities VALUES (1, 'San Francisco');
```

