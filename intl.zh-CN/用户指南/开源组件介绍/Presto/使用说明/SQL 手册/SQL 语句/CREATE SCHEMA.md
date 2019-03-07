# CREATE SCHEMA {#concept_d21_pn3_ygb .concept}

## 概要 {#section_p5k_ll3_ygb .section}

```
CREATE SCHEMA [ IF NOT EXISTS ] schema_name
[ WITH ( property_name = expression [, ...] ) ]
```

## 描述 {#section_fym_ml3_ygb .section}

创建新的SCHEMA。Schema 是管理数据库表、视图和其他数据库对象的容器。

-   如果 SCHEMA 已经存在，使用`IF NOT EXISTS`子句可以避免抛错；
-   使用`WITH`子句可以在创建时为 SCHEMA 设置属性。可以通过下列查询语句获取所有支持的属性列表：

`SELECT * FROM system.metadata.schema_properties;`

## 示例 {#section_phk_nl3_ygb .section}

```
CREATE SCHEMA web;
CREATE SCHEMA hive.sales;
CREATE SCHEMA IF NOT EXISTS traffic;
```

