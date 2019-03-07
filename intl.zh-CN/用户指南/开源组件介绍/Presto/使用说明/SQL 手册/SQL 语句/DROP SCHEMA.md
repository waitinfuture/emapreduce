# DROP SCHEMA {#concept_pdj_vf1_1hb .concept}

## 概要 {#section_p5k_ll3_ygb .section}

```
DROP SCHEMA [ IF EXISTS ] schema_name
```

## 描述 {#section_fym_ml3_ygb .section}

删除 Schema。

-   Schema 必须为空；
-   使用`IF EXISTS`来避免删除不存在的 Schema 时报错。

## 示例 {#section_phk_nl3_ygb .section}

```
DROP SCHEMA web;
DROP TABLE IF EXISTS sales;
```

