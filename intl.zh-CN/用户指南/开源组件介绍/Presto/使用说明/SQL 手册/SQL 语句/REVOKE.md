# REVOKE {#concept_wkl_kdk_zgb .concept}

## 概要 {#section_p5k_ll3_ygb .section}

```
REVOKE [ GRANT OPTION FOR ]
( privilege [, ...] | ALL PRIVILEGES )
ON [ TABLE ] table_name FROM ( grantee | PUBLIC )
```

## 描述 {#section_fym_ml3_ygb .section}

撤回指定用户的指定权限。

-   使用`ALL PRIVILEGE`可以撤销`SELECT`,`INSERT`和`DELETE`权限；
-   使用`PUBLIC`表示撤销对象为`PUBLIC`角色，用户通过其他角色获得的权限不会被收回；
-   使用`GRANT OPTION FOR`将进一步收回用户使用`GRANT`进行赋权的权限；
-   语句中`grantee`既可以是单个用户也可以是角色。

## 示例 {#section_phk_nl3_ygb .section}

```
--- 撤回用户alice对表orders进行INSET和SELECT的权限
REVOKE INSERT, SELECT ON orders FROM alice;

--- 撤回所有人对表nation进行SELECT的权限，
--- 同时，撤回所有人赋予其他人对该表进行SELECT操作权限的权限
REVOKE GRANT OPTION FOR SELECT ON nation FROM PUBLIC;

--- 撤回用户alice在表test上的所有权限
REVOKE ALL PRIVILEGES ON test FROM alice;
```

## 局限 {#section_g3b_sdk_zgb .section}

有些连接器不支持`REVOKE`语句。

