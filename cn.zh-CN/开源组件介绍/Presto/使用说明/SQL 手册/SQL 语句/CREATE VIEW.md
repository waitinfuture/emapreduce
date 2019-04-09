# CREATE VIEW {#concept_pdl_bp3_ygb .concept}

## 概要 {#section_p5k_ll3_ygb .section}

```
CREATE [ OR REPLACE ] VIEW view_name AS query
```

## 描述 {#section_fym_ml3_ygb .section}

创建视图。视图是一个逻辑表，不包含实际数据，可以在查询中被引用。每次查询引用视图时，定义视图的查询语句都会被执行一次。

创建视图时使用`OR REPLACE`可以避免在视图存在时抛出异常。

## 示例 {#section_phk_nl3_ygb .section}

```
--- 创建一个简单的视图
CREATE VIEW test AS
SELECT orderkey, orderstatus, totalprice / 2 AS half
FROM orders
--- 创建视图，使用聚合函数
CREATE VIEW orders_by_date AS
SELECT orderdate, sum(totalprice) AS price
FROM orders
GROUP BY orderdate
--- 创建视图，如果视图已经存在，就替换它
CREATE OR REPLACE VIEW test AS
SELECT orderkey, orderstatus, totalprice / 4 AS quarter
FROM orders
```

