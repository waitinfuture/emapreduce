# ALTER TABLE {#concept_khm_sl3_ygb .concept}

## 概要 {#section_p5k_ll3_ygb .section}

```
ALTER TABLE name RENAME TO new_name
ALTER TABLE name ADD COLUMN column_name data_type
ALTER TABLE name DROP COLUMN column_name
ALTER TABLE name RENAME COLUMN column_name TO new_column_name
```

## 描述 {#section_fym_ml3_ygb .section}

更新表格定义。

## 示例 {#section_phk_nl3_ygb .section}

```
ALTER TABLE users RENAME TO people; --- 重命名
ALTER TABLE users ADD COLUMN zip varchar; --- 添加列
ALTER TABLE users DROP COLUMN zip; --- 删除列
ALTER TABLE users RENAME COLUMN id TO user_id; --- 重命名列
```

