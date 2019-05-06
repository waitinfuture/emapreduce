# Regular expression {#concept_iz4_lzg_ygb .concept}

Presto supports all the regular expression functions that use the [Java Pattern](http://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html) syntax, but there are a few exceptions:

-   Multi-line mode

    -   `? m` is used to enable the multi-line mode.
    -   The line terminator is`\n`.
    -   The `? d` flag is not supported.
-   Case-sensitive mode

    -   `? i` is used to enable the case-sensitive mode.
    -   The `? u` flag is not supported.
    -   Context-sensitive matching is not supported.
    -   Local-sensitive matching is not supported.
-   Surrogate pairs are not supported.

    -   For example, `U+10000` must be expressed by `\x{10000}`, not `\uD800\uDC00`.
-   Boundaries `\b` are incorrectly processed for a non-spacing mark without a base character.

-   `\Q` and `\E` are not supported in character classes \(such as `[A-Z123]`\).

-   Unicode character classes \(`\p{prop}`\) are supported with the following differences:

    -   All underscores in names must be removed. For example, use `OldItalic` instead of `Old_Italic`.
    -   Scripts must be specified directly, without the `Is`, `script=`, or `sc=` prefix. Example: `\p{Hiragana}` instead of `\p{script=Hiragana}`.
    -   Blocks must be specified with the `In` prefix. The `block=` and `blk=` prefixes are not supported. Example: `\p{InMongolia}`.
    -   Categories must be specified directly, without the `Is`, `general_category=`, or `gc=` prefix. Example: `\p{L}`.
    -   Binary properties must be specified directly. Example: use `\p{NoncharacterCodePoint}` instead of `\p{IsNoncharacterCodePoint}`.

Presto provides the following regular expression functions:

-   `regexp_extract_all(string, pattern, [group]) → array<varchar>`

    Returns the substrings matched by the regular expression `pattern` in `string`. If the `pattern` expression uses the grouping function, then the `group` parameter can be set to specify the [capturing group](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html#gnumber%C3%9F) to be matched by the regular expression.

    Examples

    ```
    SELECT regexp_extract_all('1a 2b 14m', '\d+'); -- [1, 2, 14]
    SELECT regexp_extract_all('1a 2b 14m', '(\d+)([a-z]+)', 2); -- ['a', 'b', 'm']
    
    ```

-   `regexp_extract(string, pattern, [group]) → varchar`

    The function and usage are similar to those of `regexp_extract_all`. The difference is that this function only returns the first substring that is matched by the regular expression.

    Examples

    ```
    SELECT regexp_extract('1a 2b 14m', '\d+'); -- 1
    SELECT regexp_extract('1a 2b 14m', '(\d+)([a-z]+)', 2); -- 'a'
    
    ```

-   `regexp_like(string, pattern) → boolean`

    Evaluates the regular expression `pattern` and determines whether it is contained in `string`. If yes, `TRUE` is returned. If not, `FALSE` is returned. This function is similar to the `LIKE` operator of SQL, except that `LIKE` matches the entire string, whereas this function returns `TRUE` when the string contains the substring matched with pattern.

    Examples

    ```
    SELECT regexp_like('1a 2b 14m', '\d+b'); -- true
    
    ```

-   `regexp_replace(string, pattern, [replacement]) → varchar`

    Replaces every instance of the substring that is matched by the regular expression `pattern` in `string` with `replacement`. `replacement` is optional and replaced by `"` \(deleting the matched substrings\) if it is not specified.

    Capturing groups can be referenced in `replacement` by using `$g` \(g is the group SN, starting from 1\) for a numbered group or `${name}` for a named group. A dollar sign `$` can be included in the `replacement` by escaping it with a backslash `\$`.

    Examples

    ```
    SELECT regexp_replace('1a 2b 14m', '\d+[ab] '); -- '14m'
    SELECT regexp_replace('1a 2b 14m', '(\d+)([ab]) ', '3c$2 '); -- '3ca 3cb 14m'
    
    ```

-   `regexp_split(string, pattern) → array<varchar>`

    Splits a string by using the regular expression `pattern` and returns an array. Trailing empty strings are preserved.

    Examples

    ```
    SELECT regexp_split('1a 2b 14m', '\s*[a-z]+\s*'); -- ['1', '2', '14', ''] four elements
                                                      -- The last one is an empty character.
    ```


