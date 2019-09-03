# Redis 数据表使用说明 {#concept_1958337 .concept}

本文为您介绍如何使用Redis数据表。

## 建表语法 {#section_gle_jfz_47b .section}

``` {#codeblock_wjc_e13_trk}
CREATE TABLE tbName[(columnName dataType [,columnName dataType]*)]
USING redis
OPTIONS(propertyKey=propertyValue[, propertyKey=propertyValue]*);
```

## 配置参数说明 {#section_zer_2wv_0zt_1 .section}

|参数名|说明|是否必选|
|---|--|----|
|table| 写入redis的数据的key前缀。key格式为`${table}:${key.column}`，`${key.column}`为配置项key.column。

 |是|
|redis.save.mode|表示遇到数据已经存在时的处理方式，可以是append/overwrite/errorifexists/ignore，分别代表append到当前数据中/覆盖/抛出异常/丢弃数据，默认值为append。|否|
|model| 数据存储格式，值为hash/binaray，默认值为hash。

 |否|
|filter.keys.by.type|是否过滤不符合model的数据，默认值为false。|否|
|key.column|使用row的一列作为存入redis的key，默认key的生成方式为uuid。|否|
|ttl|不设置数值默认永久保存，设置数值即为过期时间，单位是秒。|否|
|max.pipeline.size| pipeline方式写入数据的数据量最大值，默认值为100。

 |否|
|host|Redis服务的host，默认值为localhot。|否|
|port|Redis服务的port，默认值为6379。|是|
|dbNum|数据存入Redis的dbNum，默认值为0。|否|

## Table Schema {#section_n9b_11b_3km_1 .section}

创建Redis表时，必须 显式地定义表的字段信息 ，例如：

``` {#codeblock_ly5_005_3d2}
spark-sql> CREATE TABLE redis_test_table(`key0` STRING, `value0` STRING, `key1` STRING, `value1` STRING)
         > USING redis
         > OPTIONS(
         > table="test",
         > redis.save.mode="append",
         > model="hash",
         > filter.keys.by.type="false",
         > key.column="uuid",
         > max.pipeline.size="100",
         > host="localhot",
         > port="6379",
         > dbNum="0");

			
```

