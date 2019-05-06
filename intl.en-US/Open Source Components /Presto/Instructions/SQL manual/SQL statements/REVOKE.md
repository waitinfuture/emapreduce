# REVOKE {#concept_wkl_kdk_zgb .concept}

## Overview {#section_p5k_ll3_ygb .section}

```
REVOKE [ GRANT OPTION FOR ]
( privilege [, ...] | ALL PRIVILEGES )
ON [ TABLE ] table_name FROM ( grantee | PUBLIC )
```

## Description {#section_fym_ml3_ygb .section}

Revokes the specified permissions from the specified user.

-   `ALL PRIVILEGE` revokes the `SELECT`, `INSERT`, and `DELETE` permissions.
-   Specifying `PUBLIC` revokes permissions from the `PUBLIC` role. The permissions that are assigned by other roles are retained.
-   The `GRANT OPTION FOR` clause revokes the permissions that are assigned by using `GRANT`.
-   `grantee` may be a single user or a role.

## Examples {#section_phk_nl3_ygb .section}

```
--- Revoke the INSERT and SELECT permissions on the table "orders" from user Alice
REVOKE INSERT, SELECT ON orders FROM alice;

--- Revoke the SELECT permission on the table "nation" from all users
--- In addition, revoke the SELECT permission on the table that all users assign to other users
REVOKE GRANT OPTION FOR SELECT ON nation FROM PUBLIC;

--- Revoke all the permissions on the table "test" from user Alice
REVOKE ALL PRIVILEGES ON test FROM alice;
```

## Limits {#section_g3b_sdk_zgb .section}

Some connectors do not support `REVOKE`.

