# CALL {#concept_v2y_3m3_ygb .concept}

## Overview {#section_p5k_ll3_ygb .section}

```
CALL procedure_name ( [ name => ] expression [, ...] )
```

## Description {#section_fym_ml3_ygb .section}

Calls a stored procedure. Connectors can provide stored procedures to perform data manipulation or administrative tasks. Some connectors, such as the PostgreSQL connector, are intended for underlying systems that have their own stored procedures. These systems must use the stored procedures provided by the connectors to access their own stored procedures, which are not directly callable through `CALL`.

## Examples {#section_phk_nl3_ygb .section}

```
CALL test(123, 'apple'); --- Calls a stored procedure by using positional arguments.
CALL test(name => 'apple', id => 123); --- Calls a stored procedure by using named arguments.
CALL catalog.schema.test(); --- Calls a stored procedure using a fully qualified name.
```

