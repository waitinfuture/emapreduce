# Conditional expressions {#concept_lwn_vfb_ygb .concept}

Conditional expressions are mainly used to express branch logic. Presto supports the following conditional expressions:

-   `CASE` expression

    The standard SQL `CASE` expression has two different forms:

    ```
    CASE expression
    	WHEN <value|condition> THEN result 
    	[ WHEN ... ]
    	[ ELSE result]
    END
    
    ```

    The `CASE` statement compares the `expression` and the value/condition in `value|condition`. It returns a result if the same value is found or the condition is met.

    Examples:

    ```
    --- Compare values
    SELECT a,
           CASE a
               WHEN 1 THEN 'one'
               WHEN 2 THEN 'two'
               ELSE 'many'
           END
    
    ```

    ```
    --- Compare conditional expressions
    SELECT a, b,
           CASE
               WHEN a = 1 THEN 'aaa'
               WHEN b = 2 THEN 'bbb'
               ELSE 'ccc'
           END
    
    ```

-   `IF` function

    The `IF` function is a simple comparison function that is used to simplify the writing method for the comparison logic of two values. Its expression form is as follows:

    ```
    IF(condition, true_value, [false_value])
    
    ```

    Returns `true_value` if `condition` is `TRUE`, or returns `false_value` otherwise. `false_value` is optional. If it is not specified, `NULL` is returned.

-   `COALESCE`

    The `COALESCE` function returns the first non-`null` value in the argument list. Its expression form is as follows:

    ```
    COALESCE(value1, value2[, ...])
    
    ```

-   `NULLIF`

    The `NULLIF` function returns `NULL` if `value1` equals `value2`. Otherwise, it returns `value1`. The usage is as follows:

    ```
    NULLIF(value1, value2)
    
    ```

-   `TRY`

    The `TRY` function captures the exception that is thrown during `expression` computation and returns `NULL`. The following exceptions are handled by `TRY`:

    -   Division by zero, such as `x/0`
    -   Incorrect type conversion
    -   Numeric value out of range
    TRY is typically used in conjunction with `COALESCE` to return the default value in the case of errors. The usage is as follows:

    ```
    TRY(expression)
    
    ```

    Examples:

    ```
    --- When COALESCE and TRY are used in conjunction, the default value 0 is returned if packages is equal to 0 and a "division by zero" error is thrown.
    SELECT COALESCE(TRY(total_cost / packages), 0) AS per_package FROM shipping;
    
    per_package
    -------------
       		4
       	   14
       		0
       	   19
    (4 rows)
    ```


