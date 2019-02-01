# Data types {#concept_shf_y32_z2b .concept}

Presto supports multiple common data types by default, such as Boolean, Integer, Floating-Point, String, and Date and Time. You can also add customized data types using plug-ins. Customized Presto connectors are not required to support all data types.

## Data types {#section_ahz_z32_z2b .section}

Presto's set of built-in data types are as follows:

-   Boolean

    Represents two option with a value of true or false.

-   Tinyint

    A two's complement 8-bit signed integer.

-   Smallint

    A two's complement 16-bit signed integer.

-   Integer

    A two's complement 32-bit signed integer.

-   Bigint

    A two's complement 64-bit signed integer.

-   Real

    A 32-bit inexact, variable-precision floating point which implements [IEEE Standard 754](https://en.wikipedia.org/wiki/IEEE_754) for Floating-Point Arithmetic.

-   Double

    A 64-bit multi-precision floating point which implements [IEEE Standard 754](https://en.wikipedia.org/wiki/IEEE_754) for Floating-Point Arithmetic.

-   Decimal

    A fixed precision decimal number. Precision up to 38 digits is supported but performance is best up to 17 digits. It takes two literal parameters to define the DECIMAL type :A fixed-precision decimal number. Precision up to 38 digits is supported but performance is optimal up to 18 digits. The following two literal parameters are required to define decimal:

    -   precision: The total number of digits, excluding symbols.
    -   scale: The number of digits in a fractional part. Scale is optional and defaults to 0.

        For example, `DECIMAL '-10.7'` can be expressed with the `DECIMAL(3,1)` type.

    The following table describes the bits and value range of the integer type.

    |Value type|Bits|Minimum value|Maximum value|
    |:---------|:---|:------------|:------------|
    |TINYINT|8 bit|-2^7|2^7 - 1|
    |SMALLINT|16 bit|2^15|2^15 - 1|
    |INTEGER|32 bit|-2^31|-2^31 - 1|
    |BIGINT|64 bit|-2^63|-2^63 - 1|


## String type {#section_pcg_1l2_z2b .section}

Presto supports the following built-in string types:

-   Varchar

    Variable length character data with an optional maximum length.

    For example, `VARCHAR` or `VARCHAR(10)`.

-   Char

    Fixed length character data. If no length is specified, the default length is 1.

    For example, `CHAR` or `CHAR(10)` .

    **Note:** A string with a specified length always has the number of characters equal to this length. Where the string length is smaller than the specified length, leading and trailing spaces are included in comparisons of the string value. As a result, two character values of different lengths can never be equal.

-   Varbinary

    Indicates variable length binary data.


## Date and time {#section_nbf_4l2_z2b .section}

Presto supports the following built-in date and time types:

-   Date

    Refers to a calendar date \(year, month, day\) without a time.

    For example, `DATE '1988-01-30'`.

-   Time

    Refers to a time, including the hour, minute, second, and millisecond. This value can be modified in the time zone.

    For example:

    -   `TIME '18:01:02.345'` does not have a time zone definition and is therefore parsed using the system time zone.
    -   `TIME '18:01:02.345 Asia/Shanghai'` has a time zone definition and is therefore parsed using the defined time zone.
-   Timestamp

    Refers to a point in time that includes the date and time of day. The value range is from `'1970-01-01 00:00:01' UTC` to `'2038-01-19 03:14:07' UTC`, which can be modified in the time zone.

    For example, `TIMESTAMP '1988-01-30 01:02:03.321'` or `TIMESTAMP '1988-01-30 01:02:03.321 Asia/Shanghai'`.

-   Interval

    Mainly used in time calculated expressions to refer to a time span, the unit of which can be:

    -   Year
    -   Quarter \(of the year\)
    -   Month
    -   Day
    -   Hour
    -   Minute
    -   Second
    -   Millisecond

        For example, `DATE '2012-08-08' + INTERVAL '2' DAY`.


## Complex types {#section_wmr_2m2_z2b .section}

Presto supports multiple complex built-in data types to support more complex business scenarios. These data types include the following:

-   JSON

    This can be a JSON object, array, number, or string, as well as the Boolean type in the TRUE, FALSE or NULL state.

    For example:

    -   `JSON '[1, null, 1988]'`
    -   `JSON '{"K1": 1, "K2": "ABC "}'`
-   Array

    An array of the given component type. The types of elements in an array must be consistent.

    For example, `ARRAY[1, 2, 3]`.

-   Map

    Represents a mapping relationship that consists of a key array and a value array.

    For example, `MAP(ARRAY['foo', 'bar'], ARRAY[1, 2])`

-   Row

    A structure made up of named fields. The fields may be accessed with field reference operator `.` and the field names. You can use the operator and the method of column names to access data columns.

    For example, `CAST(ROW(1988, 1.0, 30) AS ROW(y BIGINT, m DOUBLE, d TINYINT ))`.

-   IP address

    An IP address that can represent either an IPv4 or IPv6 address. Internally, the type is a IPv6 address. Support for IPv4 is handled using the [IPv4-mapped IPv6 address range](https://tools.ietf.org/html/rfc4291.html#section-2.5.5.2).

    For example, `IPADDRESS '0.0.0.0'` or `IPADDRESS '2001:db8::1'`.


