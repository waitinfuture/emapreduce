# Comparison functions and operators {#concept_jm4_wdb_ygb .concept}

## Comparison operators {#section_as3_s2b_ygb .section}

Presto supports the following comparison operators:

|Operator|Description|
|--------|-----------|
|`<`|Less than|
|`>`|Greater than|
|`<=`|Less than or equal to|
|`>=`|Greater than or equal to|
|`=`|Equal to|
|`<>`/`! =`|Not equal to|
|`[NOT] BETWEEN`|Value X \[NOT\] between the minimum and maximum values|
|`IS [NOT] NULL`|Determines if a value is NULL.|
|`IS [NOT] DISTINCT FROM`|Determines if two values are identical. Generally, `NULL` indicates an unknown value, so any comparison that involves a `NULL` returns `NULL`. However, the `IS [NOT] DISTINCT FROM` operator treats `NULL` as a known value, and returns a `TRUE` or `FALSE` result.|

## Comparison functions {#section_is3_s2b_ygb .section}

Presto provides the following comparison functions:

-   `GREATEST`

    Returns the maximum value among all input values.

    Example: `GREATEST(1, 2)`

-   `LEAST`

    Returns the minimum value among all input values.

    Example: `LEAST(1, 2)`


## Quantified comparison predicates {#section_ls3_s2b_ygb .section}

Presto also provides several quantified comparison predicates to improve the comparison expressions. The method is as follows:

```
<EXPRESSION><OPERATOR><QUANTIFIER> (<SUBQUERY>)

```

Examples:

```
SELECT 'hello' = ANY (VALUES 'hello', 'world'); -- true
SELECT 21 < ALL (VALUES 19, 20, 21); -- false
SELECT 42 >= SOME (SELECT 41 UNION ALL SELECT 42 UNION ALL SELECT 43); -- true

```

`ANY`, `ALL`, and `SOME` are quantified comparison predicates.

-   `A = ALL (...)` `TRUE` is returned if A is equal to ALL value.
-   `A <> ALL (...)` `TRUE` is returned if A is not equal to ALL value.
-   `A < ALL (...)` `TRUE` is returned if A is less than ALL value.
-   `A = ANY (...)` `TRUE` is returned if A is equal to any of the values. It is equivalent to `A IN (...)`.
-   `A <> ANY (...)` `TRUE` is returned if A is not equal to any of the values. It is equivalent to `A IN (...)`.
-   `A < ANY (...)` `TRUE` is returned if A is less than one of the values.

`ANY` and `SOME` have the same meaning and can be used interchangeably.

