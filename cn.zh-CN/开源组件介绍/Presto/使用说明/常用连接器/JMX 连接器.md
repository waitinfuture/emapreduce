# JMX 连接器 {#concept_y5p_j4r_zgb .concept}

可以通过 JMX 连接器查询 Presto 集群中所有节点的 JMX 信息。本连接器通常用于系统监控和调试。通过修改本连接器的配置，可以实现 JMX 信息定期转储的功能。

## 配置 {#section_wsy_z4r_zgb .section}

创建`etc/catalog/jmx.properties`文件，添加如下内容，一边启用 JMX 连接器。

```
connector.name=jmx
			
```

如果希望定期转储 JMX 数据，可以在配置文件中添加如下内容：

```
connector.name=jmx
jmx.dump-tables=java.lang:type=Runtime,com.facebook.presto.execution.scheduler:name=NodeScheduler
jmx.dump-period=10s
jmx.max-entries=86400
			
```

其中：

-   `dump-tables`是用逗号隔开的 MBean（Managed Beans）列表。该配置项指定了每个采样周期哪些 MBean 指标会被采样并存储到内存中；
-   `dump-period`用于设置采样周期，默认为 10 s；
-   `max-entries`用于设置历史记录的最大长度，默认为 86400 条。

如果指标项的名称中包含逗号，则需要使用`\\,`进行转义，如下所示：

```
connector.name=jmx
jmx.dump-tables=com.facebook.presto.memory:type=memorypool\\,name=general,\
   com.facebook.presto.memory:type=memorypool\\,name=system,\
   com.facebook.presto.memory:type=memorypool\\,name=reserved
			
```

## 数据表 {#section_ysy_z4r_zgb .section}

JMX 连接器提供了两个 schemas，分别为`current`和`history`。其中：

`current`中包含了 Presto 集群中每个节点的当前的 MBean，BMean 的名称即为`current`中的表名\(如果 bean 的名称中包含非标准字符，则需在查询时用`双引号`将表名扩起来\)，可以通过如下语句获取：

```
SHOW TABLES FROM jmx.current;
			
```

示例：

```
--- 获取每个节点的jvm信息
SELECT node, vmname, vmversion
FROM jmx.current."java.lang:type=runtime";

      node    |              vmname               | vmversion
--------------+-----------------------------------+-----------
 ddc4df17-xxx | Java HotSpot(TM) 64-Bit Server VM | 24.60-b09
(1 row)
			
```

```
--- 获取每个节点最大和最小的文件描述符个数指标
SELECT openfiledescriptorcount, maxfiledescriptorcount
FROM jmx.current."java.lang:type=operatingsystem";

 openfiledescriptorcount | maxfiledescriptorcount
-------------------------+------------------------
                     329 |                  10240
(1 row)
			
```

`history`中包含了配置文件中配置的需要转储的指标对应的数据表。可以通过如下语句进行查询：

```
SELECT "timestamp", "uptime" FROM jmx.history."java.lang:type=runtime";

        timestamp        | uptime
-------------------------+--------
 2016-01-28 10:18:50.000 |  11420
 2016-01-28 10:19:00.000 |  21422
 2016-01-28 10:19:10.000 |  31412
(3 rows)
```

