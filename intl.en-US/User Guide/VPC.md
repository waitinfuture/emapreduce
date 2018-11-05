# VPC {#concept_gh4_qln_y2b .concept}

Virtual Private Cloud \(VPC\) creates an isolated network environment for users. You can select an IP address range, divide networks, and configure the routing list and gateway.

For more information, see [What is VPC](../../../../intl.en-US/Product Introduction/What is VPC?.md#). The interflow of VPC intranet and between VPC and physical IDC machine rooms can be realized among regions or users through [Express Connect](https://www.alibabacloud.com/product/express-connect).

## Create a VPC cluster {#section_z2j_5t5_y2b .section}

E-MapReduce can select the type of network during cluster creation, classic network or VPC. For VPC, the following operations are required:

-   Subordinate VPC: Select a VPC where the current E-MapReduce cluster is located. If no creation is made, log on to [VPC console](https://vpc.console.aliyun.com/#/) to create a VPC.
-   VSwitch: ECS instance in E-MapReduce cluster communicates through VSwitch. If no creation is made, log on to [VPC console](https://vpc.console.aliyun.com/#/) to create a VPC. The VSwitch has the property of availability zone. Therefore, the created VSwitch must also belong to the availability zone selected during cluster creation in E-MapReduce.
-   Safety group creation: Once enabled, enter the name of the created security group.
-   Owner security group: The security group the cluster belongs to. The security group of a classic network cannot be used in VPC. The security group of VPC can only be used in current VPC. Here only the security group created in E-MapReduce product by the user is shown. For safety reasons, a security group created outside the E-MapReduce cannot be selected. To create a security group, select New security group and enter the name of security group.

## Example {#section_pkm_xt5_y2b .section}

E-MapReduce cluster communication in different VPCs \(Hive visits HBase\)

1.  Create clusters.

    Create two clusters in E-MapReduce. Hive Cluster C1 is located in VPC1 while HBase Cluster C2 is located in VPC2. Both clusters are located in the cn-hangzhou region.

2.  Configure the high-speed channel.

    See [Establish an intranet connection between VPCs under the same account](../../../../intl.en-US/Tutorials/Establish an intranet connection between VPCs under the same account.md#). Select the same region.

3.  Log on to HBase cluster through SSH and a table is created through HBase Shell.

    ```
    hbase(main):001:0> create 'testfromHbase','cf'
    ```

4.  Log on to Hive through SSH.
    1.  Change hosts and add a line as follows.

        ```
        $zk_ip emr-cluster //$zk_ip is the zk node IP of Hbase cluster.
        ```

    2.  Visit HBase through Hive Shell.

        ```
        hive> set hbase.zookeeper.quorum=172.16.126.111,172.16.126.112,172.16.126.113;
        hive> CREATE EXTERNAL TABLE IF NOT EXISTS testfromHive (rowkey STRING, pageviews Int, bytes STRING) STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler' WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,cf:c1,cf:c2') TBLPROPERTIES ('hbase.table.name' = 'testfromHbase');
        ```

        The abnormality of java.net.SocketTimeoutException is reported here because the security group where the ECS of HBase cluster is located limits the E-MapReduce visit at relevant ports. Port 22 is only opened by default in the security group created by E-MapReduce. Therefore, the security group rules need to be added into the security group of HBase cluster to open a port for Hive cluster, as shown in the following figure.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17888/154138490610585_en-US.png)


