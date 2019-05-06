# UNNEST {#concept_uyc_54l_zgb .concept}

`UNNEST` expands variables of the `ARRAY` and `MAP` types into a table. Variables of the `ARRAY` type are expanded into a single-column table, and variables of the `MAP` type are expanded into a two-column \(key, value\) table. `UNNEST` can expand multiple variables of the `ARRAY` and `MAP` types into columns at a time, with as many rows as the maximum number of expanded rows of the input parameter list \(the other columns are padded with null values\). `UNNEST` may have a `WITH ORDINALITY` clause, in which case an additional ordinal column is appended to the query results. `UNNEST` is typically used with a `JOIN` and can reference columns from relations on the left side of the JOIN.

Example 1:

```
--- Use a single column
SELECT student, score
FROM tests
CROSS JOIN UNNEST(scores) AS t (score);

```

Example 2:

```
--- Use multiple columns
SELECT numbers, animals, n, a
FROM (
  VALUES
    (ARRAY[2, 5], ARRAY['dog', 'cat', 'bird']),
    (ARRAY[7, 8, 9], ARRAY['cow', 'pig'])
) AS x (numbers, animals)
CROSS JOIN UNNEST(numbers, animals) AS t (n, a);
  
  numbers  |     animals      |  n   |  a
-----------+------------------+------+------
 [2, 5]    | [dog, cat, bird] |    2 | dog
 [2, 5]    | [dog, cat, bird] |    5 | cat 
 [2, 5]    | [dog, cat, bird] | NULL | bird
 [7, 8, 9] | [cow, pig]       |    7 | cow
 [7, 8, 9] | [cow, pig]       |    8 | pig
 [7, 8, 9] | [cow, pig]       |    9 | NULL
(6 rows)

```

Example 3:

```
--- Use a WITH ORDINALITY clause
SELECT numbers, n, a
FROM (
  VALUES
    (ARRAY[2, 5]),
    (ARRAY[7, 8, 9])
) AS x (numbers)
CROSS JOIN UNNEST(numbers) WITH ORDINALITY AS t (n, a);

  numbers  | n | a 
-----------+---+---
 [2, 5]    | 2 | 1 
 [2, 5]    | 5 | 2
 [7, 8, 9] | 7 | 1
 [7, 8, 9] | 8 | 2
 [7, 8, 9] | 9 | 3
(5 rows)
```

