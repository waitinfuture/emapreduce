# String functions {#concept_afx_kxg_ygb .concept}

## Concatenation operator {#section_qhz_txg_ygb .section}

The `||` operator performs string concatenation.

## String functions {#section_rhz_txg_ygb .section}

The following table lists the string functions supported by Presto.

|Function|Syntax|Description|
|--------|------|-----------|
|chr|chr\(n\) → varchar|Returns the *Unicode code point* \(UCP\) `n` as a single character.|
|codepoint|codepoint\(string\) → integer|Returns the UCP value of `string`.|
|concat|concat\(string1, …, stringN\) → varchar|Returns the concatenation of string1, string2, …, stringN. This function provides the same functionality as the `||` operator.|
|hamming\_distance|hamming\_distance\(string1, string2\) → bigint|Returns the [Hamming distance](https://en.wikipedia.org/wiki/Hamming_distance) of string1 and string2. Note that the two strings must have the same length.|
|length|length\(string\) → bigint|Returns the length of a string.|
|levenshtein\_distance|levenshtein\_distance\(string1, string2\) → bigint|Returns the [Levenshtein distance](https://en.wikipedia.org/wiki/Levenshtein_distance) of string1 and string2.|
|lower|lower\(string\) → varchar|Converts a string into lowercase.|
|upper|upper\(string\) → varchar|Converts a string into uppercase.|
|replace|replace\(string, search\) → varchar|Removes all instances of `search` from `string` and fill in with empty characters.|
|replace|replace\(string, search, replace\) → varchar|Replaces all instances of `search` with `replace` in `string`.|
|reverse|reverse\(string\) → varchar|Returns a string with the characters in reverse order.|
|lpad|lpad\(string, size, padstring\) → varchar|Left pads `string` to `size` characters with `padstring`. `padstring` must not be empty, and `size` must not be 0.|
|rpad|rpad\(string, size, padstring\) → varchar|Right pads `string` to `size` characters with `padstring`. `padstring` must not be empty, and `size` must not be 0.|
|ltrim|ltrim\(string\) → varchar|Removes leading whitespace from string.|
|rtrim|rtrim\(string\) → varchar|Removes trailing whitespace from string.|
|split|split\(string, delimiter\) → array|Splits string on delimiter and returns an array.|
|split|split\(string, delimiter, limit\) → array|Splits string on delimiter and returns an array of a limited size.|
|split\_part|split\_part\(string, delimiter, index\) → varchar|Splits string on delimiter and returns the field `index`. Field indexes start from 1.|
|split\_to\_map|split\_to\_map\(string, entryDelimiter, keyValueDelimiter\) → map<varchar, varchar\>|Splits string by entryDelimiter and keyValueDelimiter and returns a map.|
|strpos|strpos\(string, substring\) → bigint|Returns the starting position of the first instance of substring in string. Positions start from 1. If substring is not found, 0 is returned.|
|position|position\(substring IN string\) → bigint|Returns the starting position of the first instance of substring in string.|
|substr|substr\(string, start, \[length\]\) → varchar|Returns a substring from string of length from the starting position `start`. Positions start from 1. The length parameter is optional.|

## Unicode functions { .section}

-   `normalize(string) → varchar`

    Transforms string with the [NFC Normalization Form](https://en.wikipedia.org/wiki/Unicode_equivalence#Normalization).

-   `normalize(string, form) → varchar`

    Transforms string with the specified normalization form. `form` must be one of the following keywords:

    -   `NFD` Canonical Decomposition
    -   `NFC` Canonical Decomposition, followed by Canonical Composition
    -   `NFKD` Compatibility Decomposition
    -   `NFKC` Compatibility Decomposition, followed by Canonical Composition
-   `to_utf8(string) → varbinary`

    Encodes string into a UTF-8 varbinary representation.

-   `from_utf8(binary, [replace]) → varchar`

    Decodes binary data into a UTF-8 varbinary representation. Invalid sequences are replaced with `replace`, which is Unicode replacement character `U+FFFD` by default. This parameter is optional. Note that the replacement string `replace` must either be a single character or empty.


