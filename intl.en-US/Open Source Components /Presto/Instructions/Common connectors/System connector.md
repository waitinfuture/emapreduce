# System connector {#concept_vvs_kpr_zgb .concept}

## Overview {#section_qhz_kpr_zgb .section}

The system connector is used to query the basic information and measurements of the Presto cluster through SQL statements.

## Configuration {#section_rhz_kpr_zgb .section}

All information can be obtained through a catalog called `system` without configuration.

Examples:

```
--- List all the supported data entries
SHOW SCHEMAS FROM system;

--- List all the data entries in a project during runtime
SHOW TABLES FROM system.runtime;

--- Obtain the node status
SELECT * FROM system.runtime.nodes;

     node_id |         http_uri         | node_version | coordinator | state
-------------+--------------------------+--------------+-------------+--------
 3d7e8095-...| http://192.168.1.100:9090| 0.188        | false       | active
 7868d742-...| http://192.168.1.101:9090| 0.188        | false       | active
 7c51b0c1-...| http://192.168.1.102:9090| 0.188        | true        | active 

--- Cancel a query
CALL system.runtime.kill_query('20151207_215727_00146_tx3nr');

```

## Data tables {#section_shz_kpr_zgb .section}

The system connector provides the following data tables:

|TABLE|SCHEMA|Description|
|-----|------|-----------|
|catalogs|metadata|This table lists all the catalogs that are supported by the system connector.|
|schema\_properties|metadata|This table lists the available properties that can be set when you create a schema.|
|table\_properties|metadata|This table lists the available properties that can be set when you create a table.|
|nodes|runtime|This table lists all the visible nodes of the Presto cluster and the node status.|
|queries|runtime|This table contains the information about the queries currently and recently initiated in the Presto cluster, including the raw query text \(SQL\), the identities of the users who initiate the queries, and information about query performance, such as the query queue and analysis time.|
|tasks|runtime|This table contains the information about the tasks that are involved in the queries in the Presto cluster, including the location of task execution and the number of lines and bytes processed by each task.|
|transactions|runtime|This table lists the currently opened transactions and their related metadata. The metadata includes the creation time, idle time, initialization parameters, and access catalogs.|

## Stored procedure {#section_yhz_kpr_zgb .section}

The system connector supports the following stored procedure:

-   `runtime.kill_query(id)`

    It cancels the query with the specified ID.


