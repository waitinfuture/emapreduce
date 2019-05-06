# Logical operators {#concept_eyy_c1b_ygb .concept}

Presto supports AND, OR, and NOT logical operators, and supports `NULL` in logical computation. For example:

```
SELECT CAST(null as boolean) AND true; --- null
SELECT CAST(null AS boolean) AND false; -- false
SELECT CAST(null AS boolean) AND CAST(null AS boolean); -- null
SELECT NOT CAST(null AS boolean); -- null

```

A complete truth table is as follows:

|a|b|a AND b|a OR b|
|--|--|-------|------|
|`TRUE`|`TRUE`|`TRUE`|`TRUE`|
|`TRUE`|`FALSE`|`FALSE`|`TRUE`|
|`TRUE`|`NULL`|`NULL`|`TRUE`|
|`FALSE`|`TRUE`|`FALSE`|`TRUE`|
|`FALSE`|`FALSE`|`FALSE`|`FALSE`|
|`FALSE`|`NULL`|`FALSE`|`NULL`|
|`NULL`|`TRUE`|`NULL`|`TRUE`|
|`NULL`|`FALSE`|`FALSE`|`NULL`|
|`NULL`|`FALSE`|`NULL`|`NULL`|

The result of `NOT NULL` is `NULL`.

