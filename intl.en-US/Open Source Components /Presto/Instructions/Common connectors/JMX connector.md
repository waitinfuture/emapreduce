# JMX connector {#concept_y5p_j4r_zgb .concept}

## Overview {#section_vsy_z4r_zgb .section}

The JMX connector is used to query the JMX information about all the nodes in the Presto cluster. The JMX connector is typically used for system monitoring and debugging. You can modify the connector configuration to perform regular dump of JMX information.

## Configuration {#section_wsy_z4r_zgb .section}

Create the file `etc/catalog/jmx.properties`, add the following content, and enable the JMX connector.

```
connector.name=jmx

```

You can add the following content into the configuration file to implement regular dump of JMX data:

```
connector.name=jmx
jmx.dump-tables=java.lang:type=Runtime,com.facebook.presto.execution.scheduler:name=NodeScheduler
jmx.dump-period=10s
jmx.max-entries=86400

```

In the example:

-   `dump-tables` is a list of Managed Beans \(MBeans\) separated with commas \(,\). This configuration specifies which MBeans are sampled and stored in the memory during each sampling period.
-   `dump-period` indicates the sampling period, which is 10s by default.
-   `max-entries` indicates the maximum number of historical records, which is 86,400 by default.

If the name of a metric contains a comma \(,\), it must be escaped by using `\\,` as follows:

```
connector.name=jmx
jmx.dump-tables=com.facebook.presto.memory:type=memorypool\\,name=general,\
   com.facebook.presto.memory:type=memorypool\\,name=system,\
   com.facebook.presto.memory:type=memorypool\\,name=reserved

```

## Data tables {#section_ysy_z4r_zgb .section}

The JMX connector provides two schemas: `current` and `history`. Specifically:

`current` contains the current MBean of each node. The MBean name is the same as the table name in `current`. If the MBean name contains non-standard characters, the table name must be enclosed by `quotation marks` during the query. The MBean name can be obtained through the following statement:

```
SHOW TABLES FROM jmx.current;

```

Examples:

```
--- Obtain the JVM information about each node
SELECT node, vmname, vmversion
FROM jmx.current."java.lang:type=runtime";

      node    |              vmname               | vmversion
--------------+-----------------------------------+-----------
 ddc4df17-xxx | Java HotSpot(TM) 64-Bit Server VM | 24.60-b09
(1 row)

```

```
--- Obtain the metrics that indicate the maximum number and minimum number of file descriptors for each node
SELECT openfiledescriptorcount, maxfiledescriptorcount
FROM jmx.current."java.lang:type=operatingsystem";

 openfiledescriptorcount | maxfiledescriptorcount
-------------------------+------------------------
                     329 |                  10240
(1 row)

```

`history` contains the data table corresponding to the metrics to be dumped in the configuration file. The following statement queries the data table:

```
SELECT "timestamp", "uptime" FROM jmx.history."java.lang:type=runtime";

        timestamp        | uptime
-------------------------+--------
 2016-01-28 10:18:50.000 |  11420
 2016-01-28 10:19:00.000 |  21422 
 2016-01-28 10:19:10.000 |  31412
(3 rows)
```

