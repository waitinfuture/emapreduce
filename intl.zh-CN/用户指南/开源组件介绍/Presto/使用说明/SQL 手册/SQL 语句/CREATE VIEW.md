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
--- 从表orders中选择两个列来创建新的表
CREATE TABLE orders_column_aliased (order_date, total_price)
AS
SELECT orderdate, totalprice
FROM orders

--- 创建新表，使用聚合函数
CREATE TABLE orders_by_date
COMMENT 'Summary of orders by date'
WITH (format = 'ORC')
AS
SELECT orderdate, sum(totalprice) AS price
FROM orders
GROUP BY orderdate

--- 创建新表，使用`IF NOT EXISTS`子句
CREATE TABLE IF NOT EXISTS orders_by_date AS
SELECT orderdate, sum(totalprice) AS price
FROM orders
GROUP BY orderdate

--- 创建新表，schema和表nation一样，但是没有数据
CREATE TABLE empty_nation AS
SELECT *
FROM nation
WITH NO DATA
)
```

