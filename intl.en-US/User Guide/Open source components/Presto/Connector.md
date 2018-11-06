# Connector {#concept_x1d_4zd_z2b .concept}

## System connector {#section_wc2_d12_z2b .section}

-   Overview

    SQL can be used to query the basic information and measurements of the Presto cluster through the connector.

-   Configuration

    All information can be obtained through a catalog known as system without configuration.

    Examples

    ```
    --- List all supported data entries
    SHOW SCHEMAS FROM system;
    --- List all data entries in the project during runtime
    SHOW TABLES FROM system.runtime;
    --- Obtain node status
    SELECT * FROM system.runtime.nodes;
         node_id |         http_uri         | node_version | coordinator | state
    -------------+--------------------------+--------------+-------------+--------
     3d7e8095-...| http://192.168.1.100:9090| 0.188        | false       | active
     7868d742-...| http://192.168.1.101:9090| 0.188        | false       | active
     7c51b0c1-...| http://192.168.1.102:9090| 0.188        | true        | active
    --- Force cancel a query
    CALL system.runtime.kill_query('20151207_215727_00146_tx3nr');
    ```

-   Data tables

    The connector provides the following data tables:

    |TABLE|SCHEMA|DESCRIPTION|
    |:----|:-----|:----------|
    |catalogs|metadata|This table contains the list of all catalogs supported in the connector|
    |schema\_properties|metadata|This table contains the list of available properties that can be set when creating a Schema.|
    |table\_properties|metadata|This table contains the list of available properties that can be set when creating a table.|
    |nodes|runtime|This table contains the list of all visible nodes and statuses thereof in the Presto cluster.|
    |queries|runtime|This table contains information queries currently and recently initiated in the Presto cluster, including the original query texts \(SQL\), identities of the users who initiate the queries and information on query performances, for example, query queue and analysis time, etc.|
    |tasks|runtime|This table contains information on the task involved in the queries in the Presto, including the locations and numbers of lines and bytes processed in each task.|
    |transactions|runtime|This table contains the list of currently opened transactions and related metadata. The data includes information like creation time, idle time, initiation parameters, and access catalogs.|

-   Stored procedures

    The connector supports the following stored procedures:

    `runtime.kill_query(id)` Cancel query from specified ID.


## JMX connector {#section_hjf_512_z2b .section}

-   Overview

    JMX information for all nodes in the Presto cluster can be queried through the JMX connector. The connector is generally used for system monitoring and debugging. Regular dump of JMX information can be implemented through modifying the configuration of the connector.

-   **Configuration**

    Create a file etc/catalog/jmx.properties, add the following content, and enable JMX connector.

    ```
    connector.name=jmx
    ```

    If regular dump of JMX data is expected, the following content can be added in the configuration file:

    ```
    connector.name=jmx
    jmx.dump-tables=java.lang:type=Runtime,com.facebook.presto.execution.scheduler:name=NodeScheduler
    jmx.dump-period=10s
    jmx.max-entries=86400
    ```

    Where:

    -   dump-tables is a list of MBeans \(Managed Beans\) separated with commas. This configuration specifies which MBeans is sampled and stored in the memory for each sample period.
    -   dump-period is used for setting the sample period, which is 10s by default.
    -   max-entries is used for setting the max length of the history, which is 86400 by default.
    If the name of a metric contains a comma, it must be escaped using `\,` as follows:

    ```
    connector.name=jmx
    jmx.dump-tables=com.facebook.presto.memory:type=memorypool\\,name=general,\
       com.facebook.presto.memory:type=memorypool\\,name=system,\
       com.facebook.presto.memory:type=memorypool\\,name=reserved
    ```

-   Data tables

    JMX connector provides 2 schemas, **current** and **history**, respectively. Where:

    **current** contains the current MBean in each node, the name of which is the table name in **current** \(if the bean name contains non-standard characters, the table name must be in quotation marks for the query\), which can be obtained through the following statement:

    ```
    SHOW TABLES FROM jmx.current;
    ```

    Examples

    ```
    --- Obtain jvm information for each node
    SELECT node, vmname, vmversion
    FROM jmx.current."java.lang:type=runtime";
          node    |              vmname               | vmversion
    --------------+-----------------------------------+-----------
     ddc4df17-xxx | Java HotSpot(TM) 64-Bit Server VM | 24.60-b09
    (1 rows)
    ```

    ```
    --- Obtain the metrics of max and min file descriptor counts for each node
    SELECT openfiledescriptorcount, maxfiledescriptorcount
    FROM jmx.current."java.lang:type=operatingsystem";
     openfiledescriptorcount | maxfiledescriptorcount
    -------------------------+------------------------
                         329 |                  10240
    (1 rows)
    ```

    **history** contains the data table corresponding to the metrics to be dumped in the configuration file. The following statement can be used to query:

    ```
    SELECT "timestamp", "uptime" FROM jmx.history."java.lang:type=runtime";
            timestamp        | uptime
    -------------------------+--------
     2016-01-28 10:18:50.000 |  11420
     2016-01-28 10:19:00.000 |  21422
     2016-01-28 10:19:10.000 |  31412
    (3 rows)
    ```


## Kafka connector {#section_j2t_212_z2b .section}

-   **Overview**

    The connector maps topic in Kafka to tables in Presto. Each record in Kafka is mapped to a message in Presto tables.

    **Note:** Since data in Kafka is dynamic, when Presto is used for multiple queries, something strange may sometimes occur. Currently, Presto is incapable of handling such cases.

