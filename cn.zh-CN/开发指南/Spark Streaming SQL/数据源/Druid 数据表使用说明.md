# Druid 数据表使用说明 {#concept_1322620 .concept}

本文为您介绍如何使用Druid数据表。

## 建表语法 {#section_vko_khm_kro .section}

``` {#codeblock_n2t_fnh_g8k}
create table tbName
using druid
options(propertyKey=propertyValue[, propertyKey=propertyValue]*);
```

## 配置参数说明 {#section_nsw_7g7_gs7 .section}

|参数名|说明|是否必选|
|---|--|----|
|curator.connect|ZooKeeper host，比如，emr-header-1:2181。|是|
|curator.max.retries|ZooKeeper连接失败重试次数，默认值为5。|否|
|curator.retry.base.sleep|ZooKeeper连接失败重试的间隔初始值，默认值为100，单位毫秒。|否|
|curator.retry.max.sleep|ZooKeeper连接失败重试的间隔最大值，默认值为3000，单位毫秒。|否|
|index.service|比如，druid/overlord。|是|
|data.source|写入Druid的data source。|是|
|discovery.path|默认值为/druid/discovery。|否|
|firehouse|比如，druid:firehose:%s。|是|
|rollup.aggregators|tranquility rollup aggregators，JSON格式，比如： ``` {#codeblock_892_o5m_wpn}
{\"metricsSpec\":[{\"type\":\"count\",\"name\":\"count\"},
                  {\"type\":\"doubleSum\",\"fieldName\":\"value\",\"name\":\"sum\"},
                  {\"type\":\"doubleMin\",\"fieldName\":\"value\",\"name\":\"min\"},
                  {\"type\":\"doubleMax\",\"fieldName\":\"value\",\"name\":\"max\"}]}
```

 其中，metricsSpec为固定值。|是|
|rollup.dimensions|写入Druid的dimension。|是|
|rollup.query.granularities|rollup granularity，比如，minute。|是|
|tuning.window.period|tranquility tuning window period，默认值为PT10M。|否|
|tuning.segment.granularity|tranquility tuning segment granularity，默认值为DAY。|否|
|tuning.partitions|tranquility tuning partition，默认值为1。|否|
|tuning.replications|tranquility tuning replication，默认值为1。|否|
|tuning.warming.period|tranquility tuning warming period，默认值为0。|否|
|timestampSpec.column|写入Druid时的timestamp列名，默认值为timestamp。|否|
|timestampSpec.format|写入Druid时的timestamp列名的格式，默认值为iso。|否|

## Table Schema {#section_ky0_c5c_twb .section}

创建 Druid 数据表时，无须显式地定义表的字段信息，例如：

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

