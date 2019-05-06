# Data types {#concept_d5k_2b1_ygb .concept}

Presto supports multiple common data types by default, such as boolean, integer, floating point, string, and date. You can also add custom data types by using plugins. The custom Presto connectors are not required to support all data types.

## Value types {#section_dwt_fx1_ygb .section}

Presto has a set of built-in value types as follows:

-   `BOOLEAN`

    Represents an option with a value of `TRUE` or `FALSE`.

-   `TINYINT`

    An 8-bit signed [two's complement](https://en.wikipedia.org/wiki/Two%27s_complement)

-   `SMALLINT`

    A 16-bit signed [two's complement](https://en.wikipedia.org/wiki/Two%27s_complement)

-   `INTEGER`

    A 32-bit signed [two's complement](https://en.wikipedia.org/wiki/Two%27s_complement)

-   `BIGINT`

    A 64-bit signed [two's complement](https://en.wikipedia.org/wiki/Two%27s_complement)

-   `REAL`

    A 32-bit multi-precision implementation of the [IEEE Standard 754](https://en.wikipedia.org/wiki/IEEE_754) for binary floating-point arithmetic.

-   `DOUBLE`

    A 64-bit multi-precision implementation of the [IEEE Standard 754](https://en.wikipedia.org/wiki/IEEE_754) for binary floating-point arithmetic.

-   `DECIMAL`

    A fixed-precision decimal number. Precision up to 38 digits is supported, but performance is best with up to 17 digits. It takes two literal parameters to define the `DECIMAL` type:

    1.  Precision: the total number of digits, excluding symbols
    2.  Scale: the number of digits in the fractional part. It is optional and set to 0 by default.
    Example: `DECIMAL '-10.7'` can be expressed with the `DECIMAL(3,1)` type.


The following table lists the bits and value range of the integer type.

|Value type|Bit width|Minimum value|Maximum value|
|----------|---------|-------------|-------------|
|TINYINT|8-it|-2^7|2^7 - 1|
|SMALLINT|16-bit|2^15|2^15 - 1|
|INTEGER|32-bit|-2^31|-2^31 - 1|
|BIGINT|64-bit|-2^63|-2^63 - 1|

## String types {#section_q5q_gy1_ygb .section}

Presto supports the following built-in string types:

-   `VARCHAR`

    Variable-length string with an optional maximum length.

    Example: `VARCHAR`, `VARCHAR(10)`

-   `CHAR`

    Fixed-length string. A CHAR type without length specified has a default length of `1`.

    Example: `CHAR`，`CHAR(10)`

    **Note:** A string with the specified length always has the number of characters equal to this length. If the string length is smaller than the specified length, leading and trailing spaces are included in comparisons of the string value. As a result, two character values of different lengths can never be equal.

-   `VARBINARY` indicates variable-length binary data.


## Date and time {#section_u5q_gy1_ygb .section}

Presto supports the following built-in date and time types:

-   `DATE`

    Indicates a calendar date \(year, month, and day\) without time.

    Example: `DATE '1988-01-30'`

-   `TIME`

    Indicates time, including hours, minutes, seconds, and milliseconds. Values of this type can be rendered in the `time zone`.

    Examples:

    -   `TIME '18:01:02.345'` does not have a time zone definition and is parsed by using the system time zone.
    -   `TIME '18:01:02.345 Asia/Shanghai'` has a time zone definition and is parsed by using the defined time zone.
-   `TIMESTAMP`

    Indicates a point in time that includes the date and time of the day. The value range is from `'1970-01-01 00:00:01' UTC` to `'2038-01-19 03:14:07' UTC`, which can be rendered in the `time zone`.

    Examples: `TIMESTAMP '1988-01-30 01:02:03.321'` and `TIMESTAMP '1988-01-30 01:02:03.321 Asia/Shanghai'`

-   `INTERVAL`

    Mainly used in time calculation expressions to indicate a time span, in the following optional units:

    -   `YEAR`: year
    -   `QUARTER`: quarter
    -   `MONTH`: month
    -   `DAY`: day
    -   `HOUR`: hour
    -   `MINUTE`: minute
    -   `SECOND`: second
    -   `MILLISECOND`: millisecond
    Example: `DATE '2012-08-08' + INTERVAL '2' DAY`


## Complex types {#section_dvq_gy1_ygb .section}

Presto supports multiple complex built-in data types for more complex service scenarios. These data types include:

-   `JSON`

    JSON value type, which can be a JSON object, a JSON array, a JSON number, a JSON string, and the boolean type `true`, `false`, or `null`.

    Examples:

    -   `JSON '[1, null, 1988]'`
    -   `JSON '{"k1":1, "k2": "abc"}'`
-   `ARRAY`

    An array of the given component type. Types of elements in an array must be consistent.

    Example: `ARRAY[1, 2, 3]`

-   `MAP`

    Represents a mapping relationship that consists of a key array and a value array.

    Example: `MAP(ARRAY['foo', 'bar'], ARRAY[1, 2])`

-   `ROW`

    A structure consisting of named fields. The data columns can be accessed by using the field reference operator `.` and column names.

    Example: `CAST(ROW(1988, 1.0, 30) AS ROW(y BIGINT, m DOUBLE, d TINYINT ))`

-   `IPADDRESS`

    An IP address that can represent either an IPv4 or IPv6 address. Internally, IPv4 addresses are converted into IPv6 addresses based on the [IPv4-to-IPv6 mapping table](https://tools.ietf.org/html/rfc4291.html#section-2.5.5.2).

    Example: `IPADDRESS '0.0.0.0'`, `IPADDRESS '2001:db8::1'`


