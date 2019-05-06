# EXECUTE {#concept_vgy_lzj_zgb .concept}

## Overview {#section_p5k_ll3_ygb .section}

```
EXECUTE statement_name [ USING parameter1 [ , parameter2, ... ] ]
```

## Description {#section_fym_ml3_ygb .section}

Executes a precompiled query statement. Arguments are defined in the `USING` clause.

## Examples {#section_phk_nl3_ygb .section}

-   Example 1:

    ```
    PREPARE my_select1 FROM
    SELECT name FROM nation;
    --- Execute a precompiled query statement
    EXECUTE my_select1;
    Example 2
    ```

-   Example 2:

    ```
    PREPARE my_select2 FROM
    SELECT name FROM nation WHERE regionkey = ? and nationkey < ? ;
    --- Execute a precompiled query statement
    EXECUTE my_select2 USING 1, 3; 
    --- The preceding statement is equivalent to this statement:
    SELECT name FROM nation WHERE regionkey = 1 AND nationkey < 3;
    ```


