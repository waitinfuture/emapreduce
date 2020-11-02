# Common keywords

This topic introduces common keywords used in Spark Streaming SQL and describes how to use these keywords.

## Background information

EMR V3.21.0 and later provide a preview release of Spark Streaming SQL for you to develop streaming analytics jobs.

Spark Streaming SQL is developed based on Spark Structured Streaming. All syntax functions and limits are compliant with Spark Structured Streaming.

## Common keywords

|Keyword type|Keyword|
|------------|-------|
|DDL|CREATE TABLE, CREATE TABLE AS SELECT, CREATE SCAN, CREATE STREAM|
|DML|INSERT INTO, MERGE INTO|
|SELECT clause|SELECT FROM, WHERE, GROUP BY, JOIN, UNION ALL|

## Use keywords as field names

If you want to use a keyword as a field name, enclose the keyword in backticks \(\`\) in the format of ``value``.

