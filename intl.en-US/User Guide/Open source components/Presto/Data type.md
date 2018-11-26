# Data type {#concept_shf_y32_z2b .concept}

Presto supports multiple common data types by default, such as Boolean, Integer, Floating-Point, String, and Date and Time. You can also add customized data types using plugins. Additionally, the customized Presto connectors are not required to support all data types.

## Data types {#section_ahz_z32_z2b .section}

Presto has a set of built-in data types that are as follows:

-   BOOLEAN

    Represents two option with a value of true or false.

-   TINYINT

    An 8-bit signed [two’s complement integer](https://en.wikipedia.org/wiki/Two%27s_complement).

-   SMALLINT

    A 16-bit signed [two’s complement integer](https://en.wikipedia.org/wiki/Two%27s_complement).

-   INTEGER

    A 32-bit signed [two’s complement integer](https://en.wikipedia.org/wiki/Two%27s_complement).

-   BIGINT

    A 64-bit signed [two’s complement integer](https://en.wikipedia.org/wiki/Two%27s_complement).

-   REAL

    A real is a 32-bit inexact, variable-precision implementing the [IEEE Standard 754](https://en.wikipedia.org/wiki/IEEE_754) for Binary Floating-Point Arithmetic.

-   DOUBLE

    A 64-bit multi-precision [\[IEEE-754\]](https://en.wikipedia.org/wiki/IEEE_754) binary floating-point numeric implementation.

-   DECIMAL A fixed precision decimal number. Precision up to 38 digits is supported but performance is best up to 17 digits. It takes two literal parameters to define the DECIMAL type :

-   precision: total number of digits, excluding symbols
-   scale: number of digits in fractional part. Scale is optional and defaults to 0.

    Example: `DECIMAL '-10.7'` can be expressed with `DECIMAL(3,1)` type.

    The following table describes the bits and value range of the integer type

    |Value Type|Bits|Minimum Value|Maximum Value|
    |:---------|:---|:------------|:------------|
    |TINYINT|8 bit|-2^7|2^7 - 1|
    |SMALLINT|16 bit|2^15|2^15 - 1|
    |INTEGER|32 bit|-2^31|-2^31 - 1|
    |BIGINT|64 bit|-2^63|-2^63 - 1|


## String type {#section_pcg_1l2_z2b .section}

Presto supports the following built-in string types:

-   VARCHAR

    Variable length character data with an optional maximum length.

    Example: `VARCHAR`, and `VARCHAR(10)`

-   CHAR

    Fixed length character data. A CHAR type without length specified has a default length of 1.

    Example: `CHAR`, and `CHAR(10)` 

    **Note:** A string with the specified length always has the number of characters equal to this length. Where the string length is smaller than the specified length, leading and trailing spaces are included in comparisons of the string value. As a result, two character values of different lengths can never be equal.

-   VARBINARY

    indicates variable length binary data.


## Date and time {#section_nbf_4l2_z2b .section}

Presto supports the following built-in date and time types:

-   DATE

    Refers to a calendar date \(year, month, day\) without time.

    Example: `DATE '1988-01-30'`

-   TIME

    Refers to a time, including hour, minute, second, and millisecond. Values of this type can be rendered in the time zone.

    Example:

    -   `TIME '18:01:02.345'`, does not have a time zone definition, and is thus parsed using the system time zone.
    -   `TIME '18:01:02.345 Asia/Shanghai'`, has time zone definition, and is thus parsed using the defined time zone.
-   TIMESTAMP

    Refers to an instant in time that includes the date and time of day. The value range is from `'1970-01-01 00:00:01' UTC` to `'2038-01-19 03:14:07' UTC`, which can be rendered in the time zone.

    Example:`TIMESTAMP '1988-01-30 01:02:03.321'`，`TIMESTAMP '1988-01-30 01:02:03.321 Asia/Shanghai'`

-   INTERVAL

    Mainly used in time calculated expressions to refer to a time span, the unit of which can be:

    -   YEAR - Year
    -   QUARTER - Quarter of a year
    -   MONTH - Month
    -   DAY - Day
    -   HOUR - Hour
    -   MINUTE - Minute
    -   SECOND - Second
    -   MILLISECOND - Millisecond

        Example: `DATE '2012-08-08' + INTERVAL '2' DAY`


## Complex types {#section_wmr_2m2_z2b .section}

Presto supports multiple complex built-in data types, to support more complex business scenarios, and these data types include:

-   JSON

    JSON value type, which can be a JSON object, a JSON array, a JSON number, a JSON string, as well as the boolean type true, false or null.

    Example:

    -   `JSON '[1, null, 1988]'`
    -   `JSON '{"K1": 1, "K2": "ABC "}'`
-   ARRAY

    An array of the given component type. Types of elements in an array must be consistent.

    Example: `ARRAY[1, 2, 3]`

-   MAP

    Represents a mapping relationship consisting of a key array and a value array.

    Example: `MAP(ARRAY['foo', 'bar'], ARRAY[1, 2])`

-   ROW

    A structure made up of named fields. The fields may be accessed with field reference operator `.` and the field names. Operator + the method of column names to access data columns.

    Example: `CAST(ROW(1988, 1.0, 30) AS ROW(y BIGINT, m DOUBLE, d TINYINT ))`

-   IPADDRESS

    An IP address that can represent either an IPv4 or IPv6 address. An IP address that can represent either an IPv4 or IPv6 address. Internally, the type is a pure IPv6 address. Support for IPv4 is handled using the [IPv4-mapped IPv6 address range](https://tools.ietf.org/html/rfc4291.html#section-2.5.5.2).

    Example:`IPADDRESS '0.0.0.0'`，`IPADDRESS '2001:db8::1'`


