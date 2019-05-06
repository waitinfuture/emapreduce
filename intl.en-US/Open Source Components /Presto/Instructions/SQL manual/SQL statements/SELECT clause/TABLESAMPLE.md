# TABLESAMPLE {#concept_lhx_vnl_zgb .concept}

Presto provides two sampling methods: `BERNOULLI` and `SYSTEM`. Neither of the two methods can determine the number of rows of the sampling result set.

## BERNOULLI {#section_jlh_wnl_zgb .section}

This sampling method selects each row of the sampled table based on a probability of the sample percentage. When a table is sampled by using this method, all the physical blocks of the table are scanned, and certain rows are skipped based on a comparison between the sample percentage and a random value that is calculated during runtime.

The probability of a row being included in the result is independent from that of any other row. This does not reduce the time required to read the sampled table from the disk. Further processing of the sampled output may have an impact on the total query time.

## SYSTEM {#section_klh_wnl_zgb .section}

This sampling method divides a table into logical segments of data and samples the table at this granularity. This sampling method either selects all the rows from a particular segment of data or skips it \(based on a comparison between the sample percentage and a random value calculated during runtime\).

The selection of rows in SYSTEM sampling is dependent on which connector is used. For example, when used with Hive, it is dependent on how the data is distributed in HDFS. This method does not guarantee independent sampling probabilities.

## Examples {#section_n4s_d4l_zgb .section}

```
--- Use BERNOULLI sampling
SELECT *
FROM users TABLESAMPLE BERNOULLI (50);

--- Use SYSTEM sampling
SELECT *
FROM users TABLESAMPLE SYSTEM (75);
Using sampling with joins:

--- Use sampling with JOIN
SELECT o.*, i. *
FROM orders o TABLESAMPLE SYSTEM (10)
JOIN lineitem i TABLESAMPLE BERNOULLI (40)
  ON o.orderkey = i.orderkey;
```

