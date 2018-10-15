# Common functions and operators {#concept_gdq_tm2_z2b .concept}

This article describes common Presto functions and operators.

## Logical operators {#section_hhk_c5j_z2b .section}

Presto supports AND, OR, and NOT logical operators, and supports NULL in logical computation. For example:

```
SELECT CAST(null as boolean) AND true; --- null
SELECT CAST(null AS boolean) AND false; -- false
SELECT CAST(null AS boolean) AND CAST(null AS boolean); -- null
SELECT NOT CAST(null AS boolean); -- null
```

A complete truth table is shown as follows:

|a|b|a AND b|A or B|
|:-|:-|:------|:-----|
|TRUE|TRUE|TRUE|TRUE|
|TRUE|FALSE|FALSE|TRUE|
|TRUE|NULL|NULL|TRUE|
|FALSE|TRUE|FALSE|TRUE|
|FALSE|FALSE|FALSE|FALSE|
|FALSE|NULL|FALSE|NULL|
|NULL|TRUE|NULL|TRUE|
|NULL|FALSE|FALSE|NULL|
|NULL|FALSE|NULL|NULL|

Additionally, the result of NOT FALSE is TRUE, the result of NOT TRUE is FALSE, and the result of NOT NULL is NULL. For more information about the NOT operator, see [NOT operator](http://www.veryhuo.com/a/manual/vbscript/html/vsoprNot.htm).

Comparison functions and operators

-   Comparison operators:

    Comparison operations supported by Presto are as follows:

    |Operator|Description|
    |:-------|:----------|
    |<|Less than|
    |\>|Greater than|
    |<=|Less than or equal to|
    |\>=|Greater than or equal to|
    |=|Equal to|
    |<\>/**! =**|Not equal to|
    |\[NOT\] BETWEEN|Value X is \[not\] between the min and the max values|
    |IS \[NOT\] NULL|Tests whether a value is NULL|
    |IS \[NOT\] DISTINCT FROM|Determines if two values are identical. Generally, NULL signifies an unknown value, so any comparison involving a NULL will produce NULL. However, the IS \[NOT\] DISTINCT FROM operator treats NULL as a known value, and returns a TRUE or FALSE result.|

-   **Comparison functions**

    Presto provides the following comparison related functions:

    -   GREATEST

        Returns the largest of the provided values.

        Example:`GREATEST(1, 2)`

    -   LEAST

        Returns the smallest of the provided values.

        Example: `LEAST(1, 2)`

-   **Quantified comparison predicates**

    Presto also provides several quantified comparison predicates to enhance the comparison expressions. The usage is as follows:

    ```
    <EXPRESSION> <OPERATOR> <QUANTIFIER> (<SUBQUERY>)
    ```

    For example:

    ```
    SELECT 21 < ALL (VALUES 19, 20, 21); -- false
    SELECT 42 >= SOME (SELECT 41 UNION ALL SELECT 42 UNION ALL SELECT 43); -- true
    ```

    ALL, ANY and SOME are quantified comparison predicates.

    -   `A = ALL (…)`: Evaluates to true when A is equal to all values. Example: `SELECT 21 = ALL (VALUES 20, 20, 20);` , return TRUE.
    -   `A <> ALL (…)`: Evaluates to true when A doesn’t match any value. Example: `SELECT 21 <> ALL (VALUES 19, 20, 22);` , return TRUE.
    -   `A < ALL (…)`: Evaluates to true when A is smaller than the smallest value. Example: `SELECT 18 < ALL (VALUES 19, 20, 22);` , return TRUE.
    -   `A = ANY (…)`: Evaluates to true when A is equal to any of the values. This form is equivalent to `A IN (…)`. Example: `SELECT ‘hello’ = ANY (VALUES ‘hello’, ‘world’);` , return TRUE.
    -   `A <> ANY (…)`: Evaluates to true when A doesn’t match one or more values. This form is equivalent to `A IN (…)`. Example: `SELECT 21 &lt;&gt; ALL (VALUES 19, 20, 21);`, return TRUE.
    -   `A < ANY (…)`: Evaluates to true when A is smaller than the biggest value. Example: `SELECT 21 < ALL (VALUES 19, 20, 22);`, return TRUE.
    ANY and SOME have the same meaning and can be used interchangeably.


## Conditional expressions {#section_gcp_dxj_z2b .section}

Conditional expressions are mainly used to express branch logic. Presto supports the following conditional expressions:

-   CASE expression

    The standard SQL CASE expression has two different forms:

    ```
    CASE expression
        WHEN <value|condition> THEN result
        [ WHEN ... ]
        [ ELSE result]
    END
    ```

    The expression statement compares the expression and the value/condition in value|condition. It returns a result if the same value is found or the condition is met.

    Example:

    ```
    --- Compare value
    SELECT a,
           CASE a
               WHEN 1 THEN 'one'
               WHEN 2 THEN 'two'
               ELSE 'many'
           END
    ```

    ```
    --- Compare conditional expression
    SELECT a, b,
           CASE
               WHEN a = 1 THEN 'aaa'
               WHEN b = 2 THEN 'bbb'
               ELSE 'ccc'
           END
    ```

-   IF function

    The IF function is a simple comparison function used to simplify the writing method for comparison logic of two values. Its expression forms are as follows:

    ```
    IF(condition, true_value, [false_value])
    ```

    Evaluates and returns true\_value if condition is true, otherwise false\_value is returned. false\_value is optional. If it is not specified, NULL will be returned if condition is not true.

-   COALESCE

    The COALESCE function returns the first non-null value in the argument list. Its expression forms are as follows:

    ```
    COALESCE(value1, value2[, ...])
    ```

-   NULLIF

    The COALESCE function returns null if value1 equals value2, otherwise returns value1. Usage of the function is as follows:

    ```
    NULLIF(value1, value2)
    ```

-   TRY

    The TRY function evaluates an expression and handle certain types of errors by returning NULL. The following errors are handled by TRY:

    -   Division by zero, e.g. x/0
    -   Invalid cast or function argument
    -   Numeric value out of range
    Generally used in conjunction with COALESCE to return the default value in case of errors. The usage is as follows:

    ```
    TRY(expression)
    ```

    Example:

    ```
    --- When COALESCE and TRY are used in conjunction, if packages=0, and a "division by zero" error is thrown, the default value (0) will be returned.
    SELECT COALESCE(TRY(total_cost / packages), 0) AS per_package FROM shipping;
    per_package
    -------------
               4
              14
               0
              19
    (4 rows)
    ```


## Conversion functions {#section_rpc_k4k_z2b .section}

Presto provides the following explicit conversion functions:

-   CAST

    Explicitly casts a value as a type, and raises an error if the cast fails. The usage is as follows:

    ```
    CAST(value AS type) -> value1:type
    ```

-   TRY\_CAST

    Like cast, but returns null if the cast fails. The usage is as follows:

    ```
    TRY_CAST(value AS TYPE) -> value1:TYPE | NULL
    ```

-   TYPEOF

    Returns the name of the type of the provided parameter or expression value. The usage is as follows:

    ```
    TYPEOF(expression) -> type:VARCHAR
    ```

    Example:

    ```
    SELECT TYPEOF(123); -- integer
    SELECT TYPEOF('cat'); -- varchar(3)
    SELECT TYPEOF(cos(2) + 1.5); -- double
    ```


## Mathematical functions and operators { .section}

-   **Mathematical operators**

    |Operator|Description|
    |:-------|:----------|
    |+|Addition|
    |-|Subtraction|
    |\*|Multiplication|
    |/|Division \(integer division performs truncation\)|
    |%|Modulus \(remainder\)|

-   **Mathematical functions**

    Presto provides a wealth of mathematical functions, as shown in the following table:

    |Function|Syntax|Description|
    |:-------|:-----|:----------|
    |abs|abs\(x\) →|Returns the absolute value of x.|
    |cbrt|cbrt\(x\) → double|Returns the cube root of x.|
    |ceil|ceil\(x\)|Returns x rounded up to the nearest integer. This is an alias for **ceiling**.|
    |ceiling|ceiling\(x\)|Returns x rounded up to the nearest integer.|
    |cosine\_similarity|cosine\_similarity\(x, y\) → double|Returns the cosine similarity between the sparse vectors x and y.|
    |degrees|degress\(x\) -\> double|Converts angle x in radians to degrees.|
    |e|e\(\)-\>double|Returns the constant Euler’s number.|
    |exp|exp\(x\)-\>double|Returns Euler’s number raised to the power of x.|
    |floor|floor\(x\)|Returns x rounded down to the nearest integer.|
    |from\_base|from\_base\(string, radix\) → bigint|Returns the value of string interpreted as a base-radix number.|
    |inverse\_normal\_cdf|inverse\_normal\_cdf\(mean,sd,p\)-\>double|Computes the inverse of the Normal cdf with given mean and standard deviation \(sd\) for the cumulative probability.|
    |ln|ln\(x\)-\>double|Returns the natural logarithm of x.|
    |log2|log2\(x\)-\>double|Returns the base 2 logarithm of x.|
    |log10|log10\(x\)-\>double|Returns the base 10 logarithm of x.|
    |log|log\(x,b\) -\> double|Returns the base b logarithm of x.|
    |mod|mod\(n,m\)|Returns the modulus \(remainder\) of n divided by m.|
    |pi|pi\(\)-\>double|Returns the constant Pi.|
    |pow|pow\(x,p\)-\>double|Returns x raised to the power of p. This is an alias for **power**.|
    |power|power\(x,p\)-\>double|Returns x raised to the power of p.|
    |radians|radians\(x\)-\>double|Converts angle x in degrees to radians.|
    |rand|rand\(\)-\>double|Returns a pseudo-random value in the range 0.0 <= x < 1.0. This is an alias for **random**.|
    |random|random\(\)-\>double|Returns a pseudo-random value in the range 0.0 <= x < 1.0.|
    |random|random\(n\)|Returns a pseudo-random number between 0 and n \(exclusive\).|
    |round|round\(x\)|Returns x rounded to the nearest integer.|
    |round|round\(x, d\)|Returns x rounded to d decimal places.|
    |sign|sign\(x\)|Returns the signum function of x, that is: 0 if the argument is 0; if the argument is greater than 0; -1 if the argument is less than 0. For double arguments, the function additionally returns: NaN if the argument is NaN; 1 if the argument is +Infinity; -1 if the argument is -Infinity.|
    |sqrt|sqrt\(x\)-\>double|Returns the square root of x.|
    |to\_base|to\_base\(x, radix\)-\>varchar|Returns the base-radix representation of x.|
    |truncate|truncate\(x\) → double|Returns x rounded to integer by dropping digits after decimal point.|
    |width\_bucket|width\_bucket\(x, bound1, bound2, n\) → bigint|Returns the bin number of x in an equi-width histogram with the specified bound1 and bound2 bounds and n number of buckets.|
    |width\_bucket|width\_bucket\(x, bins\)|Returns the bin number of x according to the bins specified by the array bins.|
    |acos|acos\(x\)-\>double|Returns the arc cosine of x, which is a radian.|
    |asin|asin\(x\)-\>double|Returns the arc sine of x, which is a radian.|
    |atan|atan\(x\)-\>double|Returns the arc tangent of x, which is a radian.|
    |atan2|atan2\(y,x\)-\>double|Returns the arc tangent of y / x, which is a radian.|
    |cos|cos\(x\)-\>double|Returns the cosine of x, which is a radian.|
    |cosh|cosh\(x\)-\>double|Returns the hyperbolic cosine of x, which is a radian.|
    |sin|sin\(x\)-\>double|Returns the sine of x, which is a radian.|
    |tan|tan\(x\)-\>double|Returns the tangent of x, which is a radian.|
    |tanh|tanh\(x\)-\>double|Returns the hyperbolic tangent of x, which is a radian.|
    |infinity|infinity\(\) → double|Returns the constant representing positive infinity.|
    |is\_finite|is\_finite\(x\) → boolean|Determines if x is finite.|
    |is\_infinite|is\_infinite\(x\) → boolean|Determines if x is infinite.|
    |is\_nan|is\_nan\(x\) → boolean|Determines if x is not-a-number.|
    |nan|nan\(\)|Returns the constant representing not-a-number.|


## Bitwise functions {#section_el2_cvk_z2b .section}

Presto provides following bitwise functions:

|Function|Syntax|Description|
|:-------|:-----|:----------|
|bit\_count|bit\_count\(x, bits\) → bigint|Returns the number of bits set in x at position 1 in 2’s complement representation.|
|bitwise\_and|bitwise\_and\(x, y\) → bigint|The bitwise AND function|
|bitwise\_not|bitwise\_not\(x\) → bigint|The bitwise NOT function|
|bitwise\_or|bitwise\_or\(x, y\) → bigint|The bitwise OR function|
|bitwise\_xor|bitwise\_xor\(x, y\) → bigint|The bitwise XOR function|
|bitwise\_and\_agg|bitwise\_and\_agg\(x\) → bigint|Returns the bitwise AND of all input values in 2’s complement representation, and x is an array.|
|bitwise\_or\_agg|bitwise\_or\_agg\(x\) → bigint|Returns the bitwise OR of all input values in 2’s complement representation, and x is an array.|

Examples

```
SELECT bit_count(9, 64); -- 2
SELECT bit_count(9, 8); -- 2
SELECT bit_count(-7, 64); -- 62
SELECT bit_count(-7, 8); -- 6
```

## Decimal functions and operators { .section}

-   **Decimal literals**

    Use the following syntax to define literal of DECIMAL type:

    ```
    DECIMAL 'xxxx.yyyyy'
    ```

    The precision of DECIMAL type for literal will be equal to number of digits in literal \(including trailing and leading zeros\). The scale will be equal to number of digits in fractional part \(including trailing zeros\). For example:

    |Example literal|Data type|
    |:--------------|:--------|
    |DECIMAL ‘0’|DECIMAL\(1\)|
    |DECIMAL ‘12345’|DECIMAL\(5\)|
    |DECIMAL ‘0000012345.1234500000’|DECIMAL\(20, 10\)|

-   **Operators**

    Arithmetic operators

    Assuming x is of type DECIMAL\(xp, xs\) and y is of type DECIMAL\(yp, ys\),

    -   x: DECIMAL\(xp,xs\)
    -   y: DECIMAL\(yp,ps\)
    and they observe the following rules when used in arithmetic operation:

    -   x + y or x - y
        -   precision = min\(38, 1 + min\(xs, ys\) + min\(xp-xs, yp-ys\)\)
        -   scale = max\(xs, ys\)
    -   x \* y

        -   precision = min\(38, xp + yp\)
        -   scale = xs + ys
    -   x / y

        -   precision = min\(38, xp + ys + max\(0, ys-xs\)\)
        -   scale = max\(xs, ys\)
    -   x % y

        -   precision = min\(xp - xs, yp - ys\) + max\(xs, bs\)
        -   scale = max\(xs, ys\)
    -   Comparison operators

        All standard comparison operators and BETWEEN operator work for DECIMAL type.

    -   Unary decimal operators

        The - operator performs negation for DECIMAL type.


## String functions and operators { .section}

-   **Concatenation operator**

    The `||` operator performs concatenation.

-   **String functions**

    String functions supported by Presto are listed in the following table:

    |Function Name|Syntax|Description|
    |:------------|:-----|:----------|
    |chr|chr\(n\) → varchar|Returns the Unicode code point n as a single character string.|
    |codepoint|codepoint\(string\) → integer|Returns the Unicode code point of the only character of string.|
    |concat|concat\(string1, …, stringN\) → varchar|Returns the concatenation of string1, string2, …, stringN. This function provides the same functionality as the SQL-standard concatenation operator.|
    |hamming\_distance|hamming\_distance\(string1, string2\) → bigint|Returns the [Hamming distance](https://en.wikipedia.org/wiki/Hamming_distance) of string1 and string2, i.e. the number of positions at which the corresponding characters are different. Note that the two strings must have the same length.|
    |length|length\(string\) → bigint|Returns the length of string in characters.|
    |levenshtein\_distance|levenshtein\_distance\(string1, string2\) → bigint|Returns the [Levenshtein edit distance](https://en.wikipedia.org/wiki/Levenshtein_distance) of string1 and string2.|
    |lower|lower\(string\) → varchar|Converts string to lowercase.|
    |upper|upper\(string\) → varchar|Converts string to uppercase.|
    |replace|replace\(string, search\) → varchar|Removes all instances of search from string.|
    |replace|replace\(string, search, replace\) → varchar|Replaces all instances of search with replace in string.|
    |reverse|reverse\(string\) → varchar|Returns string with the characters in reverse order.|
    |lpad|lpad\(string, size, padstring\) → varchar|Left pads string to size characters with padstring.If size is less than the length of string, the result is truncated to size characters. size must not be negative and padstring must be non-empty.|
    |rpad|rpad\(string, size, padstring\) → varchar|Right pads string to size characters with padstring.If size is less than the length of string, the result is truncated to size characters. size must not be negative and padstring must be non-empty.|
    |ltrim|ltrim\(string\) → varchar|Removes leading whitespace from string.|
    |rtrim|rtrim\(string\) → varchar|Removes trailing whitespace from string.|
    |split|split\(string, delimiter\) → array|Splits string on delimiter and returns an array.|
    |split|split\(string, delimiter, limit\) → array|Splits string on delimiter and returns an array of size at the maximum of limit.|
    |split\_part|split\_part\(string, delimiter, index\) → varchar|Splits string on delimiter and returns the field index. Field indexes start with 1.|
    |split\_to\_map|split\_to\_map\(string, entryDelimiter, keyValueDelimiter\) → map|Splits string by entryDelimiter and keyValueDelimiter and returns a map.|
    |strpos|strpos\(string, substring\) → bigint|Returns the starting position of the first instance of substring in string. Positions start with 1. If not found, 0 is returned.|
    |position|position\(substring IN string\) → bigint|Returns the starting position of the first instance of substring in string.|
    |substr|substr\(string, start, \[length\]\) → varchar|Returns a substring from string of \[length\] length from the starting position start. Positions start with 1. The length parameter is optional.|

-   **Unicode functions**
    -   normalize\(string\) → varchar

        Transforms string with [NFC normalization form](https://en.wikipedia.org/wiki/Unicode_equivalence#Normalization).

    -   normalize\(string, form\) → varchar

        Transforms string with the specified normalization form. form must be one of the following keywords:

        -   NFD Canonical Decomposition
        -   NFC Canonical Decomposition, followed by Canonical Composition
        -   NFKD Compatibility Decomposition
        -   NFKC Compatibility Decomposition, followed by Canonical Composition
    -   to\_utf8\(string\) → varbinary

        Encodes string into a UTF-8 varbinary representation.

    -   from\_utf8\(binary, \[replace\]\) → varchar

        Decodes a UTF-8 encoded string from binary. Invalid UTF-8 sequences are replaced with replace, which is Unicode replacement character U+FFFD by default. Note that the replacement string replace must either be a single character or empty.


## Regular expression functions { .section}

Presto supports all of the regular expression functions use the [Java Pattern](http://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html) syntax, with a few notable exceptions:

-   When using multi-line mode

    -   enabled via the `? m` flag.
    -   `\n` is recognized as a line terminator
    -   the `? d` flag is not supported
-   Case-insensitive matching

    -   enabled via the `? i` flag
    -   the `? u` flag is not supported
    -   context-sensitive matching is not supported
    -   local-sensitive matching is not supported
-   Surrogate pairs are not supported

    For example, `\uD800\uDC00` is not treated as `U+10000` and must be specified as `\x{10000}`.

-   Boundaries `\b` are incorrectly handled for a non-spacing mark without a base character.
-   `\Q` and `\E` are not supported in character classes \(such as `[A-Z123]`\) and are instead treated as literals.
-   Unicode character classes \(`\p{prop}`\) are supported with the following differences:

-   All underscores in names must be removed. For example, use `OldItalic` instead of `Old_Italic`.
-   Scripts must be specified directly, without the `Is`, `script=` or `sc=` prefixes. Example:`\p{Hiragana}` instead of `\p{script=Hiragana}`.
-   Blocks must be specified with the `In` prefix. The `block=` and `blk=` prefixes are not supported. Example: `\p{InMongolia}`.
-   Categories must be specified directly, without the `Is`, `general_category=` or `gc=` prefixes. Example: `\p{L}`.
-   Binary properties must be specified directly, without the Is. Example: use `\p{NoncharacterCodePoint}` instead of `\p{IsNoncharacterCodePoint}`.

Regular expression functions provided by Presto are as follows:

-   regexp\_extract\_all\(string, pattern, \[group\]\) → array

    Returns the substring\(s\) matched by the regular expression pattern in string. If the pattern expression uses the grouping function, then the group parameter can be set to specify the [capturing group](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html#gnumber%C3%9F).

    Examples

    ```
    SELECT regexp_extract_all('1a 2b 14m', '\d+'); -- [1, 2, 14]
    SELECT regexp_extract_all('1a 2b 14m', '(\d+)([a-z]+)', 2); -- ['a', 'b', 'm']
    ```

-   regexp\_extract\(string, pattern, \[group\]\) → varchar

    The function and usage is similar to those of `regexp_extract_all`. The difference is that this function only returns the first substring matched by the regular expression.

    Examples

    ```
    SELECT regexp_extract_all('1a 2b 14m', '\d+'); -- [1, 2, 14]
    SELECT regexp_extract_all('1a 2b 14m', '(\d+)([a-z]+)', 2); -- ['a', 'b', 'm']
    ```

-   regexp\_extract\_all\(string, pattern, \[group\]\) → array

    Returns the substring\(s\) matched by the regular expression pattern in string: If the pattern expression uses the grouping function, then the group parameter can be set to specify the [capturing group](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html#gnumber%C3%9F) to be matched by the regular expression.

    Examples

    ```
    SELECT regexp_extract('1a 2b 14m', '\d+'); -- 1
    SELECT regexp_extract('1a 2b 14m', '(\d+)([a-z]+)', 2); -- 'a'
    ```

-   regexp\_like\(string, pattern\) → boolean

    Evaluates the regular expression pattern and determines if it is contained within string. It returns TRUE if yes, and False if otherwise. This function is similar to the LIKE operator, expect that the pattern only needs to be contained within string, rather than needing to match all of string.

    Examples

    ```
    SELECT regexp_like('1a 2b 14m', '\d+b'); -- true
    ```

-   regexp\_replace\(string, pattern, \[replacement\]\) → varchar

    Replaces every instance of the substring matched by the regular expression pattern in string with replacement. replacement is optional, and will be replaced by **‘’**\(deleting the matched substrings\) if it is not specified.

    Capturing groups can be referenced in replacement using $g \(g is the ordinal number, starting at one\) for a numbered group or $\{name\} for a named group. A dollar sign $ may be included in the replacement by escaping it with a backslash `\$`.

    Examples

    ```
    SELECT regexp_replace('1a 2b 14m', '\d+[ab] '); -- '14m'
    SELECT regexp_replace('1a 2b 14m', '(\d+)([ab]) ', '3c$2 '); -- '3ca 3cb 14m'
    ```

-   regexp\_split\(string, pattern\) → array

    Splits string using the regular expression pattern and returns an array. Trailing empty strings are preserved.

    Examples

    ```
    SELECT regexp_split('1a 2b 14m', '\s*[a-z]+\s*'); -- ['1', '2', '14', ''] 4 elements
                                                      -- The last one is an empty string
    ```


## Binary functions and operators { .section}

-   **Concatenation operator**

    The `||` operator performs binary concatenation.

-   **Binary functions**

    |Function|Syntax|Description|
    |:-------|:-----|:----------|
    |length|length\(binary\) → bigint|Returns the length of binary in bytes.|
    |concat|concat\(binary1, …, binaryN\) → varbinary|Returns the concatenation of binary1, binary2, …, binaryN.|
    |to\_base64|to\_base64\(binary\) → varchar|Encodes binary into a base64 string representation.|
    |from\_base64|from\_base64\(string\) → varbinary|Decodes binary data from the base64 encoded string.|
    |to\_base64url|to\_base64url\(binary\) → varchar|Encodes binary into a base64 string representation using the URL safe alphabet.|
    |from\_base64url|from\_base64url\(string\) → varbinary|Decodes binary data from the base64 encoded string using the URL safe alphabet.|
    |to\_hex|to\_hex\(binary\) → varchar|Encodes binary into a hex string representation.|
    |from\_hex|from\_hex\(string\) → varbinary|Decodes binary data from the hex encoded string.|
    |to\_big\_endian\_64|to\_big\_endian\_64\(bigint\) → varbinary|Encodes bigint in a 64-bit 2’s complement big endian format.|
    |from\_big\_endian\_64|from\_big\_endian\_64\(binary\) → bigint|Decodes bigint value from a 64-bit 2’s complement big endian binary.|
    |to\_ieee754\_32|to\_ieee754\_32\(real\) → varbinary|Encodes real in a 32-bit big-endian binary according to [IEEE 754](http://grouper.ieee.org/groups/754/) single-precision floating-point format.|
    |to\_ieee754\_64|to\_ieee754\_64\(double\) → varbinary|Encodes double in a 64-bit big-endian binary according to [IEEE 754](http://grouper.ieee.org/groups/754/) double-precision floating-point format.|
    |crc32|crc32\(binary\) → bigint|Computes the CRC-32 of binary.|
    |md5|md5\(binary\) → varbinary|Computes the md5 hash of binary.|
    |sha1|sha1\(binary\) → varbinary|Computes the sha1 hash of binary.|
    |sha256|sha256\(binary\) → varbinary|Computes the sha256 hash of binary.|
    |sha512|sha512\(binary\) → varbinary|Computes the sha512 hash of binary.|
    |xxhash64|xxhash64\(binary\) → varbinary|Computes the xxhash64 hash of binary.|


## Date and time functions and operators { .section}

-   **Date and time operators**

    Presto supports two date and time operators: `+` and `-`.

    Examples

    ```
    --- +
     date '2012-08-08' + interval '2' day                --- 2012-08-10
     time '01:00' + interval '3' hour                    --- 04:00:00.000
     timestamp '2012-08-08 01:00' + interval '29' hour    --- 2012-08-09 06:00:00.000
     timestamp '2012-10-31 01:00' + interval '1' month    --- 2012-11-30 01:00:00.000
     interval '2' day + interval '3' hour                --- 2 03:00:00.000
     interval '3' year + interval '5' month                --- 3-5
     --- -
     date '2012-08-08' - interval '2' day                --- 2012-08-06
     time '01:00' - interval '3' hour                    --- 22:00:00.000
     timestamp '2012-08-08 01:00' - interval '29' hour    --- 2012-08-06 20:00:00.000
     timestamp '2012-10-31 01:00' - interval '1' month    --- 2012-09-30 01:00:00.000
     interval '2' day - interval '3' hour                --- 1 21:00:00.000
     interval '3' year - interval '5'                   --- month    2-7
    ```

-   **Time zone conversion**

    The `AT TIME ZONE` operator sets the time zone of a timestamp.

    Examples

    ```
    SELECT timestamp '2012-10-31 01:00 UTC'; 
     --- 2012-10-31 01:00:00.000 UTC
     SELECT timestamp '2012-10-31 01:00 UTC' AT TIME ZONE 'America/Los_Angeles'; 
     --- 2012-10-30 18:00:00.000 America/Los_Angeles
    ```

-   **Date and time functions**
    -   **Basic functions**

        |Function|Syntax|Description|
        |:-------|:-----|:----------|
        |current\_date|current\_date -\> date|Returns the current date as of the start of the query.|
        |current\_time|current\_time -\> time with time zone|Returns the current time as of the start of the query.|
        |current\_timestamp|current\_timestamp -\> timestamp with time zone|Returns the current timestamp as of the start of the query.|
        |current\_timezone|current\_timezone\(\) → varchar|Returns the current time zone.|
        |date|date\(x\) → date|Parses a date literal into a date|
        |from\_iso8601\_timestamp|from\_iso8601\_timestamp\(string\) → timestamp with time zone|Parses the ISO 8601 formatted string into a timestamp with time zone.|
        |from\_iso8601\_date|from\_iso8601\_date\(string\) → date|Parses the ISO 8601 formatted string into a date.|
        |from\_unixtime|from\_unixtime\(unixtime, \[timezone\_str\]\) → timestamp|Returns the UNIX timestamp as a timestamp. Timestamp option is allowed.|
        |from\_unixtime|from\_unixtime\(unixtime, hours, minutes\) → timestamp with time zone|Returns the UNIX timestamp as a timestamp with time zone using hours and minutes for the time zone offset.|
        |localtime|localtime -\> time|Returns the current time as of the start of the query.|
        |localtimestamp|localtimestamp -\> timestamp|Returns the current timestamp as of the start of the query.|
        |now|now\(\) → timestamp with time zone|Returns the current time. This is an alias for current\_time.|
        |to\_iso8601|to\_iso8601\(x\) → varchar|Formats x as an ISO 8601 string. x can be DATE, or TIMESTAMP \[with time zone\].|
        |to\_milliseconds|to\_milliseconds\(interval\) → bigint|Returns the day-to-second interval as milliseconds.|
        |to\_unixtime|to\_unixtime\(timestamp\) → double|Returns timestamp as a UNIX timestamp.|

        **Note:** The following SQL-standard functions do not use parenthesis:

        -   current\_data
        -   current\_time
        -   current\_timestamp
        -   localtime
        -   localtimestamp
    -   **Truncation function**

        The truncation function truncates date and time value by the specified unit, and returns the date and time value of this unit. The usage is as follows:

        ```
        date_trunc(unit, x) -> [same as x]
        ```

        where unit is one of:

        -   second: Seconds
        -   minute: Minutes
        -   hour: Hours
        -   day: Days
        -   week: Weeks
        -   month: Months
        -   quarter: Months
        -   year: Years
    -   **Interval functions**

        Presto provides two functions for interval calculation, which are:

        -   date\_add\(unit, value, timestamp\) → \[same as input\]

            Adds an interval value of type unit to timestamp. Subtraction can be performed by using a negative value with a unit.

        -   date\_diff\(unit, timestamp1, timestamp2\) → bigint

            Returns interval between two timestamps expressed in terms of unit.

            Where unit is one of:

            -   ns: Nanoseconds
            -   us: Microseconds
            -   ms: Milliseconds
            -   s: Seconds
            -   m: Minutes
            -   h: Hours
            -   d: Days
    -   **Date and time extraction functions**

        Presto provides a function `extract` to extract the specified fields from a date and time value, which is:

        extract\(field FROM x\) → bigint

        where, x is the date and time value, field is field to be extracted, which can be one of the following values:

        -   YEAR: Year
        -   QUARTER: Quarter of a year
        -   MONTH: Month
        -   WEEK: Week
        -   DAY: Day
        -   DAY\_OF\_MONTH: Day of a month
        -   DAY\_OF\_WEEK: Day of a week
        -   DOW: This is an alias for DAY\_OF\_WEEK
        -   DAY\_OF\_YEAR: Day of a year
        -   DOY: This is an alias for DAY\_OF\_YEAR
        -   YEAR\_OF\_WEEK: Year of an [ISO Week](https://en.wikipedia.org/wiki/ISO_week_date)
        -   YOW: This is an alias for YEAR\_OF\_WEEK
        -   HOUR: Hour
        -   MINUTE: Minute
        -   SECOND: Second
        -   TIMEZONE\_HOUR: Hour with timezone
        -   TIMEZONE\_MINUTE: Minute with timezone
        For the sake of convenience, Presto provides the following helper functions:

        |Function|Syntax|Description|
        |:-------|:-----|:----------|
        |day|day\(x\) → bigint|Returns the day of the month from x.|
        |day\_of\_month|day\_of\_month\(x\) → bigint|This is an alias for `day`.|
        |dayofweek|day\_of\_week\(x\) → bigint|Returns the ISO day of the week from x.|
        |day\_of\_year|day\_of\_year\(x\) → bigint|Returns the day of the year from x.|
        |dow|dow\(x\) → bigint|This is an alias for `day_of_week`.|
        |doy|doy\(x\) → bigint|This is an alias for `day_of_year`.|
        |hour|hour\(x\) → bigint|Returns the hour of the day from x. The value ranges from 0 to 23.|
        |minute|minute\(x\) → bigint|Returns the minute from x. The value ranges from 0 to 59.|
        |month|month\(x\) → bigint|Returns the month of the year from x. The value ranges from 1 to 12.|
        |quarter|quarter\(x\) → bigint|Returns the quarter of the year from x.|
        |second|second\(x\) → bigint|Returns the second from x. The value ranges from 0 to 59.|
        |timezone\_hour|timezone\_hour\(timestamp\) → bigint|Returns the hour of the time zone offset from timestamp.|
        |timezone\_minute|timezone\_minute\(timestamp\) → bigint|Returns the minute of the time zone offset from timestamp.|
        |week|week\(x\) → bigint|Returns the ISO week of the year from x. The value ranges from 1 to 53.|
        |week\_of\_year|week\_of\_year\(x\) → bigint|This is an alias for `week`.|
        |year|year\(x\) → bigint|Returns the year from x.|
        |year\_of\_week|year\_of\_week\(x\) → bigint|Returns the year of a week from x\([ISO Week](https://en.wikipedia.org/wiki/ISO_week_date)\).|
        |yow|yow\(x\) → bigint|This is an alias for `year_of_week`.|

    -   **MySQL date functions**

        Presto uses a format string that is compatible with MySQL `date_parse` and `str_to_date` functions, which are:

        -   date\_format\(timestamp, format\) → varchar

            Formats timestamp as a string using format.

        -   date\_parse\(string, format\) → timestamp

            Parses string into a timestamp using format.

        MySQL format specifiers supported by Presto are shown in the following table:

        |Specifier|Description|
        |:--------|:----------|
        |%a|Abbreviated weekday name \(Sun .. Sat\).|
        |%b|Abbreviated month name \(Jan .. Dec\).|
        |%c|Month, numeric \(1 .. 12\), cannot be zero|
        |%d|Day of the month, numeric \(01 .. 31\), cannot be zero|
        |%e|Day of the month, numeric \(1 .. 31\), cannot be zero|
        |%f|Fraction of second \(6 digits for printing: 000000 .. 999000; 1 - 9 digits for parsing: 0 .. 999999999\).|
        |%H|Hour \(00 .. 23\).|
        |%h|Hour \(01 .. 12\).|
        |%I|Hour \(01 .. 12\).|
        |%i|Minutes, numeric \(00 .. 59\).|
        |%j|Day of year \(001 .. 366\).|
        |%k|Hour \(0 .. 23\).|
        |%l|Hour \(1 .. 12\).|
        |%M|Month name \(January .. December\).|
        |%m|Month, numeric \(01 .. 12\) \[4\].|
        |%p|AM ／ PM|
        |%r|Time, 12-hour \(hh:mm:ss AM/PM\)|
        |%S|Seconds \(00 .. 59\).|
        |%s|Seconds \(00 .. 59\).|
        |%T|Time, 24-hour \(hh:mm:ss\)|
        |%v|Week \(01 .. 53\), where Monday is the first day of the week; used with `%X`|
        |%W|Weekday name \(Sunday .. Saturday\)|
        |%x|Year for the week, where Monday is the first day of the week, numeric, four digits|
        |%Y|Year, numeric, four digits|
        |%y|Year, numeric \(two digits\). When parsing, two-digit year format assumes range \[1970 .. 2069\]|
        |%%|A literal **‘%’** character|

        **Note:** The following specifiers are not currently supported: %D %U %u %V %w %X

    -   **Java date functions**

        The functions in this section use a format string that is compatible with [JodaTime’s DateTimeFormat pattern](http://joda-time.sourceforge.net/apidocs/org/joda/time/format/DateTimeFormat.html) format.

        -   format\_datetime\(timestamp, format\) → varchar: Formats timestamp
        -   parse\_datetime\(string, format\) → timestamp with time zone: Parses string into a timestamp

## Aggregate functions {#section_e2k_32l_z2b .section}

Aggregate functions have the following features:

-   Input a data set
-   Output a single computation result.

Almost all of these aggregate functions ignore `null` values and return `null` for no input rows or when all values are `null`, with a few notable exceptions:

-   count
-   count\_if
-   max\_by
-   min\_by
-   approx\_distinct

-   **Basic aggregate functions**

    |Function|Syntax|Description|
    |:-------|:-----|:----------|
    |arbitrary|arbitrary\(x\) → \[same as input\]|Returns an arbitrary non-null value of x.|
    |array\_agg|array\_agg\(x\) → array<\[same as input\]\>|Returns an array created from the input x elements.|
    |avg|avg\(x\) → double|Returns the average \(arithmetic mean\) of all input values.|
    |avg|avg\(time interval type\) → time interval type|Returns the average interval length of all input values.|
    |bool\_and|bool\_and\(boolean\) → boolean|Returns TRUE if every input value is TRUE, otherwise FALSE.|
    |bool\_or|bool\_or\(boolean\) → boolean|Returns TRUE if any input value is TRUE, otherwise FALSE.|
    |checksum|checksum\(x\) → varbinary|Returns an order-insensitive checksum of the given values.|
    |count|count\(\*\) → bigint|Returns the number of input rows.|
    |count|count\(x\) → bigint|Returns the number of non-null input values.|
    |count\_if|count\_if\(x\) → bigint|Returns the number of TRUE input values. This function is equivalent to `count(CASE WHEN x THEN 1 END)`.|
    |every|every\(boolean\) → boolean|This is an alias for **bool\_and**.|
    |geometric\_mean|geometric\_mean\(x\) → double|Returns the geometric mean of all input values.|
    |max\_by|max\_by\(x, y\) → \[same as x\]|Returns the value of x associated with the maximum value of y over all input values.|
    |max\_by|max\_by\(x, y, n\) → array<\[same as x\]\>|Returns n values of x associated with the n largest of all input values of y in descending order of y.|
    |min\_by|min\_by\(x, y\) → \[same as x\]|Returns the value of x associated with the minimum value of y over all input values.|
    |min\_by|min\_by\(x, y, n\) → array<\[same as x\]\>|Returns n values of x associated with the n smallest of all input values of y in ascending order of y.|
    |max|max\(x\) → \[same as input\]|Returns the maximum value of all input values.|
    |max|max\(x, n\) → array<\[same as x\]\>|Returns n largest values of all input values of x.|
    |min|min\(x\) → \[same as input\]|Returns the minimum value of all input values.|
    |min|min\(x, n\) → array<\[same as x\]\>|Returns n smallest values of all input values of x.|
    |sum|sum\(x\) → \[same as input\]|Returns the sum of all input values.|

-   **Bitwise aggregate functions**

    For bitwise aggregate functions, refer to`bitwise_and_agg` and `bitwise_or_agg` functions as described in [General aggregate functions](https://help.aliyun.com/document_detail/c3-2-2-6).

-   **Map aggregate functions**

    |Function|Syntax|Description|
    |:-------|:-----|:----------|
    |histogram|histogram\(x\) → map|Returns a map containing the count of the number of times each input value occurs.|
    |map\_agg|map\_agg\(key, value\) → map|Returns a MAP created from the input key/value pairs.|
    |map\_union|map\_union\(x\) → map|Returns the union of all the input maps. If a key is found in multiple input maps, that key’s value in the resulting map comes from an arbitrary input map.|
    |multimap\_agg|multimap\_agg\(key, value\) → map\>|Returns a multimap created from the input key/value pairs.|

-   **Close aggregate function**

    |Function|Syntax|Description|
    |:-------|:-----|:----------|
    |approx\_distinct|approx\_distinct\(x, \[e\]\) → bigint|Returns the approximate number of distinct input values. This function provides an approximation of `count(DISTINCT x)`. Zero is returned if all input values are null. This function should produce a standard error of no more than e, which is the standard deviation of the \(approximately normal\) error distribution over all possible sets. It is optional, and is 2.3% by default. The current implementation of this function requires that e be in the range of \[0.01150, 0.26000\]. It does not guarantee an upper bound on the error for any specific input set.|
    |approx\_percentile|approx\_percentile\(x, percentage\) → \[same as x\]|Returns the approximate percentile for all input values of x at the given percentage.|
    |approx\_percentile|approx\_percentile\(x, percentages\) → array<\[same as x\]\>|Similar to the preceding function, percentages is an array, and returns constant values for all input rows.|
    |approx\_percentile|approx\_percentile\(x, w, percentage\) → \[same as x\]|Similar to the preceding function, w is the weighted value of x.|
    |approx\_percentile|approx\_percentile\(x, w, percentage, accuracy\) → \[same as x\]|Similar to the preceding function, accuracy is the upper bound of the estimation accuracy, and the value must be in the range of \[0, 1\].|
    |approx\_percentile|approx\_percentile\(x, w, percentages\) → array<\[same as x\]\>|Similar to the preceding function, percentages is an array, and returns constant values for all input rows.|
    |numeric\_histogram|numeric\_histogram\(buckets, value, \[weight\]\) → map|Computes an approximate histogram with up to a given number of buckets. buckets must be a BIGINT. value and weight must be numeric. weight is optional, and is 1 by default.|

-   **Statistical aggregate functions**

    |Function|Syntax|Description|
    |:-------|:-----|:----------|
    |corr|corr\(y, x\) → double|Returns correlation coefficient of input values.|
    |covar\_pop|covar\_pop\(y, x\) → double|Returns the population covariance of input values.|
    |covar\_samp|covar\_samp\(y, x\) → double|Returns the sample covariance of input values.|
    |kurtosis|kurtosis\(x\) → double|Returns the excess kurtosis of all input values. Unbiased estimate using the following expression: kurtosis\(x\) = n\(n+1\)/\(\(n-1\)\(n-2\)\(n-3\)\)sum\[\(x\_i-mean\)^4\]/sttdev\(x\)^4-3\(n-1\)^2/\(\(n-2\)\(n-3\)\)|
    |regr\_intercept|regr\_intercept\(y, x\) → double|Returns linear regression intercept of input values. y is the dependent value. x is the independent value.|
    |regr\_slope|regr\_slope\(y, x\) → double|Returns linear regression slope of input values. y is the dependent value. x is the independent value.|
    |skewness|skewness\(x\) → double|Returns the skewness of all input values.|
    |sttdev\_pop|sttdev\_pop\(x\) → double|Returns the population standard deviation of all input values.|
    |sttdev\_samp|sttdev\_samp\(x\) → double|Returns the sample standard deviation of all input values.|
    |sttdev|sttdev\(x\) → double|This is an alias for `sttdev_samp`.|
    |var\_pop|var\_pop\(x\) → double|Returns the population variance of all input values.|
    |var\_samp|var\_samp\(x\) → double|Returns the sample variance of all input values.|
    |variance|variance\(x\) → double|This is an alias for `var_samp`.|


