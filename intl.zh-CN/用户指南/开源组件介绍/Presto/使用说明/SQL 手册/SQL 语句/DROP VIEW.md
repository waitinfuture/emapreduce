# DROP VIEW {#concept_kfy_kyj_zgb .concept}

## 概要 {#section_p5k_ll3_ygb .section}

```
DROP VIEW  [ IF EXISTS ] view_name
```

## 描述 {#section_fym_ml3_ygb .section}

删除数据表。使用 IF EXISTS 来避免删除不存在的表时报错。

## 示例 {#section_phk_nl3_ygb .section}

```
DROP VIEW orders_by_date;
DROP VIEW IF EXISTS orders_by_date;
```

