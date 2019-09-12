# Streaming query configuration {#concept_1322727 .concept}

This topic describes basic concepts and related parameters of streaming query configuration.

## Query configuration {#section_c2r_pos_8o1 .section}

Before using Spark SQL for a streaming query, you need to know the following two concepts:

-   Data source configuration: the definition of a table.
-   Query instance configuration: the parameter settings for running each streaming query.

The definition of a table only contains data source configurations, such as the connection address of the Kafka data source and the topic name. You can perform multiple non-business queries simultaneously in a table. Therefore, the definition of a table must not contain the configurations for running specific query instances.

Each query instance must be configured separately. To reduce unnecessary modifications to the query SQL statements, you can set a query name for each query instance. By using the query name, you can set parameters for running each specific query instance. The parameters of query instances are configured using the `SET` syntax. For more information, see [Configuration parameters](#section_al9_70k_7qk).

Conventions regarding query configuration: The query name for each query instance is available in the nearest SQL SET statement. Here are two examples:

-   Case 1

    ``` {#codeblock_cfx_4sx_amx}
    SET streaming.query.name=one_test_job
    
    -- query 1
    INSERT INO tb_test_1 SELECT ...
    
    -- query 2
    INSERT INO tb_test_2 SELECT ...
    
    -- The names of queries 1 and 2 are both one_test_job. However, this case is invalid because the name of each query instance must be unique.
    ```

-   Case 2

    ``` {#codeblock_kqz_ih2_3o9}
    SET streaming.query.name=one_test_job_1
    
    SET streaming.query.name=one_test_job_2
    
    -- query 1
    CREATE TABLE tb_test_1 AS SELECT ...
    
    -- The name of query 1 is one_test_job_2.
    ```


The statements for query instances include:

-   `INSERT INTO ...`
-   `CREATE TABLE ... AS SELECT ...`

## Configuration parameters {#section_al9_70k_7qk .section}

|Parameter|Corresponding DataFrame API|SQL statement format|Description|Required|
|---------|---------------------------|--------------------|-----------|--------|
|queryName|writeStream.queryName\(...\)|SET streaming.query.name=$queryName|The name of each streaming query. The parameters of different query instances are distinguished by the query name.|Yes|
|option|writeStream.option\(...\)|SET spark.sql.streaming.query.options.$queryName.$optionName=$optionValue|checkpointLocation: the directory of the checkpoint.|Yes|
|The custom option.|No|
|outputMode|writeStream.outputMode\(...\)|SET spark.sql.streaming.query.outputMode.$queryName=$outputMode|The output mode of the query result. Default value: append.|No|
|trigger|writeStream.trigger\(...\)|SET spark.sql.streaming.query.trigger.$queryName=$triggerType|The trigger controlling the moment where the query is executed. Default value: ProcessingTime. **Note:** Currently, this parameter can only be set to ProcessingTime.

 |No|
|SET spark.sql.streaming.query.trigger.intervalMs.$queryName=$intervalMs|The interval between query batches. Unit: milliseconds. Default value: 0.|No|

