# ALTER TABLE {#concept_khm_sl3_ygb .concept}

## Overview {#section_p5k_ll3_ygb .section}

```
ALTER TABLE name RENAME TO new_name
ALTER TABLE name ADD COLUMN column_name data_type
ALTER TABLE name DROP COLUMN column_name
ALTER TABLE name RENAME COLUMN column_name TO new_column_name
```

## Description {#section_fym_ml3_ygb .section}

Changes the definition of an existing table.

## Examples {#section_phk_nl3_ygb .section}

```
ALTER TABLE users RENAME TO people; --- Renames a table.
ALTER TABLE users ADD COLUMN zip varchar; --- Adds a column.
ALTER TABLE users DROP COLUMN zip; --- Deletes a column.
ALTER TABLE users RENAME COLUMN id TO user_id; --- Renames a column.
```

