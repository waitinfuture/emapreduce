# SHOW GRANTS {#concept_vk4_xcm_zgb .concept}

## Overview {#section_p5k_ll3_ygb .section}

```
SHOW GRANTS [ ON [ TABLE ] table_name ]
```

## Description {#section_fym_ml3_ygb .section}

Lists the permissions of the current user on the specified table in the current catalog.

## Examples {#section_phk_nl3_ygb .section}

```
--- List the permissions of the current user on the table "orders"
SHOW GRANTS ON TABLE orders;

--- List the permissions of the current user on the current catalog
SHOW GRANTS;
```

## Limits {#section_pz5_zcm_zgb .section}

Some connectors do not support `SHOW GRANTS`.

