# DESCRIBE OUTPUT {#concept_xnf_bw3_ygb .concept}

## 概要 {#section_p5k_ll3_ygb .section}

```
DESCRIBE OUTPUT statement_name
```

## 描述 {#section_fym_ml3_ygb .section}

列出输出结果的所有列信息，包括列名（或别名）、Catalog、Schema、表名、类型、类型大小（单位字节）和一个标识位，说明该列是否为别名。

## 示例 {#section_phk_nl3_ygb .section}

示例1

准备一个预编译查询：

```
PREPARE my_select1 FROM
SELECT * FROM nation;

```

执行`DESCRIBE OUTPUT`，输出：

```
DESCRIBE OUTPUT my_select1;

 Column Name | Catalog | Schema | Table  |  Type   | Type Size | Aliased
-------------+---------+--------+--------+---------+-----------+---------
 nationkey   | tpch    | sf1    | nation | bigint  |         8 | false
 name        | tpch    | sf1    | nation | varchar |         0 | false
 regionkey   | tpch    | sf1    | nation | bigint  |         8 | false
 comment     | tpch    | sf1    | nation | varchar |         0 | false
(4 rows)

```

示例2

```
PREPARE my_select2 FROM
SELECT count(*) as my_count, 1+2 FROM nation

```

执行`DESCRIBE OUTPUT`，输出：

```
DESCRIBE OUTPUT my_select2;

 Column Name | Catalog | Schema | Table |  Type  | Type Size | Aliased
-------------+---------+--------+-------+--------+-----------+---------
 my_count    |         |        |       | bigint |         8 | true
 _col1       |         |        |       | bigint |         8 | false
(2 rows)

```

示例3

```
PREPARE my_create FROM
CREATE TABLE foo AS SELECT * FROM nation;

```

执行`DESCRIBE OUTPUT`，输出：

```
DESCRIBE OUTPUT my_create;

 Column Name | Catalog | Schema | Table |  Type  | Type Size | Aliased
-------------+---------+--------+-------+--------+-----------+---------
 rows        |         |        |       | bigint |         8 | false
(1 row)
```

