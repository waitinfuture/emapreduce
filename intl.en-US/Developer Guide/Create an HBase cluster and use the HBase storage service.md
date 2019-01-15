# Create an HBase cluster and use the HBase storage service {#concept_iyr_gyn_hfb .concept}

This topic describes how to create and configure an HBase cluster and use the HBase storage service.

To make better use of HBase, we recommend that you use the following configurations when creating a cluster.

-   Enable access from the public network.

-   Select the zone where the server of the application that accesses HBase is located. Do not randomly assign a zone.

-   Select four or more hardware nodes, including the master and slave nodes. E-MapReduce creates NameNode, DataNode, JournalNode, HMaster, RegionServer, and ZooKeeper roles on these nodes.

-   Select 4-core 16 GB and 8-core 32 GB for servers. Lower configurations may impair the stability of HBase clusters.

-   Select the SSD disk type for the data disk for cost-effectiveness. You can select Basic Disk as the disk type for services that are not frequently used but require a large amount of storage space.

-   Configure the data capacity as needed.

-   HBase clusters support memory expansion.


## Configure HBase {#section_qy1_hzn_hfb .section}

You can use the [Software Configuration](../DNemapreduce1876943/EN-US_TP_17886.dita#concept_ctp_kkn_y2b) feature on the Create Cluster page when creating an HBase cluster. You can optimize and modify the default parameters of HBase based in specific scenarios. See the following example:

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

Some default configurations for HBase clusters are as follows:

|key|value|
|---|-----|
|zookeeper.session.timeout|180000|
|hbase.regionserver.global.memstore.size|0.35|
|hbase.regionserver.global.memstore.lowerLimit|0.3|
|hbase.hregion.memstore.flush.size|128MB|

## Access HBase {#section_ips_d14_hfb .section}

**Note:** 

-   Considering network performance, we recommend that you initiate access requests to HBase clusters created by E-MapReduce from ECS instances in the same zone.
-   The ECS instance accessing HBase clusters must be in the same security group as the HBase clusters. Otherwise, the access fails. Therefore, if you create a Hadoop, Spark, or Hive cluster in E-MapReduce, and the cluster you create needs to access HBase, then make sure that you select the same security group as the HBase cluster.

After you have created an HBase cluster in the E-MapReduce console, you can use the HBase storage service. The procedure is as follows:

1.  Get the master IP address and the address of the ZooKeeper cluster. On the Cluster Overview page in the E-MapReduce console, you can view the IP addresses of the master nodes and ZooKeeper access addresses \(intranet IP addresses\), as shown in the following figure.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17990/154752347813234_en-US.png)

    For more information about master nodes with public IP addresses enabled, see [How to log on to a master node](../DNemapreduce1876943/EN-US_TP_17923.dita#concept_sns_sww_y2b) and browse the HMaster WEB UI \(localhost:16010\).

2.  You can connect to the master node of a cluster over SSH using HBase Shell. You can use SSH to access the master node of a cluster. Switch to an HDFS account and use HBase Shell to access the clusters. \(For more information about HBase Shell, see the [Apache HBase website](http://hbase.apache.org/book.html?spm=a2c4g.11186623.2.17.1877a3baxItNfG#shell).\)

    ```
    [root@emr-header-1 ~]# su hdfs
    [hadoop@emr-header-1 root]$ hbase shell
    HBase Shell; enter 'help<RETURN>' for a list of supported commands.
    Type "exit<RETURN>" to leave the HBase Shell
    Version 1.1.1, r374488, Fri Aug 21 09:18:22 CST 2015
    hbase(main):001:0>
    ```

3.  Access the cluster using HBase Shell from another ECS node \(within the same security group\). Download the HBase-1.x resource from the Apache HBase website \([download link](http://www.apache.org/dyn/closer.cgi/hbase/?spm=a2c4g.11186623.2.18.1877a3baxItNfG)\). Extract the resources, modify conf/hbase-site.xml, and then add the ZooKeeper address of the cluster as follows:

    ```
    <configuration>
         <property>
             <name>hbase.zookeeper.quorum</name>
             <value>$ZK_IP1,$ZK_IP2,$ZK_IP3</value>
         </property>
    </configuration>
    ```

    Then you can access the cluster using the bin/hbase shell command.

    If the ECS instances are created using E-MapReduce, you only need to modify the hbase-site.xml file without downloading the HBase-1.x resource.

    The configuration files of versions later than 3.2.0 are located in /etc/ecm/hbase-conf/hbase-site.xml.

    The configuration files of versions earlier than 3.2.0 are located in /etc/emr/hbase-conf/hbase-site.xml.

4.  To access the HBase cluster using APIs, add the following Maven dependency:

    ```
    <groupId>org.apache.hbase</groupId>
    <artifactId>hbase-client</artifactId>
    <version>1.1.1</version>
    ```

    Configure the correct ZooKeeper address to connect to the cluster.

    ```
    Configuration config = HBaseConfiguration.create();
    config.set(HConstants.ZOOKEEPER_QUORUM,"$ZK_IP1,$ZK_IP2,$ZK_IP3");
    Connection connection = ConnectionFactory.createConnection(config);
    try {
         Table table = connection.getTable(TableName.valueOf("myLittleHBaseTable"));
         try {
             //Do table operation
         }finally {
             if (table ! = null) table.close();
         }
    } finally {
         connection.close();
    }
    ```


For more information about the Apache HBase development, see the [Apache HBase](http://hbase.apache.org/book.html?spm=a2c4g.11186623.2.19.1877a3baxItNfG#architecture.client) website.

## Examples {#section_b2v_kb4_hfb .section}

-   Prerequisites

    The ECS instance accessing the Hbase cluster must be in the same security group as the HBase cluster.

-   Allow Spark to access HBase

    See [spark-hbase-connector](https://github.com/nerdammer/spark-hbase-connector).

-   Allow Hadoop to access HBase

    See [HBase MapReduce Examples](http://hbase.apache.org/0.94/book/mapreduce.example.html#mapreduce.example.read).

-   Allow Hive to access HBase

    Only Hive running in clusters with a version of E-MapReduce 1.2.0 or later can access the HBase cluster. Follow these steps:

    1.  Log on to the Hive cluster, and modify hosts by adding the following line:

        ```
        $zk_ip emr-cluster //$zk_ip is the IP address of the ZooKeeper node in the HBase cluster.
        ```

    2.  For more information about how to operate Hive, see [Hive HBase Integration](https://cwiki.apache.org/confluence/display/Hive/HBaseIntegration).

