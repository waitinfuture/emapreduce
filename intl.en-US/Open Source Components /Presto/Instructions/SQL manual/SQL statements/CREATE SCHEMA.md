# CREATE SCHEMA {#concept_d21_pn3_ygb .concept}

## Overview {#section_p5k_ll3_ygb .section}

```
CREATE SCHEMA [ IF NOT EXISTS ] schema_name
[ WITH ( property_name = expression [, ...] ) ]
```

## Description {#section_fym_ml3_ygb .section}

Creates a schema. A schema is a container that holds tables, views, and other database objects.

-   Use the `IF NOT EXISTS` clause to suppress the exception that is thrown when the schema to be created already exists.
-   Use the `WITH` clause to set properties for the new schema. To list all available schema properties, run the following statement:

`SELECT * FROM system.metadata.schema_properties;`

## Examples {#section_phk_nl3_ygb .section}

```
CREATE SCHEMA web;
CREATE SCHEMA hive.sales;
CREATE SCHEMA IF NOT EXISTS traffic;
```

