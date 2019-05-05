# VPC {#concept_gh4_qln_y2b .concept}

Virtual Private Cloud \(VPC\) helps you build an isolated network environment, including customizing the IP address range, network segment, routing table, and gateway.

For more information, see [What is VPC](../../../../reseller.en-US/Product Introduction/What is VPC?.md#). VPC can be interconnected with physical IDC equipment rooms using [Express Connect](https://partners-intl.console.aliyun.com/#/express-connect).

## Create a VPC cluster {#section_z2j_5t5_y2b .section}

When you create a cluster in E-MapReduce, you can select from two types of network: classic and VPC. If you select VPC, complete the following operations:

-   Subordinate VPC: Select a VPC where the current E-MapReduce cluster is located. If you have not yet created a VPC, log on to the [VPC console](https://partners-intl.console.aliyun.com/#/vpc) and create one.
-   VSwitch: An ECS instance in the E-MapReduce cluster communicates through a VSwitch. If you have not yet created a VSwitch, log on to the [VPC console](https://partners-intl.console.aliyun.com/#/vpc) and create one. Because a VSwitch has the properties of an availability zone, when you create a cluster in E-MapReduce, the VSwitch you create must also belong to the availability zone selected.
-   Security group: The security group the cluster belongs to. Currently, only the security group of a VPC can be used, not the security group of a classic network. To ensure security, a security group created outside of E-MapReduce cannot be selected. Enter a security group name to create a security group.

## Example {#section_pkm_xt5_y2b .section}

The following example shows how to enable Hive to access HBase clusters in E-MapReduce in different VPCs.

1.  Create clusters.

    Create two clusters in E-MapReduce. Hive cluster C1 is located in VPC1, whereas HBase cluster C2 is located in VPC2. Both clusters are located in the cn-hangzhou region.

2.  Configure the high-speed channel.

    For more information, see [Establish an intranet connection between VPCs under the same account](../../../../reseller.en-US/Getting Started (New Console)/Interconnect two VPCs under the same account.md#). Select the same region.

3.  Log on to the HBase cluster through SSH. Create a table through HBase Shell.

    ```
    hbase(main):001:0> create 'testfromHbase','cf'
    ```

4.  Log on to Hive through SSH.
    1.  Modify the hosts and add the following line:

        ```
        $zk_ip emr-cluster //$zk_ip is the zk node IP of Hbase cluster.
        ```

    2.  Access HBase through Hive Shell.

        ```
        hive> set hbase.zookeeper.quorum=172.16.126.111,172.16.126.112,172.16.126.113;
        hive> CREATE EXTERNAL TABLE IF NOT EXISTS testfromHive (rowkey STRING, pageviews Int, bytes STRING) STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler' WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,cf:c1,cf:c2') TBLPROPERTIES ('hbase.table.name' = 'testfromHbase');
        ```

        At this point, the java.net.SocketTimeoutException exception is reported. This is because the security group where the HBase cluster's ECS is located limits access to E-MapReduce at the related port. By default, security groups created by E-MapReduce only open port 22. Therefore, a security group rule must be added to the HBase cluster's security group so as to open a port for the Hive cluster, as shown in the following figure.

        ![Authorization policy list](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17888/155704122110585_en-US.png)


