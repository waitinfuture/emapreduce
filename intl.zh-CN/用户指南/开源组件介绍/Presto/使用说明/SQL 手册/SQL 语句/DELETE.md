# DELETE {#concept_kkr_ps3_ygb .concept}

## 概要 {#section_p5k_ll3_ygb .section}

```
DELETE FROM table_name [ WHERE condition ]
```

## 描述 {#section_fym_ml3_ygb .section}

删除表中与 WHERE 子句匹配的行，如果没有指定 WHERE 子句，将删除所有行。

## 示例 {#section_phk_nl3_ygb .section}

```
--- 删除匹配行
DELETE FROM lineitem WHERE shipmode = 'AIR';

--- 删除匹配行
DELETE FROM lineitem
WHERE orderkey IN (SELECT orderkey FROM orders WHERE priority = 'LOW');

--- 清空表
DELETE FROM orders;
```

## 局限 {#section_lwk_ts3_ygb .section}

部分连接器可能不支持`DELETE`。

