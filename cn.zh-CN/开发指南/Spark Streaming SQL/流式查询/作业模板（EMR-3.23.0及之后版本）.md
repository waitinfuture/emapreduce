# 作业模板（EMR-3.23.0及之后版本）

本文为您介绍如何在EMR-3.23.0及之后版本中，使用Spark SQL开发一个流式处理作业。

## 作业模板

```
-- dbName：数据库名。
CREATE DATABASE IF NOT EXISTS ${dbName};
USE ${dbName};

-- 创建SLS数据表。
-- slsTableName：SLS表的名称。
-- logProjectName：LogService的项目名。
-- logStoreName：LogService的logstore名。
-- accessKeyId：阿里云AccessKeyId。
-- accessKeySecret：阿里云AccessKeySecret。
-- endpoint：LogService的logstore所在的Endpoint。
-- 当显式指定LogStore的各个字段时，必须定义为STRING类型
-- 保留6个系统字段：（`__logProject__` STRING, `__logStore__` STRING, `__shard__` INT, `__time__` TIMESTAMP, `__topic__` STRING, `__source__` STRING)
CREATE TABLE IF NOT EXISTS ${slsTableName} (col1 dataType[, col2 dataType])。
USING loghub
OPTIONS (
sls.project = '${logProjectName}',
sls.store = '${logStoreName}',
access.key.id = '${accessKeyId}',
access.key.secret = '${accessKeySecret}',
endpoint = '${endpoint}');

-- 创建HDFS数据表，需要完成表的列字段定义。
-- hdfsTableName：HDFS表的名称。
-- location: 存储数据路径，支持HDFS和OSS路径。
-- 数据格式支持：delta, csv, json, orc, parquet等，默认为delta。
CREATE TABLE IF NOT EXISTS ${hdfsTableName} (col1 dataType[, col2 dataType])
USING delta
LOCATION '${location}';

-- 配置读表的方式，支持STREAM和BATCH，默认为BATCH。
CREATE SCAN tmp_read_sls_table 
ON ${slsTableName} 
USING STREAM;

-- 创建一个流式查询作业。
CREATE STREAM ${queryName}
OPTIONS(
outputMode='Append',
triggerType='ProcessingTime',
triggerInterval='30000',
checkpointLocation='${checkpointLocation}')
INSERT INTO ${hdfsTableName}
SELECT col1, col2
FROM tmp_read_sls_table
WHERE ${condition};
```

**说明：** endpoint详细信息请参见[访问域名和数据中心](/cn.zh-CN/开发指南/访问域名（Endpoint）/访问域名和数据中心.md)。

