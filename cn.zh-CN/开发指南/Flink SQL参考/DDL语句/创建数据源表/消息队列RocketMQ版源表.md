---
keyword: [消息队列, 源表, MQ]
---

# 消息队列RocketMQ版源表

本文为您介绍消息队列RocketMQ版源表DDL定义、WITH参数、类型映射和类型映射。

## 什么是消息队列RocketMQ版

消息队列 RocketMQ版是阿里云基于Apache RocketMQ构建的低延迟、高并发、高可用和高可靠的分布式消息中间件。消息队列RocketMQ版既可为分布式应用系统提供异步解耦和削峰填谷的能力，同时也具备互联网应用所需的海量消息堆积、高吞吐和可靠重试等特性。

## DDL定义

```
create table mq_source(
   x varchar,
   y varchar,
   z varchar
) with (
   'connector'='mq',
   'topic'='<yourTopicName>',
   'endpoint'='<yourEndpoint>',
   'pullIntervalMs'='1000',
   'accessId'='<yourAccessId>',
   'accessKey'='<yourAccessSecret>',
   'startMessageOffset'='1000',
   'consumerGroup'='<yourConsumerGroup>',
   'fieldDelimiter'='|'
);
```

**说明：** MQ是非结构化存储格式的消息中间件，对于数据的Schema不提供强制定义，完全由业务层指定。Flink仅支持CSV和二进制格式的MQ消息。

## WITH参数

|参数|说明|是否必填|备注|
|--|--|----|--|
|connector|源表类型|是|固定值为`mq`。|
|topic|topic名称|是|无|
|endPoint|endPoint地址|是|阿里云消息队列RocketMQ版接入地址支持以下两种类型： -   内网服务（阿里云经典网络/VPC）： 华北2（北京）、华东2（上海）、华东1（杭州）、华南1（深圳）：`onsaddr-internal.aliyun.com:8080`。

**说明：** 仅VVR 2.1.1及以上版本支持以上地域。

-   公网服务：`onsaddr-internet.aliyun.com:80`。 |
|accessId|AccessKey ID|是|无|
|accessKey|AccessKey Secret|是|无|
|consumerGroup|订阅消费group名称|是|无|
|pullIntervalMs|拉取时间间隔|是|单位为毫秒。|
|nameServerSubgroup|NameServer组|否|-   内网服务（阿里云经典网络/VPC）：nsaddr4client-internal
-   公网服务：nsaddr4client-internet

**说明：** 仅VVR 2.1.1及以上版本支持该参数。 |
|startTime|消息消费启动的时间点|否|无|
|startMessageOffset|消息开始的偏移量|否|如果填写，将优先以startMessageoffset的位点开始加载数据。|
|tag|订阅的标签|否|无|
|lineDelimiter|解析Block时，行分隔符|否|默认值为 `\n`。|
|fieldDelimiter|字段分隔符|否|默认值为`\u0001` 。表示在只读模式下以`\u0001`（`\u0001`在只读模式不可见）作为分隔符，在编辑模式下以`^A`作为分隔符。|
|encoding|编码格式|否|默认值为`utf-8`。|
|lengthCheck|单行字段条数检查策略|否|默认值为NONE，表示： -   解析出的字段数大于定义字段数时，按从左到右的顺序，取定义字段数量的数据。
-   解析出的字段数小于定义字段数时，跳过这行数据。

其它可选值为SKIP、EXCEPTION和PAD。 -   SKIP：解析出的字段数和定义字段数不同时跳过这行数据。
-   EXCEPTION：解析出的字段数和定义字段数不同时提示异常。
-   PAD：按从左到右顺序填充。
    -   解析出的字段数大于定义字段数时，按从左到右的顺序，取定义字段数量的数据。
    -   解析出的字段数小于定义字段数时，在行尾用null填充缺少的字段。 |
|columnErrorDebug|是否打开调试开关。|否|默认值为FALSE。如果设置为TRUE，则打印解析异常的Log。|
|instanceID|实例ID|否|根据MQ实例是否有独立命名空间，执行如下操作：-   是，必须配置instanceID参数。
-   否，不能配置instanceID参数。 |

## 类型映射

|MQ字段类型|Flink字段类型|
|------|---------|
|STRING|VARCHAR|

## 代码示例

-   CSV格式

    假设您的1条CSV格式消息记录如下。

    ```
    1,name,male 
    2,name,female
    ```

    **说明：** 1条MQ消息可以包括0条到多条数据记录，记录之间使用`\n`分隔。

    Flink作业中，声明MQ数据源表的DDL如下。

    ```
    create table mq_source(
       id varchar,
       name varchar,
       gender varchar
    ) with (
       'connector'='mq',
       'topic'='<yourTopicName>',
       'endpoint'='<yourEndpoint>',
       'pullIntervalMs'='1000',
       'accessId'='<yourAccessId>',
       'accessKey'='<yourAccessSecret>',
       'startMessageOffset'='1000',
       'consumerGroup'='<yourConsumerGroup>',
       'fieldDelimiter'='|'
    );
    ```

-   二进制格式

    ```
    create table mq_source (
       mess varbinary
    ) with (
       'connector'='mq',
       'endpoint'='<yourEndpoint>',
       'pullIntervalMs'='500',
       'accessId'='<yourAccessId>',
       'accessKey'='<yourAccessSecret>',
       'topic'='<yourTopicName>',
       'consumerGroup'='<yourConsumerGroup>'
    );
    ```


