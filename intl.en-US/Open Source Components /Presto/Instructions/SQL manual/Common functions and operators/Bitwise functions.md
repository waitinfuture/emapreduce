# Bitwise functions {#concept_c5n_qmb_ygb .concept}

Presto provides the following bitwise functions:

|Function|Syntax|Description|
|--------|------|-----------|
|bit\_count|bit\_count\(x, bits\) → bigint|Returns the number of bits set in x at position 1 in two's complement representation.|
|bitwise\_and|bitwise\_and\(x, y\) → bigint|Bitwise AND function|
|bitwise\_not|bitwise\_not\(x\) → bigint|Bitwise NOT function|
|bitwise\_or|bitwise\_or\(x, y\) → bigint|Bitwise OR function|
|bitwise\_xor|bitwise\_xor\(x, y\) → bigint|Bitwise XOR function|
|bitwise\_and\_agg|bitwise\_and\_agg\(x\) → bigint|Returns the bitwise AND of all input values in x, which is an array.|
|bitwise\_or\_agg|bitwise\_or\_agg\(x\) → bigint|Returns the bitwise OR of all input values in x, which is an array.|

Examples:

```
SELECT bit_count(9, 64); -- 2
SELECT bit_count(9, 8); -- 2
SELECT bit_count(-7, 64); -- 62
SELECT bit_count(-7, 8); -- 6
```

