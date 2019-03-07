# SHOW PARTITIONS {#concept_p2t_cdm_zgb .concept}

## 概要 {#section_p5k_ll3_ygb .section}

```
SHOW PARTITIONS FROM table [ WHERE ... ] [ ORDER BY ... ] [ LIMIT ... ]
```

## 描述 {#section_fym_ml3_ygb .section}

获取一张表的分区列表。可以使用`WHERE`子句进行条件过滤，使用`ORDER BY`进行排序，使用LIMIT来限制结果集大小。这些子句的使用方式和`SELECT`无异。

## 示例 {#section_phk_nl3_ygb .section}

```
--- 获取表orders的所有分区列表
SHOW PARTITIONS FROM orders;

--- 获取表orders从2013年至今的所有分区列表，并按照年份排序
SHOW PARTITIONS FROM orders WHERE ds >= '2013-01-01' ORDER BY ds DESC;

--- 获取表order最近的几个分区
SHOW PARTITIONS FROM orders ORDER BY ds DESC LIMIT 10;
```

