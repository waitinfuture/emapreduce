# HBase 开发手册 {#concept_iyr_gyn_hfb .concept}

本文介绍如何配置一个 HBase 集群以及 HBase 存储服务使用流程。

为了更好地使用 HBase，在创建集群过程中，推荐您使用如下配置:

-   公网状态选择打开。

-   可用区选择为访问 HBase 的应用服务器所在的可用区，请勿选择随机分配。

-   硬件节点数请选择 4 个及以上，其包含了 Master 和 Slave 节点，E-MapReduce 会在这些节点上创建 namenode、datanode、journalnode、hmaster、regionserver 和 zookeeper 角色。

-   服务器配置推荐选择 4 核 16G / 8 核 32G 这两款机型，过低的配置可能导致 HBase 集群无法稳定运行。

-   数据盘类型建议选择 SSD 云盘，会有更好的成本性价比。对于访问少，存储量大的业务，可以选择普通云盘。

-   数据容量请按实际需求配置。

-   HBase 集群支持扩容。


## HBase 配置 {#section_qy1_hzn_hfb .section}

创建 HBase 集群的时候，在创建页面可以利用[软件配置](../../../../../intl.zh-CN/用户指南/软件配置.md#)功能，结合使用场景，对 HBase 的默认参数配置做一些优化修改，如下所示：

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

HBase 集群一些默认的配置如下所示：

|key|value|
|---|-----|
|zookeeper.session.timeout|180000|
|hbase.regionserver.global.memstore.size|0.35|
|hbase.regionserver.global.memstore.lowerLimit|0.3|
|hbase.hregion.memstore.flush.size|128MB|

## 访问 HBase {#section_ips_d14_hfb .section}

**说明：** 

-   由于网络性能的考虑，通过 E-MapReduce 创建的 HBase 集群，我们只建议从同个可用区的 ECS 上发起访问。
-   访问 HBase 集群的 ECS 必须和 HBase 集群处于同一个安全组内，否则无法访问，所以在 EMR 中创建 Hadoop/Spark/Hive 集群时，如果它们需要访问 HBase，则在创建集群的时候一定要选择和 HBase 集群相同的安全组。

通过 E-MapReduce 控制台创建完 HBase 集群以后，用户可以开始使用 HBase 存储服务。其操作步骤如下：

1.  获取 Master IP 和集群 ZK 地址。通过 E-MapReduce 控制台集群详情页面，用户可以查看集群的 Master 节点 IP 和 ZK 访问地址（内网IP），如下图所示：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17990/155143000313234_zh-CN.png)

    对于开通了公网 IP 的 Master 节点，用户可以参考[如何登录 master 节点](../../../../../intl.zh-CN/用户指南/SSH 登录集群.md#)，浏览 HMaster 的 WEB UI\(localhost:16010\)。

2.  SSH 连接到集群主节点，使用 HBase Shell。用户可以直接通过 SSH 连接到集群的 Master 节点，切换到 HDFS 用户，通过 HBase Shell 访问集群（关于 HBase Shell 的更多介绍，请参考[Apache HBase 官网](http://hbase.apache.org/book.html?spm=a2c4g.11186623.2.17.1877a3baxItNfG#shell)）。

    ```
    [root@emr-header-1 ~]# su hdfs
    [hadoop@emr-header-1 root]$ hbase shell
    HBase Shell; enter 'help<RETURN>' for list of supported commands.
    Type "exit<RETURN>" to leave the HBase Shell
    Version 1.1.1, r374488, Fri Aug 21 09:18:22 CST 2015
    hbase(main):001:0>
    ```

3.  从其他 ECS 节点\(同一个安全组内\)，使用 HBase Shell 访问集群。从 Apache HBase 的官网下载 HBase-1.x 版本的资源包（[下载链接](http://www.apache.org/dyn/closer.cgi/hbase/?spm=a2c4g.11186623.2.18.1877a3baxItNfG)），解压后，修改 conf/hbase-site.xml，添加集群的 ZK 地址，如下所示：

    ```
    <configuration>
         <property>
             <name>hbase.zookeeper.quorum</name>
             <value>$ZK_IP1,$ZK_IP2,$ZK_IP3</value>
         </property>
    </configuration>
    ```

    然后就可以通过命令bin/hbase shell 访问集群了。

    若 ECS 是通过 EMR 创建的,则只需修改 hbase-site.xml，无需下载 HBase-1.x 版本的资源包。

    3.2.0及以上版本配置位于： /etc/ecm/hbase-conf/hbase-site.xml

    3.2.0以下版本位于：/etc/emr/hbase-conf/hbase-site.xml

4.  通过 API 访问 HBase 集群，引入 Maven 依赖。

    ```
    <groupId>org.apache.hbase</groupId>
    <artifactId>hbase-client</artifactId>
    <version>1.1.1</version>
    ```

    配置正确的 ZK 地址，连接集群。

    ```
    Configuration config = HBaseConfiguration.create();
    config.set(HConstants.ZOOKEEPER_QUORUM,"$ZK_IP1,$ZK_IP2,$ZK_IP3");
    Connection connection = ConnectionFactory.createConnection(config);
    try {
         Table table = connection.getTable(TableName.valueOf("myLittleHBaseTable"));
         try {
             //Do table operation
         }finally {
             if (table != null) table.close();
         }
    } finally {
         connection.close();
    }
    ```


更多开发介绍，请参考 [Apache HBase 官网](http://hbase.apache.org/book.html?spm=a2c4g.11186623.2.19.1877a3baxItNfG#architecture.client)。

## 示例 {#section_b2v_kb4_hfb .section}

-   前提条件

    访问 HBase 集群的 ECS 必须和 HBase 集群处于同一个安全组内。

-   Spark 访问 Hbase

    请参照 [spark-hbase-connector](https://github.com/nerdammer/spark-hbase-connector)。

-   Hadoop 访问 Hbase

    请参照 [HBase MapReduce Examples](http://hbase.apache.org/0.94/book/mapreduce.example.html#mapreduce.example.read)。

-   Hive 访问 HBase

    EMR 1.2.0 及以上主版本的集群中的 Hive 才能访问 Hbase 集群。其步骤如下：

    1.  登录 Hive 集群，修改 hosts，增加如下一行:

        ```
        $zk_ip emr-cluster //$zk_i p为 Hbase 集群的 zk 节点 IP
        ```

    2.  具体 Hive 操作请参照 [Hive HBase Integration](https://cwiki.apache.org/confluence/display/Hive/HBaseIntegration)。

