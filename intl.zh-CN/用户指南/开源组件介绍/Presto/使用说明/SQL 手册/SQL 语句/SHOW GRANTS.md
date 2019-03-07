# SHOW GRANTS {#concept_vk4_xcm_zgb .concept}

## 概要 {#section_p5k_ll3_ygb .section}

```
SHOW GRANTS [ ON [ TABLE ] table_name ]
```

## 描述 {#section_fym_ml3_ygb .section}

显示权限列表。

## 示例 {#section_phk_nl3_ygb .section}

```
--- 获取当前用户在表orders上的权限
SHOW GRANTS ON TABLE orders;

--- 获取当前用户在当前catalog中的权限
SHOW GRANTS;
```

## 局限 {#section_pz5_zcm_zgb .section}

有些连接器不支持`SHOW GRANTS`操作。

