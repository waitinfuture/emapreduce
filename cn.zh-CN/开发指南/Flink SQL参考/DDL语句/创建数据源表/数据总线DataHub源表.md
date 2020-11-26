---
keyword: [DataHub, 源表]
---

# 数据总线DataHub源表

本文为您介绍数据总线DataHub源表的DDL定义、WITH参数、类型映射、属性字段和常见问题。

## 什么是数据总线DataHub

阿里云流数据处理平台DataHub是流式数据（Streaming Data）的处理平台，提供对流式数据的发布（Publish）、订阅（Subscribe）和分发功能，让您可以轻松构建基于流式数据的分析和应用。

## DDL定义

```
create table datahub_source(
  name varchar,
  age BIGINT,
  birthday BIGINT
) with (
  'connector'='datahub',
  'endPoint'='<endPoint>',
  'project'='<yourProjectName>',
  'topic'='<yourTopicName>',
  'subId'='<yourSubId>',
  'accessId'='<yourAccessId>',
  'accessKey'='<yourAccessKey>'
);
```

## WITH参数

|参数|说明|是否必填|备注|
|--|--|----|--|
|connector|源表类型|是|固定值为`datahub`。|
|endPoint|消费端点信息|是|详情请参见[DataHub域名列表](https://help.aliyun.com/document_detail/158778.html?spm=a2c4g.11186623.6.547.77a91fd1eveQrC)。|
|accessId|AccessKey ID|是|无|
|accessKey|AccessKey Secret|是|无|
|project|读取的项目|是|无|
|topic|Project下的具体的Topic名称|是|无|
|subId|topic的订阅ID|是|多个任务不能同时使用同一个订阅。|
|startTime|启动位点的时间|否|格式为`yyyy-MM-dd hh:mm:ss`。|
|retryTimeout|最大持续重试时间|否|单位为毫秒，默认值为180000毫秒（半小时）。|
|retryInterval|重试间隔|否|单位为毫秒，默认值为1000。|
|maxFetchSize|单次读取条数|否|默认值为50。|
|maxBufferSize|异步读取的最大缓存数据条数|否|默认值为50。|
|lengthCheck|单行字段条数检查策略|否|-   NONE（默认值）：
    -   解析出的字段数大于定义字段数时，按从左到右的顺序，取定义字段数量的数据。
    -   解析出的字段数小于定义字段数时，跳过该行数据。
-   SKIP：解析出的字段数和定义字段数不同时跳过该行数据。
-   EXCEPTION：解析出的字段数和定义字段数不同时提示异常。
-   PAD：按从左到右顺序填充。
    -   解析出的字段数大于定义字段数时，按从左到右的顺序，取定义字段数量的数据。
    -   解析出的字段数小于定义字段数时，按从左到右的顺序，在行尾用Null填充缺少的字段。 |
|columnErrorDebug|是否打开调试开关。|否|-   false（默认值）：关闭调试功能。
-   true：打开调试开关，打印解析异常的日志。 |

**说明：** 使用BLOB类型时，字段需要声明为VARBINARY类型，与METAQ类似。

## 类型映射

DataHub和Flink字段类型对应关系如下，建议使用该对应关系时进行DDL声明。

|DataHub字段类型|Flink字段类型|
|-----------|---------|
|STRING|VARCHAR|
|TIMESTAMP|BIGINT|
|TINYINT|TINYINT|
|SMALLINT|SMALLINT|
|INTEGER|INTEGER|
|BIGINT|BIGINT|
|FLOAT|FLOAT|
|DOUBLE|DOUBLE|
|BOOLEAN|BOOLEAN|
|DECIMAL|DECIMAL|

## 属性字段

Flink SQL支持获取DataHub的属性字段。通过读取属性字段可以获得每条信息输入DataHub的系统时间（System Time）。

|字段名|注释说明|
|---|----|
|TIMESTAMP|每条记录写入DataHub的系统时间（System Time）。|

![Shard数据抽样](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6084359951/p64847.png)

**说明：** 获取属性字段的方法，请参见[获取数据源表属性字段](/cn.zh-CN/Blink独享/共享集群（原产品线）/Flink SQL参考/DDL语句/创建数据源表/数据源表概述.md)。

## 代码示例

```
CREATE TEMPORARY table datahub_source(
  name VARCHAR
) with (
  'connector'='datahub',
  'endpoint'='<endPoint>',
  'project'='<yourProjectName>',
  'topic'='<yourTopicName>',
  'subId'='<yourSubId>',
  'accessId'='<yourAccessId>',
  'accessKey'='<yourAccessKey>',
  'startTime'='2018-06-01 00:00:00'
);

CREATE TEMPORARY table datahub_sink(
  name  VARCHAR  
) with (
  'connector'='datahub',
  'endpoint'='<endPoint>',
  'project'='<yourProjectName>',
  'topic'='<yourTopicName>',
  'accessId'='<yourAccessId>',
  'accessKey'='<yourAccessKey>'
);

INSERT INTO datahub_sink
SELECT 
  LOWER(name)
from datahub_source;
```

## 常见问题

-   Q：分裂或者缩容DataHub Topic后导致Flink作业失败，如何恢复？

    A：如果分裂或者缩容了Flink正在读取的某个Topic，则会导致任务持续出错，无法自行恢复。该情况下需要重新启动（停止-\>启动）来使任务恢复正常。

-   Q：可以删除正在消费的DataHub Topic吗？

    A：不支持删除或重建正在消费的DataHub Topic。


