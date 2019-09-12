# Druid data table instructions {#concept_1322620 .concept}

This topic describes how to use a Druid data table.

## Syntax {#section_vko_khm_kro .section}

``` {#codeblock_n2t_fnh_g8k}
create table tbName
using druid
options(propertyKey=propertyValue[, propertyKey=propertyValue]*);
```

## Configuration parameters {#section_nsw_7g7_gs7 .section}

|Parameter|Description|Required|
|---------|-----------|--------|
|curator.connect|The host and port of ZooKeeper, for example, emr-header-1:2181.|Yes|
|curator.max.retries|The maximum number of reconnection attempts upon a ZooKeeper connection failure. Default value: 5.|No|
|curator.retry.base.sleep|The initial interval at which a reconnection attempt is made upon a ZooKeeper connection failure. Default value: 100. Unit: milliseconds.|No|
|curator.retry.max.sleep|The maximum interval at which a reconnection attempt is made upon a ZooKeeper connection failure. Default value: 3000. Unit: milliseconds.|No|
|index.service|The service name of the indexing service overlord node. Example: druid/overlord.|Yes|
|data.source|The name of the data source from which data is written to Druid.|Yes|
|discovery.path|The discovery path of Druid within ZooKeeper. Default value: /druid/discovery.|No|
|firehouse|The firehose. Example: druid:firehose:%s.|Yes|
|rollup.aggregators|The rollup aggregators of Tranquility, in JSON format. Example: ``` {#codeblock_892_o5m_wpn}
{\"metricsSpec\":[{\"type\":\"count\",\"name\":\"count\"},
                  {\"type\":\"doubleSum\",\"fieldName\":\"value\",\"name\":\"sum\"},
                  {\"type\":\"doubleMin\",\"fieldName\":\"value\",\"name\":\"min\"},
                  {\"type\":\"doubleMax\",\"fieldName\":\"value\",\"name\":\"max\"}]}
```

 where, metricsSpec is a fixed value.|Yes|
|rollup.dimensions|The dimension from which data is written to Druid.|Yes|
|rollup.query.granularities|The rollup granularity. Example: minute.|Yes|
|tuning.window.period|The tuning window period of Tranquility. Default value: PT10M.|No|
|tuning.segment.granularity|The tuning segment granularity of Tranquility. Default value: DAY.|No|
|tuning.partitions|The number of tuning partitions of Tranquility. Default value: 1.|No|
|tuning.replications|The number of tuning replication times of Tranquility. Default value: 1.|No|
|tuning.warming.period|The tuning warming period of Tranquility. Default value: 0.|No|
|timestampSpec.column|The name of the timestamp column when data is written to Druid. Default value: timestamp.|No|
|timestampSpec.format|The format of the timestamp column name when data is written to Druid. Default value: iso.|No|

## Table schema {#section_ky0_c5c_twb .section}

When creating a Druid data table, you do not need to explicitly define the fields in the data table. For example, a valid table creation statement is as follows:

``` {#codeblock_nhl_oso_nnj}
create table druid_test_table
using druid
options(
curator.connect="${ZooKeeper-host}:${ZooKeeper-port}}",
index.service="druid/overlord",
data.source="test_source",
discovery.path="/druid/discovery",
firehouse="druid:firehose:%s",
rollup.aggregators="{\"metricsSpec\":[{\"type\":\"count\",\"name\":\"count\"},
                                      {\"type\":\"doubleSum\",\"fieldName\":\"value\",\"name\":\"sum\"},
                                      {\"type\":\"doubleMin\",\"fieldName\":\"value\",\"name\":\"min\"},
                                      {\"type\":\"doubleMax\",\"fieldName\":\"value\",\"name\":\"max\"}]}",
rollup.dimensions="timestamp,metric,userId",
rollup.query.granularities="minute",
tuning.segment.granularity="FIVE_MINUTE",
tuning.window.period="PT5M",
timestampSpec.column="timestamp",
timestampSpec.format="posix");
```

