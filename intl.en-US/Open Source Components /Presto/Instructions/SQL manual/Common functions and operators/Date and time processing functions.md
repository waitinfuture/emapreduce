# Date and time processing functions {#concept_p1v_vch_ygb .concept}

## Date and time operators {#section_t3j_xch_ygb .section}

Presto supports two date and time operators: `+` and `-`.

Examples:

```
--- +
date '2012-08-08' + interval '2' day	            --- 2012-08-10
time '01:00' + interval '3' hour	                --- 04:00:00.000
timestamp '2012-08-08 01:00' + interval '29' hour	--- 2012-08-09 06:00:00.000
timestamp '2012-10-31 01:00' + interval '1' month	--- 2012-11-30 01:00:00.000
interval '2' day + interval '3' hour	            --- 2 03:00:00.000
interval '3' year + interval '5' month	            --- 3-5
--- -
date '2012-08-08' - interval '2' day	            --- 2012-08-06
time '01:00' - interval '3' hour	                --- 22:00:00.000
timestamp '2012-08-08 01:00' - interval '29' hour	--- 2012-08-06 20:00:00.000
timestamp '2012-10-31 01:00' - interval '1' month	--- 2012-09-30 01:00:00.000
interval '2' day - interval '3' hour	            --- 1 21:00:00.000
interval '3' year - interval '5'                   --- month	2-7
```

## Time zone conversion {#section_pfl_h2h_ygb .section}

The `AT TIME ZONE` operator sets the time zone of a timestamp.

Examples:

```
SELECT timestamp '2012-10-31 01:00 UTC'; 
--- 2012-10-31 01:00:00.000 UTC
SELECT timestamp '2012-10-31 01:00 UTC' AT TIME ZONE 'America/Los_Angeles'; 
--- 2012-10-30 18:00:00.000 America/Los_Angeles
```

## Date and time functions {#section_vjv_lfh_ygb .section}

-   Basic functions

    |Function|Syntax|Description|
    |--------|------|-----------|
    |current\_date|current\_date -\> date|Returns the current date as of the start of the query.|
    |current\_time|current\_time -\> time with time zone|Returns the current time as of the start of the query.|
    |current\_timestamp|current\_timestamp -\> timestamp with time zone|Returns the current timestamp as of the start of the query.|
    |current\_timezone|current\_timezone\(\) → varchar|Returns the current time zone.|
    |date|date\(x\) → date|Parses the literal value of a date into a date.|
    |from\_iso8601\_timestamp|from\_iso8601\_timestamp\(string\) → timestamp with time zone|Parses the literal value of an ISO 8601-formatted timestamp into a timestamp variable with a time zone.|
    |from\_iso8601\_date|from\_iso8601\_date\(string\) → date|Parses the literal value of an ISO 8601-formatted date into a variable of the date type.|
    |from\_unixtime|from\_unixtime\(unixtime, \[timezone\_str\]\) → timestamp|Parses a UNIX timestamp into a timestamp variable. A time zone can be included.|
    |from\_unixtime|from\_unixtime\(unixtime, hours, minutes\) → timestamp with time zone|Parses a UNIX timestamp into a timestamp variable with a time zone, with `hours` and `minutes` indicating the time zone offset.|
    |localtime|localtime -\> time|Returns the current time as of the start of the query.|
    |localtimestamp|localtimestamp -\> timestamp|Returns the current timestamp as of the start of the query.|
    |now|now\(\) → timestamp with time zone|Returns the current time. This is an alias for `current_time`.|
    |to\_iso8601|to\_iso8601\(x\) → varchar|Formats `x` as an ISO 8601 string. `x` can be `DATE` or `TIMESTAMP [with time zone]`.|
    |to\_milliseconds|to\_milliseconds\(interval\) → bigint|Returns the number of milliseconds that have elapsed since 00:00 of the current day.|
    |to\_unixtime|to\_unixtime\(timestamp\) → double|Returns a timestamp in the UNIX format.|

    **Note:** 

    The following SQL standard functions do not use parenthesis:

    -   `current_data`
    -   `current_time`
    -   `current_timestamp`
    -   `localtime`
    -   `localtimestamp`
-   Truncation function

    The truncation function truncates date and time values by the specified unit, and returns the date and time values of this unit. The usage is as follows:

    `date_trunc(unit, x) -> [same as x]`

    `unit` is one of:

    -   `second`: seconds
    -   `minute`: minutes
    -   `hour`: hours
    -   `day`: days
    -   `week`: weeks
    -   `month`: months
    -   `quarter`: quarters
    -   `year`: years
-   Interval functions

    Presto provides two functions for interval calculation, which are:

    -   `date_add(unit, value, timestamp) → [same as input]`
    Adds an interval value of the type unit to a timestamp. Subtraction can be performed by using a negative value with a unit.

    -   `date_diff(unit, timestamp1, timestamp2) → bigint`
    Returns the interval between two timestamps expressed with a unit.

    `unit` is one of the following:

    -   `ns`: nanoseconds
    -   `us`: microseconds
    -   `ms`: milliseconds
    -   `s`: seconds
    -   `m`: minutes
    -   `h`: hours
    -   `d`: days
