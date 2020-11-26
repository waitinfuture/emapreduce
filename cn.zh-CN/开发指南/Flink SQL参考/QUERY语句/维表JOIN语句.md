# 维表JOIN语句

对于每条流式数据，可以关联一个外部维表数据源，为Flink提供数据关联查询。

**说明：** 维表是一张不断变化的表，在维表JOIN时，需指明该条记录关联维表快照的时刻。维表JOIN仅支持对当前时刻维表快照的关联，未来会支持关联左表rowtime所对应的维表快照。关于维表的详细介绍，请参见[概述](/cn.zh-CN/Blink独享/共享集群（原产品线）/Flink SQL参考/DDL语句/创建数据维表/概述.md)。

## 维表JOIN语法

```
SELECT column-names
FROM table1  [AS <alias1>]
[LEFT] JOIN table2 FOR SYSTEM_TIME AS OF PROCTIME() [AS <alias2>]
ON table1.column-name1 = table2.key-name1;
```

**说明：**

-   维表支持INNER JOIN和LEFT JOIN，不支持RIGHT JOIN或FULL JOIN。
-   必须声明FOR SYSTEM\_TIME AS OF PROCTIME\(\)，表示JOIN维表当前时刻所看到的每条数据。
-   源表后面进来的数据只会关联当时维表的最新信息，即JOIN行为只发生在处理时间（Processing Time）。如果JOIN行为发生后，维表中的数据发生了变化（新增、更新或删除），则已关联的维表数据不会被同步变化。
-   ON条件中必须包含维表所有的PRIMARY KEY的等值条件（且要求与真实表定义一致）。此外，ON条件中也可以有其他等值条件。
-   维表和维表不能进行JOIN。

## 示例

```
CREATE TEMPORARY TABLE TEST_ES_SOURCE_TABLE(
  `name` STRING,
   `v2` STRING,
   proctime as PROCTIME()
) with (
   'connector' ='elasticsearch',
   'endPoint' = '<yourEndPoint>',
   'accessId' = '<yourAccessId>',
   'accessKey' = '<yourAccessSecret>',
   'indexName' = '<yourIndexName>',
   'typeNames' = '<yourTypeName>'
 );

CREATE TEMPORARY TABLE TEST_DIM_TABLE(
  `name` STRING,
  `v1` STRING,
  PRIMARY KEY (`name`) NOT ENFORCED
 ) with (
  'connector' = 'redis',
  'host' = '<yourHost>',
  'port' = '<yourPort>',
  'password' = '<yourPassword>',
  'dbNum' = '<yourDbNum>'
);


CREATE TEMPORARY TABLE TEST_SINK_TABLE(
  `v1` STRING,
  `v2` STRING,
  PRIMARY KEY (`v1`) NOT ENFORCED
) with (
  'connector' = 'redis',
  'mode' = 'string',
  'host' = '<yourHost>', 
  'port' = '<yourPort>', 
  'password' = '<yourPassword>',
  'dbNum' = '<yourDbNum>', 
  'ignoreDelete' = 'true'
);


INSERT INTO TEST_SINK_TABLE 
SELECT 
  H.v1, 
  Upper(T.v2) 
FROM TEST_ES_SOURCE_TABLE AS T 
JOIN TEST_DIM_TABLE FOR SYSTEM_TIME AS OF T.proctime AS H 
ON T.name = H.name;
```

