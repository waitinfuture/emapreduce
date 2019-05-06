# Conversion functions {#concept_xfn_w3b_ygb .concept}

Presto provides the following explicit conversion functions:

-   `CAST`

    Explicitly casts a value as a type, and throws an exception if the cast fails. The usage is as follows:

    ```
    CAST(value AS type) -> value1:type
    
    ```

-   `TRY_CAST`

    Similar to `CAST`, but returns `NULL` if the cast fails. The usage is as follows:

    ```
    TRY_CAST(value AS TYPE) -> value1:TYPE | NULL
    
    ```

-   `TYPEOF`

    Returns the name of the type of the provided parameter or expression value. The usage is as follows:

    ```
    TYPEOF(expression) -> type:VARCHAR
    
    ```

    Examples:

    ```
    SELECT TYPEOF(123); -- integer
    SELECT TYPEOF('cat'); -- varchar(3)
    SELECT TYPEOF(cos(2) + 1.5); -- double
    ```