-   Configuration

    Create a file etc/catalog/kafka.properties, add the following content, and enable Kafka connector.

    ```
    connector.name=kafka
    kafka.table-names=table1,table2
    kafka.nodes=host1:port,host2:port
    ```

    **Note:** Presto can connect to multiple Kafka cluster through adding a new properties file in the configuration catalog, and the file name is mapped to the Presto catalog. For example, when a configuration file orders.properties is added, Presto creates a catalog named orders.

    ```
    ## orders.properties
    connector.name=kafka  # It denotes the connector type, which cannot be changed
    kafka.table-names=tableA,tableB
    kafka.nodes=host1:port,host2:port
    ```

    Kafka connector provides the following properties:

    -   kafka.table-names

        Description: Required, it defines the list of tables supported in the connector.

        Details: The file name here can be modified using Schema name, with forms like \{schema\_name\}.\{table\_name\}. The file name also can be not modified using Schema name, and the table is mapped to the Schema defined in kafka.default-schema .

    -   kafka.default-schema

        Description: Optional，default Schema name, with the default value default.

    -   kafka.nodes

        Description: Required，the node list in the Kafka cluster.

        Details: the configuration form is like `hostname:port[,hostname:port…]`. You can configure only part of the Kafka nodes here, but Presto must be capable of connecting to all nodes in the Kafka cluster. Otherwise, a part of data may not be obtained.

    -   kafka.connect-timeout

        Description: Optional, timeout for the connector and the Kafka cluster, which is 10s by default.

        Details: If the pressure on the Kafka cluster is large, a long time may be taken for creating a connection, causing timeout when executing the query by Presto. At this time, adding the configured value is a better choice.

    -   kafka.buffer-size

        Description: Optional, read buffer size, which is 64 kb by default.

        Details: It is used to set the size of the buffer for internal data reads from Kafka. The data buffer must have a size larger than that of a message. A data buffer is distributed for each worker and data node.

    -   kafka.table-description-dir

        Description: \[Optional\]，the catalog of topic \(table\) description file, which is etc/kafka by default.

        Details: Data table definition files in JSON format is stored in this directory \(.json has to be used as a suffix\).

    -   kafka.hide-internal-columns

        Description: Optional, the list of preset columns to be hidden, the value of which is true by default.

        Details: In addition to data columns defined in the table description file, the connector also maintains many extra columns for each table. The property is used to control whether these columns are shown in the execution result of the statement `DESCRIBE` and `SELECT *`. Regardless of the setting in the configuration, these columns are involved in the query process.

    The Kafka connector provides internal columns as shown in the following table:

    |Column name|Type|Description|
    |:----------|:---|:----------|
    |\_partition\_id|BIGINT|The ID of the Kafka partition where the record is located.|
    |\_partition\_offset|BIGINT|The offset of the Kafka partition where the record is located.|
    |\_segment\_start|BIGINT|The lowest offset containing this data segment. This offset is for each partition.|
    |\_segment\_end|BIGINT|The largest offset containing this data segment \(which is the start offset of the next segment\). This offset is for each partition.|
    |\_segment\_count|BIGINT|The serial number of the column in this segment. For an uncompressed topic, \_segment\_start + \_segment\_count = \_partition\_offset.|
    |\_message\_corrupt|BOOLEAN|This field will be set to TRUE if a decoder cannot decode the record.|
    |\_message|VARCHAR|A string coded with UTF-8 from the message bytes. When the type of the topic message is text, the field will be useful.|
    |\_message\_length|BIGINT|The byte length of the message.|
    |\_key\_corrupt|BOOLEAN|This field will be set to TRUE if a key decoder cannot decode the record.|
    |\_key|VARCHAR|A string coded with UTF-8 from the key bytes. When the type of the topic message is text, the field will be useful.|
    |\_key\_length|BIGINT|The byte length of the key.|

    **Note:** For those tables without definition files, \_key\_corrupt and \_message\_corrupt remain FALSE.

-   Table definition files

    Kafka is a Schema-Less message system, and the formats of the messages must defined by the producers and consumers. While Presto requires that data must be capable of being mapped into tables. Therefore, the users must provide corresponding table definition files according to the actual uses of the messages. For messages in JSON format, if a definition file is not provided, JSON functions in Presto can be used for resolution in the queries. While the method is flexible, it increases the difficulty of writing SQL statements.

    When JSON is used to define a table in a table definition file, the file name can be customized, while the extension must be \*.json.

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

    |Field|Optionality|Type|Description|
    |:----|:----------|:---|:----------|
    |tableName|required|string|Presto table name|
    |schemaName|optional|string|the name of the Schema where the table is located|
    |topicName|required|string|Kafka topic name|
    |key|optional|JSON object|rules for mapping from message keys to columns|
    |message|optional|JSON object|rules for mapping from messages to columns|

    In which, the mapping rules for keys and messages use the following fields for description.

    |Field|Optionality|Type|Description|
    |:----|:----------|:---|:----------|
    |dataFormat|required|string|A decoder for setting a group of columns|
    |fields|required|JSON array|Column definition list|

    fields here is a JSON array, and each element is a JSON object as follows:

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

    |Field|Optionality|Type|Description|
    |:----|:----------|:---|:----------|
    |name |required|string|column name|
    |type|required|string|column data type|
    |dataFormat|optional|string|column data decoder|
    |mapping|optional|string|decoder parameters|
    |formatHint|optional|string|hint set for the column, which can be used by the decoder|
    |hiddent|optional|boolean|whether hidden or not|
    |comment|optional|string|column description|

-   Decoder

    The function of the decoder is to map Kafka messages \(key + message\) to the columns in the data tables. Presto uses **dummy** decoder in the absence of table definition files.

    Kafka connector provides the following decoders:

    -   raw: Original bytes are directly used without conversion.
    -   csv: Messages are processed as strings in CSV format.
    -   json: Messages are processed as strings in JSON format.

