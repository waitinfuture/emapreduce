# Job template {#concept_1062442 .concept}

This topic describes how to use Spark SQL to develop a streaming job.

## Query statement block {#section_osd_xsr_r8i .section}

It is difficult to properly express some job-related parameters in SQL query statements. Therefore, you need to add a SET statement before the specific SQL query statement to set required parameters. An important parameter is streaming.query.name. Each SQL query must be associated with a unique streaming.query.name. Based on this query statement, you can set other parameters \(such as checkpoint\) for each SQL query. Based on the convention, the SET streaming.query.name statement must be added to the front for each SQL query. Otherwise, an error may be returned during the query. A valid query statement block is as follows:

``` {#codeblock_iay_w6e_5rw}
SET streaming.query.name=${queryName};

queryStatement
```

## Job template {#section_uc3_3uu_4yv .section}

``` {#codeblock_i6u_riu_6dp}
-- dbName: the name of the database where a table is to be created.
CREATE DATABASE IF NOT EXISTS ${dbName};
USE ${dbName};

-- Create a Log Service table.
-- slsTableName: the name of the Log Service table.
-- logProjectName: the name of the Log Service project.
-- logStoreName: the name of the Logstore in Log Service.
-- accessKeyId: the AccessKey ID provided by Alibaba Cloud.
-- accessKeySecret: the AccessKey secret provided by Alibaba Cloud.
-- endpoint: the endpoint of the Logstore in Log Service. For more information, see [Service endpoint](../../../../intl.en-US/API Reference/Service endpoint.md#).
CREATE TABLE IF NOT EXISTS ${slsTableName}
USING loghub
OPTIONS (
sls.project = '${logProjectName}',
sls.store = '${logStoreName}',
access.key.id = '${accessKeyId}',
access.key.secret = '${accessKeySecret}',
endpoint = '${endpoint}');

-- Create an HDFS table and define the column fields in the table.
-- hdfsTableName: the name of the HDFS table.
-- location: the storage path of data. Both HDFS and OSS paths are supported.
-- Supported data formats: CSX, JSON, ORC, and Parquet. The default format is Parquet.
CREATE TABLE IF NOT EXISTS ${hdfsTableName} (col1 dataType[, col2 dataType])
USING PARQUET
LOCATION '${location}';

-- Define some parameters for running each streaming query. Such parameters include:
-- streaming.query.name: the name of the streaming query job.
-- spark.sql.streaming.checkpointLocation.${queryName}: the directory of the checkpoint for the streaming query job.
SET streaming.query.name=${queryName};
SET spark.sql.streaming.query.options.${queryName}.checkpointLocation=${checkpointLocation};
-- The following parameters are optional and can be defined as required:
-- outputMode: the output mode of the query result. Default value: append.
-- trigger: the trigger controlling the moment where the query is executed. Default value: ProcessingTime. Currently, this parameter can only be set to ProcessingTime.
-- trigger.intervalMs: the interval between query batches. Unit: milliseconds. Default value: 0.
-- SET spark.sql.streaming.query.outputMode.${queryName}=${outputMode};
SET spark.sql.streaming.query.trigger.${queryName}=ProcessingTime;
SET spark.sql.streaming.query.trigger.intervalMs.${queryName}=30;

INSERT INTO ${hdfsTableName}
SELECT col1, col2
FROM ${slsTableName}
WHERE ${condition}
```

## Parameter {#section_aq8_0wf_gcl .section}

The following table lists several key parameters.

|Parameter|Description|Default value|
|---------|-----------|-------------|
|streaming.query.name|The name of the query.|This parameter must be explicitly configured.|
|spark.sql.streaming.query.options.$\{queryName\}.checkpointLocation|The directory of the checkpoint for the streaming query job.|This parameter must be explicitly configured.|
|spark.sql.streaming.query.outputMode.$\{queryName\}|The output mode of the query result.|append|
|spark.sql.streaming.query.trigger.$\{queryName\}|The trigger controlling the moment where the query is executed. Currently, this parameter can only be set to ProcessingTime.|ProcessingTime|
|spark.sql.streaming.query.trigger.intervalMs.$\{queryName\}|The interval between query batches. Unit: milliseconds.|0|

