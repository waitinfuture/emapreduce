# 访问HBase

本文介绍如何配置HBase集群以及HBase存储服务使用流程。

已创建集群，并添加HBase服务，详情请参见[创建集群](/intl.zh-CN/集群管理/集群配置/创建集群.md)。

## HBase配置

您可以在创建HBase集群的**软件配置**页面，利用**高级设置**的**软件自定义配置**功能，结合使用场景，修改HBase的默认参数，示例如下。

```
{
  "configurations": [
    {
      "classification": "hbase-site",
      "properties": {
        "hbase.hregion.memstore.flush.size": "268435456",
        "hbase.regionserver.global.memstore.size": "0.5",
        "hbase.regionserver.global.memstore.lowerLimit": "0.6"
      }
    }
  ]
}
```

HBase集群部分默认配置如下所示。

|key|value|
|---|-----|
|zookeeper.session.timeout|180000|
|hbase.regionserver.global.memstore.size|0.35|
|hbase.regionserver.global.memstore.lowerLimit|0.3|
|hbase.hregion.memstore.flush.size|128MB|

## 访问HBase

1.  通过SSH方式登录集群。

    详情请参见[使用SSH连接主节点](/intl.zh-CN/集群管理/集群配置/连接集群/使用SSH连接主节点.md)。

2.  执行如下命令，进入Hbase Shell。

    ```
    hbase shell
    ```

    返回如下信息。

    ```
    SLF4J: Class path contains multiple SLF4J bindings.
    SLF4J: Found binding in [jar:file:/opt/apps/ecm/service/hbase/1.4.9-1.0.0/package/hbase-1.4.9-1.0.0/lib/slf4j-log4j12-1.7.10.jar!/org/slf4j/impl/StaticLoggerBinder.class]
    SLF4J: Found binding in [jar:file:/opt/apps/ecm/service/hadoop/2.8.5-1.5.3/package/hadoop-2.8.5-1.5.3/share/hadoop/common/lib/slf4j-log4j12-1.7.10.jar!/org/slf4j/impl/StaticLoggerBinder.class]
    SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
    SLF4J: Actual binding is of type [org.slf4j.impl.Log4jLoggerFactory]
    HBase Shell
    Use "help" to get list of supported commands.
    Use "exit" to quit this interactive shell.
    Version 1.4.9, r8214a16c5d80f077abf1aa01bb312851511a2b15, Thu Jan 31 20:35:22 CST 2019
    
    hbase(main):001:0>
    ```

    **说明：** 如果您是通过ECS控制台创建的实例，请参见[使用HBase Shell](https://www.alibabacloud.com/help/zh/doc-detail/52056.htm?spm=a2c63.l28256.b99.52.60665f1foCzjSC)。


## 示例

-   Spark访问Hbase

    详情请参见[spark-hbase-connector](https://github.com/nerdammer/spark-hbase-connector)。

-   Hadoop访问Hbase

    详情请参见[HBase MapReduce Examples](http://hbase.apache.org/0.94/book/mapreduce.example.html#mapreduce.example.read)。

-   Hive访问HBase
    1.  登录Hive集群主节点，修改hosts文件，增加如下内容。

        ```
        $zk_ip emr-cluster //$zk_ip为Hbase集群的Zookeeper节点IP。
        ```

    2.  执行Hive操作，详情请参见 [Hive HBase Integration](https://cwiki.apache.org/confluence/display/Hive/HBaseIntegration)。

