# Kafka 连接器 {#concept_i3n_lfr_zgb .concept}

本连接器将 Kafka 上的 topic 映射为 Presto 中的表。Kafka 中的每条记录都映射为 Presto 表中的消息。

**说明：** 

由于 Kafka 中数据是动态变化的，因此，在使用 Presto 进行多次查询时，可能会出现数据不一致的情形。目前，Presto 还没有处理这些情况的能力。

## 配置 {#section_evj_1gr_zgb .section}

创建`etc/catalog/kafka.properties`文件，添加如下内容，一边启用 Kafka 连接器。

```
connector.name=kafka
kafka.table-names=table1,table2
kafka.nodes=host1:port,host2:port
			
```

**说明：** 

Presto 可以同时连接多个 Kafka 集群，只需要在配置目录中添加新的 properties 文件即可，文件名将会被映射为 Presto 的 catalog。如新增一个"orders.properties"配置文件，Presto 会创建一个新的名为'orders'的 catalog。

```
## orders.properties
connector.name=kafka  # 这个表示连接器类型，不能变
kafka.table-names=tableA,tableB
kafka.nodes=host1:port,host2:port
			
```

Kafka 连接器提供了如下几个属性：

|参数|是否必选|描述|说明|
|--|----|--|--|
|kafka.table-names|是|定义本连接器支持的表格列表|这里的文件名可以使用 Schema 名称进行修饰，形式如`{schema_name}.{table_name}`。也可以不使用 Schema 名称修饰，此时，表格将被映射到`kafka.default-schema`中定义的schema中。|
|kafka.default-schema| |默认的Schema名称, 默认值为`default`| |
|kafka.nodes|是|Kafka集群中的节点列表|配置格式形如`hostname:port[,hostname:port...]`。此处可以只配置部分 Kafka 节点，但是，Presto 必须能够连接到 Kafka 集群中的所有节点。否则，可能拿不到部分数据。|
|kafka.connect-timeout|否|连接器与 Kafka 集群的超时时间，默认为 10 秒|如果 Kafka 集群压力比较大，创建连接可能需要相当长的时间，从而导致 Presto 在执行查询时出现超时的情况。此时，增加当前的配置值是一个不错的选择。|
|kafka.buffer-size|否|读缓冲区大小，默认为 64 kb|用于设置从 Kafka 读取数据的内部数据缓冲区的大小。 数据缓冲区必须至少大于一条消息的大小。 每个 Worker 和数据节点都会分配一个数据缓冲区。|
|kafka.table-description-dir|否|Topic\(表\)描述文件目录，默认为`etc/kafka`|该目录下保存着 JSON 格式的数据表定义文件（必须使用`.json`作为后缀）。|
|kafka.hide-internal-columns|否|需要隐藏的预置列清单，默认为`true`|除了在表格描述文件中定义的数据列之外，连接器还为每个表格维护了许多额外的列。本属性用于控制这些列在是否会在`DESCRIBE <table-name>`和`SELECT *`语句的执行结果中显示。无论本配置设置是什么，这些列都可以参与查询过程中。|

-   Kafka连接器提供的内部列如下表所示：

    |列名|类型|说明|
    |--|--|--|
    |\_partition\_id|BIGINT|本行记录所在的 Kafka 分区 ID。|
    |\_partition\_offset|BIGINT|本行记录在 Kafka 分区中的偏移量。|
    |\_segment\_start|BIGINT|包含此行的数据段的最低偏移量。此偏移量是针对每个分区的。|
    |\_segment\_end|BIGINT|包含此行的数据段的最大偏移量\(为下一个段的起始偏移量\)。此偏移量是针对每个分区的。|
    |\_segment\_count|BIGINT|当前行在数据段中的序号. 对于没有压缩的topic, `_segment_start`+ `_segment_count` = `_partition_offset`.|
    |\_message\_corrupt|BOOLEAN|如果解码器无法解码本行记录，本字段将会被设为`TRUE`。|
    |\_message|VARCHAR|将消息字节作为 UTF-8 编码的字符串。当 topic 的消息为文本类型是，这个字段比较有用。|
    |\_message\_length|BIGINT|本行消息的字节长度。|
    |\_key\_corrupt|BOOLEAN|如果键解码器无法解码本行记录，该字段将会被设为`TRUE`。|
    |\_key|VARCHAR|将键字节作为 UTF-8 编码的字符串。当 topic 的消息为文本类型是，这个字段比较有用。|
    |\_key\_length|BIGINT|消息中键的字节长度。|

    **说明：** 对于那些没有定义文件的表，`_key_corrupt`和`_message_corrupt`默认为`FALSE`。


## 表格定义文件 {#section_xvj_1gr_zgb .section}

Kafka 本身是 Schema-Less 的消息系统，消息的格式需要生产者和消费者自己来定义。而 Presto 要求数据必须可以被映射成表的形式。因此，通常需要用户根据消息的实际合适，提供对应的表格定义文件。对于 JSON 格式的消息，如果没有提供定义文件，也可以在查询时使用Presto的JSON函数进行解析。这种处理方式虽然比较灵活，但是对会增加 SQL 语句的编写难度。

一个表格描述文件使用 JSON 来定义一个表，文件名可以任意，但是必须以`.json`作为扩展名。

```
{
    "tableName": ...,
    "schemaName": ...,
    "topicName": ...,
    "key": {
        "dataFormat": ...,
        "fields": [
            ...
        ]
    },
    "message": {
        "dataFormat": ...,
        "fields": [
            ...
       ]
    }
}
			
```

|字段|可选性|类型|说明|
|--|---|--|--|
|tableName|必选|string|Presto 表名|
|schemaName|可选|string|表所在的 Schema 名称|
|topicName|必选|string|Kafka topic 名称|
|key|可选|JSON object|消息键到列的映射规则|
|message|可选|JSON object|消息到列的映射规则|

其中，键和消息的映射规则使用如下几个字段来描述。

|字段|可选性|类型|说明|
|--|---|--|--|
|dataFormat|必选|string|设置一组列的解码器|
|fields|必选|JSON 数组|列定义列表|

这里的`fields`是一个 JSON 数组，每一个元素为如下的 JOSN 对象：

```
{
    "name": ...,
    "type": ...,
    "dataFormat": ...,
    "mapping": ...,
    "formatHint": ...,
    "hidden": ...,
    "comment": ...
}
			
```

|字段|可选性|类型|说明|
|--|---|--|--|
|name|必选|string|列名|
|type|必选|string|列的数据类型|
|dataFormat|可选|string|列数据解码器|
|mapping|可选|string|解码器参数|
|formatHint|可选|string|设置在该列上的提示，可以被解码器使用|
|hiddent|可选|boolean|是否隐藏|
|comment|可选|string|列的描述|

## 解码器 {#section_pwj_1gr_zgb .section}

解码器的功能是将 Kafka 的消息（key+message）映射到数据表到列中。如果没有表的定义文件，Presto 将使用`dummy`解码器。

Kafka 连接器提供了如下 3 中解码器：

-   `raw` - 不做转换，直接使用原始的 bytes
-   `csv` - 将消息作为 CSV 格式的字符串进行处理
-   `json` - 将消息作为 JSON 进行处理

