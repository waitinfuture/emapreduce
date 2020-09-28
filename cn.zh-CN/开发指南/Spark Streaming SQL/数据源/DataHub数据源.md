# DataHub数据源

本文介绍如何使用DataHub数据源进行数据分析或者交互式开发。

## 建表语法

```
CREATE TABLE tbName
USING datahub
OPTIONS(propertyName=propertyValue[,propertyName=propertyValue]*);
```

## Table Schema

创建DataHub表时，无需显式定义表的字段信息，示例如下所示。

```
spark-sql> CREATE TABLE datahub_table_test
         > USING datahub
         > OPTIONS
         > ( access.key.id = '<your access key id>',
         >   access.key.secret = '<you access key secret>',
         >   endpoint = '<your end point>',
         >   project = '<your project name>',
         >   topic = '<your topic>'
         > )

spark-sql> DESC datahub_table_test;
id  string  NULL
name string NULL
Time taken: 0.401 seconds, Fetched 2 row(s)
```

## 配置参数说明

|参数名|说明|是否必选|
|---|--|----|
|access.key.id|阿里云AccessKey ID。|是|
|access.key.secret|阿里云AccessKey Secret。|是|
|endpoint|DataHub API Endpoint。|是|
|project|DataHub项目名。|是|
|topic|DataHub的topic。|是|
|decimal.precision|当topic字段中包含decimal字段时，需要指定。|否|
|decimal.scale|当topic字段中包含decimal字段时，需要指定。|否|

