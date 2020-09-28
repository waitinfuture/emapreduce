# Loghub数据源

本文介绍如何使用Loghub数据源进行数据分析或者交互式开发。

## 建表语法

```
CREATE TABLE tbName(columnName dataType [,columnName dataType]*)
USING loghub
OPTIONS(propertyName=propertyValue[,propertyName=propertyValue]*);
```

## Table Schema

创建Loghub表时，必须显式地定义表的字段信息。自定义Scheam字段名称要和SLS日志中key的名称相同。

```
spark-sql> CREATE TABLE loghub_table_test(content string)
         > USING loghub
         > OPTIONS(
         > endpoint="sls.aliyuncs.com",
         > access.key.id="yHiu*******BG2s",
         > access.key.secret="ABctuw0M***************iKKljZy",
         > sls.project="test",
         > sls.store="myInstance");

spark-sql> DESC loghub_table_test;
content  string  NULL
Time taken: 0.436 seconds, Fetched 1 row(s)
```

不带Schema时如下。

```
spark-sql> CREATE TABLE loghub_table_test
         > USING loghub
         > OPTIONS
         > (...)

spark-sql> DESC loghub_table_test;
__logProject__ string  NULL
__logStore__ string NULL
__shard__ string NULL
__time__ string NULL
__topic__ string NULL
__source__ string NULL
__value__ string NULL
__sequence_number__ string NULL
Time taken: 0.436 seconds, Fetched 1 row(s)
```

## 配置参数说明

|参数名|说明|是否必选|
|---|--|----|
|endpoint|LogService API Endpoint。|是|
|access.key.id|阿里云AccessKeyId。|是|
|access.key.secret|阿里云AccessKeySecret。|是|
|sls.store|LogService项目名。|是|
|sls.project|LogStore名。|是|

对于EMR SDK 2.0.0以及以上版本，Loghub Schema为可选项，参数说明如下。

|参数|类型|说明|
|--|--|--|
|\_\_logProject\_\_|String|LogStore名。|
|\_\_logStore\_\_|String|LogService项目名。|
|\_\_shard\_\_|String|Logstore的Shard。|
|\_\_time\_\_|String|该记录日志时间。|
|\_\_topic\_\_|String|日志主题。|
|\_\_source\_\_|String|日志服务的来源IP。|
|\_\_value\_\_|String|日志服务的内容。JSON格式。|
|\_\_sequence\_number\_\_|String|该记录序列号（通过appendSequenceNumber='true'开启改字段，默认为NULL\)。|

