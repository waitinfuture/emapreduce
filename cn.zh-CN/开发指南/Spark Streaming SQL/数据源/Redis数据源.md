# Redis数据源

本文介绍如何使用Redis数据源进行数据分析或者交互式开发。

## 建表语法

```
CREATE TABLE tbName[(columnName dataType [,columnName dataType]*)]
USING redis
OPTIONS(propertyKey=propertyValue[, propertyKey=propertyValue]*);
```

## Table Schema

创建Redis表时，必须显式地定义表的字段信息，示例如下所示。

```
spark-sql> CREATE TABLE redis_test_table(`key0` STRING, `value0` STRING, `key1` STRING, `value1` STRING)
         > USING redis
         > OPTIONS(
         > table="test",
         > redis.save.mode="append",
         > model="hash",
         > filter.keys.by.type="false",
         > key.column="uuid",
         > max.pipeline.size="100",
         > host="localhost",
         > port="6379",
         > dbNum="0");
```

## 配置参数说明

|参数名|说明|是否必选|
|---|--|----|
|table|写入Redis数据的key前缀。key格式为`${table}:${key.column}`，其中`${key.column}`为配置项。|是|
|redis.save.mode|数据已经存在时的处理方式，包含append、overwrite、errorifexists或ignore，依次表示append到当前数据中、覆盖、抛出异常或丢弃数据，默认值为append。|否|
|model|数据存储格式，包含hash和binaray，默认值为hash。

|否|
|filter.keys.by.type|是否过滤不符合数据存储格式的数据，默认值为false。|否|
|key.column|用来指定key的column。不指定时默认值为uuid。|否|
|ttl|不设置数值时表示默认永久保存；设置数值即为过期时间，单位是秒。|否|
|max.pipeline.size|通过pipeline方式写入数据的数据量最大值，默认值为100。|否|
|host|Redis实例的本地服务器地址，默认值为localhost。|否|
|port|Redis服务的端口，默认值为6379。|否|
|dbNum|数据存入Redis的数据库序号，默认值为0。|否|

