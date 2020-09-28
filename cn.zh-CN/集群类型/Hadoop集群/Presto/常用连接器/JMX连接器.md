# JMX连接器

您可以通过JMX连接器查询Presto集群中所有节点的JMX信息。本连接器通常用于系统监控和调试。通过修改本连接器的配置，可以实现JMX信息定期转储的功能。

## 配置

创建`etc/catalog/jmx.properties`文件，添加如下内容，启用JMX连接器。

```
connector.name=jmx          
```

如果希望定期转储JMX数据，可以在配置文件中添加如下内容。

```
connector.name=jmx
jmx.dump-tables=java.lang:type=Runtime,com.facebook.presto.execution.scheduler:name=NodeScheduler
jmx.dump-period=10s
jmx.max-entries=86400          
```

-   `dump-tables`是用逗号隔开的MBean（Managed Beans）列表。该配置项指定了每个采样周期哪些MBean指标会被采样并存储到内存中。
-   `dump-period`用于设置采样周期，默认为10s。
-   `max-entries`用于设置历史记录的最大长度，默认为86400条。

如果指标项的名称中包含逗号，则需要使用`\\,`进行转义，如下所示。

```
connector.name=jmx
jmx.dump-tables=com.facebook.presto.memory:type=memorypool\\,name=general,\
   com.facebook.presto.memory:type=memorypool\\,name=system,\
   com.facebook.presto.memory:type=memorypool\\,name=reserved      
```

## 数据表

JMX连接器提供了两个Schemas，分别为`current`和`history`。

`current`中包含了Presto集群中每个节点当前的MBean。MBean的名称即为`current`中的表名，如果MBean的名称中包含非标准字符，则需要在查询时使用双引号（"），可以通过如下语句获取。

```
SHOW TABLES FROM jmx.current;        
```

获取每个节点的jvm信息，示例如下。

```
SELECT node, vmname, vmversion
FROM jmx.current."java.lang:type=runtime";

      node    |              vmname               | vmversion
--------------+-----------------------------------+-----------
 ddc4df17-xxx | Java HotSpot(TM) 64-Bit Server VM | 24.60-b09
(1 row)          
```

获取每个节点最大和最小的文件描述符个数指标，示例如下。

```
SELECT openfiledescriptorcount, maxfiledescriptorcount
FROM jmx.current."java.lang:type=operatingsystem";

 openfiledescriptorcount | maxfiledescriptorcount
-------------------------+------------------------
                     329 |                  10240
(1 row)          
```

`history`中包含了配置文件中配置的需要转储的指标对应的数据表。可以通过如下语句查询。

```
SELECT "timestamp", "uptime" FROM jmx.history."java.lang:type=runtime";

        timestamp        | uptime
-------------------------+--------
 2016-01-28 10:18:50.000 |  11420
 2016-01-28 10:19:00.000 |  21422
 2016-01-28 10:19:10.000 |  31412
(3 rows)
```

