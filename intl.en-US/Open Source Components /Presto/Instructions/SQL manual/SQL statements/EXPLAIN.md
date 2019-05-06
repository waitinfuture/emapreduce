# EXPLAIN {#concept_x3s_21k_zgb .concept}

## Overview {#section_p5k_ll3_ygb .section}

```
EXPLAIN [ ( option [, ...] ) ] statement

"option" can be one of:

    FORMAT { TEXT | GRAPHVIZ }
    TYPE { LOGICAL | DISTRIBUTED | VALIDATE }
```

## Description {#section_fym_ml3_ygb .section}

Implements one of the following functions based on the used option:

-   Shows the logical plan of a query statement.
-   Shows the distributed execution plan of a query statement.
-   Validates a query statement.

Use the `TYPE DISTRIBUTED` option to display plan fragments. Each plan fragment is executed by a single or multiple Presto nodes. Fragments separation represents the data exchange between Presto nodes. The fragment type specifies how the fragment is executed by Presto nodes and how data is distributed between fragments. The following fragment types are available:

-   `SINGLE`: Fragments are executed on a single node.
-   `HASH`: Fragments are executed on a fixed number of nodes with the input data distributed by using a hash function.
-   `ROUND_ROBIN`: Fragments are executed on a fixed number of nodes with the input data distributed in a `ROUND-ROBIN` fashion.
-   `BROADCAST`: Fragments are executed on a fixed number of nodes with the input data broadcast to all nodes.
-   `SOURCE`: Fragments are executed on nodes where data is stored.

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


