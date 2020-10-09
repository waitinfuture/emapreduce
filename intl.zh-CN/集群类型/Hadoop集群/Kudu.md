# Kudu

本文介绍如何使用Kudu。

## 典型的应用场景

-   实时计算场景
-   时间序列数据的场景
-   预测建模
-   与存量数据共存

    通常生产环境中会有大量的存量数据。数据可能存储在HDFS、RDBMS或Kudu中。您可以使用Impala访问和查询这些存量数据，无需迁移存量数据至Kudu。


## 基本架构

Kudu包含如下两种类型的组件：

-   Master Server：负责管理元数据。

    元数据包括Tablet Server的服务器的信息以及Tablet的信息，Master Server通过Raft协议提供高可用性。

-   Tablet Server：用来存储Tablets。

    每个Tablet存在多个副本，副本之间通过Raft协议提供高可用性。


Kudu集群架构图如下。

![base](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/2998197951/p63501.png)

## 创建集群

EMR-3.22.0及后续版本支持Kudu组件。您在创建Hadoop集群时勾选Kudu组件，即会创建Kudu集群，默认情况下Kudu集群包含3个Kudu Master服务并支持HA。

![create_colony](../images/p63503.jpeg)

## Impala集成Kudu

您可以在Impala配置文件中设置`kudu_master_hosts= [master1][:port],[master2][:port],[master3][:port]`，或者在SQL中添加`kudu.master_addresses`来指定Kudu集群。

本示例展示如何在SQL语句中添加`kudu.master_addresses`来指定Kudu集群。

```
CREATE TABLE my_first_table
    (
      id BIGINT,
      name STRING,
      PRIMARY KEY(id)
    )
    PARTITION BY HASH PARTITIONS 16
    STORED AS KUDU
    TBLPROPERTIES(
      'kudu.master_addresses' = 'master1:7051,master2:7051,master3:7051');
```

## 常用命令

-   查看Master节点状态。

    ```
    kudu cluster ksck <master_addresses>
    ```

    `<master_addresses>`为Master节点的内网IP地址。

-   检查集群Metrics。

    ```
    kudu-tserver --dump_metrics_json
    kudu-master --dump_metrics_json
    ```

-   展示所有表以及Tablets。

    ```
    kudu table list <master_addresses>
    ```

-   输出Cfile文件内容。

    ```
    kudu fs dump cfile <block_id>
    ```

-   输出Kudu文件系统的文件目录结构。

    ```
    kudu fs dump tree [-fs_wal_dir=<dir>] [-fs_data_dirs=<dirs>]
    ```


