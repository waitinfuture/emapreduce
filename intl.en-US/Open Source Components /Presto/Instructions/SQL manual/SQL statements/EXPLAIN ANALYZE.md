# EXPLAIN ANALYZE {#concept_pz4_q1k_zgb .concept}

## Overview {#section_p5k_ll3_ygb .section}

```
EXPLAIN ANALYZE [VERBOSE] statement
```

## Description {#section_fym_ml3_ygb .section}

Executes the statement and shows the distributed execution plan of the statement along with the cost of each operation. The `VERBOSE` option gives more details and underlying statistics.

## Examples {#section_phk_nl3_ygb .section}

In the following example, you can view the CPU time spent in each stage and the relative cost of each plan node in the stage. Note that the relative costs of the plan nodes are calculated based on the actual time and may not be correlated to the CPU time. You can also view additional statistics on each plan node, which are useful if you want to detect data errors during a query \(such as skewness and hash collisions\).

```
presto:sf1> EXPLAIN ANALYZE SELECT count(*), clerk FROM orders WHERE orderdate > date '1995-01-01' GROUP BY clerk;

                                          Query Plan
-----------------------------------------------------------------------------------------------
Fragment 1 [HASH]
    Cost: CPU 88.57ms, Input: 4000 rows (148.44kB), Output: 1000 rows (28.32kB)
    Output layout: [count, clerk]
    Output partitioning: SINGLE []
    - Project[] => [count:bigint, clerk:varchar(15)]
            Cost: 26.24%, Input: 1000 rows (37.11kB), Output: 1000 rows (28.32kB), Filtered: 0.00%
            Input avg.: 62.50 lines, Input std.dev.: 14.77%
        - Aggregate(FINAL)[clerk][$hashvalue] => [clerk:varchar(15), $hashvalue:bigint, count:bigint]
                Cost: 16.83%, Output: 1000 rows (37.11kB)
                Input avg.: 250.00 lines, Input std.dev.: 14.77%
                count := "count"("count_8")
            - LocalExchange[HASH][$hashvalue] ("clerk") => clerk:varchar(15), count_8:bigint, $hashvalue:bigint
                    Cost: 47.28%, Output: 4000 rows (148.44kB)
                    Input avg.: 4000.00 lines, Input std.dev.: 0.00%
                - RemoteSource[2] => [clerk:varchar(15), count_8:bigint, $hashvalue_9:bigint]
                        Cost: 9.65%, Output: 4000 rows (148.44kB)
                        Input avg.: 4000.00 lines, Input std.dev.: 0.00%

Fragment 2 [tpch:orders:1500000]
    Cost: CPU 14.00s, Input: 818058 rows (22.62MB), Output: 4000 rows (148.44kB)
    Output layout: [clerk, count_8, $hashvalue_10]
    Output partitioning: HASH [clerk][$hashvalue_10]
    - Aggregate(PARTIAL)[clerk][$hashvalue_10] => [clerk:varchar(15), $hashvalue_10:bigint, count_8:bigint]
            Cost: 4.47%, Output: 4000 rows (148.44kB)
            Input avg.: 204514.50 lines, Input std.dev.: 0.05%
            Collisions avg.: 5701.28 (17569.93% est.), Collisions std.dev.: 1.12%
            count_8 := "count"(*)
        - ScanFilterProject[table = tpch:tpch:orders:sf1.0, originalConstraint = ("orderdate" > "$literal$date"(BIGINT '9131')), filterPredicate = ("orderdate" > "$literal$date"(BIGINT '9131'))] => [cler 
                Cost: 95.53%, Input: 1500000 rows (0B), Output: 818058 rows (22.62MB), Filtered: 45.46%
                Input avg.: 375000.00 lines, Input std.dev.: 0.00%
                $hashvalue_10 := "combine_hash"(BIGINT '0', COALESCE("$operator$hash_code"("clerk"), 0))
                orderdate := tpch:orderdate
                clerk := tpch:clerk
```

When the `VERBOSE` option is used, some operators may report additional information.

```
EXPLAIN ANALYZE VERBOSE SELECT count(clerk) OVER() FROM orders WHERE orderdate > date '1995-01-01';

                                          Query Plan
-----------------------------------------------------------------------------------------------
  ...
         - Window[] => [clerk:varchar(15), count:bigint]
                 Cost: {rows: ?, bytes: ?}
                 CPU fraction: 75.93%, Output: 8130 rows (230.24kB)
                 Input avg.: 8130.00 lines, Input std.dev.: 0.00%
                 Active Drivers: [ 1 / 1 ]
                 Index size: std.dev.: 0.00 bytes , 0.00 rows
                 Index count per driver: std.dev.: 0.00
                 Rows per driver: std.dev.: 0.00
                 Size of partition: std.dev.: 0.00
                 count := count("clerk")
 ...
```