-   Date and time extraction functions

    Presto provides the `extract` function to extract the specified fields from a date and time value, which is:

    `extract(field FROM x) → bigint`

    `x` is the date and time value, and `field` is the field to be extracted, which can be one of the following values:

    -   `YEAR`: year
    -   `QUARTER`: quarter
    -   `MONTH`: month
    -   `WEEK`: week
    -   `DAY`: day
    -   `DAY_OF_MONTH`: day of a month
    -   `DAY_OF_WEEK`: day of a week
    -   `DOW`: an alias for `DAY_OF_WEEK`
    -   `DAY_OF_YEAR`: day of a year
    -   `DOY`: an alias for `DAY_OF_YEAR`
    -   `YEAR_OF_WEEK`: year of an [ISO week](https://en.wikipedia.org/wiki/ISO_week_date)
    -   `YOW`: an alias for `YEAR_OF_WEEK`
    -   `HOUR`: hour
    -   `MINUTE`: minute
    -   `SECOND`: second
    -   `TIMEZONE_HOUR`: hour with a time zone
    -   `TIMEZONE_MINUTE`: minute with a time zone
    For convenience, Presto provides the following helper functions:

    |Function|Syntax|Description|
    |--------|------|-----------|
    |day|day\(x\) → bigint|Returns the day of the month from x.|
    |day\_of\_month|day\_of\_month\(x\) → bigint|This is an alias for `day`.|
    |day\_of\_week|day\_of\_week\(x\) → bigint|Returns the day of the week from x.|
    |day\_of\_year|day\_of\_year\(x\) → bigint|Returns the day of the year from x.|
    |dow|dow\(x\) → bigint|This is an alias for `day_of_week`.|
    |doy|doy\(x\) → bigint|This is an alias for `day_of_year`.|
    |hour|hour\(x\) → bigint|Returns the hour of the day from x. The value ranges from 0 to 23.|
    |minute|minute\(x\) → bigint|Returns the minute from x. The value ranges from 0 to 59.|
    |month|month\(x\) → bigint|Returns the month of the year from x. The value ranges from 1 to 12.|
    |quarter|quarter\(x\) → bigint|Returns the quarter of the year from x.|
    |second|second\(x\) → bigint|Returns the number of seconds from x. The value ranges from 0 to 59.|
    |timezone\_hour|timezone\_hour\(timestamp\) → bigint|Returns the number of hours of the time zone offset from the timestamp.|
    |timezone\_minute|timezone\_minute\(timestamp\) → bigint|Returns the number of minutes of the time zone offset from the timestamp.|
    |week|week\(x\) → bigint|Returns the week of the year from x. The value ranges from 1 to 53.|
    |week\_of\_year|week\_of\_year\(x\) → bigint|This is an alias for `week`.|
    |year|year\(x\) → bigint|Returns the year from x.|
    |year\_of\_week|year\_of\_week\(x\) → bigint|Returns the year of the week from `x` \([ISO week](https://en.wikipedia.org/wiki/ISO_week_date)\).|
    |yow|yow\(x\) → bigint|This is an alias for `year_of_week`.|

-   MySQL date functions

    Presto provides two date parsing functions that are compatible with MySQL `date_parse` and `str_to_date`.

    -   `date_format(timestamp, format) → varchar`

        Formats `timestamp` as a string by using `format`.

    -   `date_parse(string, format) → timestamp`

        Parses the literal value of a date by using `format`.

    The following table lists the MySQL format specifiers supported by Presto.

    |Specifier|Description|
    |---------|-----------|
    |%a|Abbreviated weekday name \(Sun .. Sat\)|
    |%b|Abbreviated month name \(Jan .. Dec\)|
    |%c|Month, numeric \(1 .. 12\), which cannot be 0|
    |%d|Day of the month, numeric \(01 .. 31\), which cannot be 0|
    |%e|Day of the month, numeric \(1 .. 31\), which cannot be 0|
    |%f|Number of seconds \(6 digits for printing: 000000 .. 999000; 1 - 9 digits for parsing: 0 .. 999999999\)|
    |%H|Hours \(00 .. 23\)|
    |%h|Hours \(01 .. 12\)|
    |%I|Hours \(01 .. 12\)|
    |%i|Minutes \(00 .. 59\)|
    |%j|Day of the year \(001 .. 366\)|
    |%k|Hours \(0 .. 23\)|
    |%l|Hours \(1 .. 12\)|
    |%M|Month \(January .. December\)|
    |%m|Month, numeric \(01 .. 12\) \[4\]|
    |%p|AM ／ PM|
    |%r|Time, 12-hour \(hh:mm:ss AM/PM\)|
    |%S|Seconds \(00 .. 59\)|
    |%s|Seconds \(00 .. 59\)|
    |%T|Time, 24-hour \(hh:mm:ss\)|
    |%v|Week \(01 .. 53\), where Monday is the first day of the week, used with `%X`|
    |%W|Weekday name \(Sunday .. Saturday\)|
    |%x|Year for the week, where Monday is the first day of the week, numeric, four digits|
    |%Y|Year, numeric, four digits|
    |%y|Year, numeric \(two digits\). During parsing, the two-digit year format assumes the range \[1970, 2069\].|
    |%%|A literal `'%'` character|

    **Note:** Currently, Presto does not support the following specifiers: `%D`, `%U`, `%u`, `%V`, `%w`, and `%X`.

-   Java date functions

    The following functions use a format string compatible with [JodaTime Pattern](http://joda-time.sourceforge.net/apidocs/org/joda/time/format/DateTimeFormat.html) of Java.

    -   `format_datetime(timestamp, format) → varchar`: formats a timestamp.
    -   `parse_datetime(string, format) → timestamp with time zone`: parses a string into a timestamp.

