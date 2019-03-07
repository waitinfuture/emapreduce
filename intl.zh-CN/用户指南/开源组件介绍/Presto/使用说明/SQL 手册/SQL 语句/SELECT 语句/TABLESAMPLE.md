# TABLESAMPLE {#concept_lhx_vnl_zgb .concept}

Presto 提供了两种采样方式，`BERNOULLI`和`SYSTEM`，但是无论哪种采样方式， 都无法确定的给出采样结果集的行数。

## BERNOULLI {#section_jlh_wnl_zgb .section}

本方法基于样本百分比概率进行采样。当使用 Bernoulli 方法对表格进行采样时，执行器会扫描表格的所有物理块，并且基于样本百分比与运行时计算的随机值之间的比较结果跳过某些行。

每一行被选中对概率是独立对，与其他行无关，但是，这不会减少从磁盘读取采样表所需的时间。如果还需对采样结果做进一步处理，可能会对总查询时间产生影响。

## SYSTEM {#section_klh_wnl_zgb .section}

本方法将表格分成数据的逻辑段，并以此粒度对表格进行采样。这种抽样方法要么从一个特定的数据段中选择所有的行，要么跳过它（根据样本百分比和在运行时计算的一个随机值之间的比较）。

采样结果与使用哪种连接器有关。例如，与 Hive 一起使用时，取决于数据在 HDFS 上的分布。这种方法不保证独立的抽样概率。

## 示例 {#section_n4s_d4l_zgb .section}

```
--- 使用BERNOULLI采样
SELECT *
FROM users TABLESAMPLE BERNOULLI (50);

--- 使用SYSTEM采样
SELECT *
FROM users TABLESAMPLE SYSTEM (75);
Using sampling with joins:

--- 采样过程中使用JOIN
SELECT o.*, i.*
FROM orders o TABLESAMPLE SYSTEM (10)
JOIN lineitem i TABLESAMPLE BERNOULLI (40)
  ON o.orderkey = i.orderkey;
```

