# Connect to ApsaraDB for HBase using E-MapReduce Hive {#concept_ygm_vfw_mgb .concept}

This topic describes how to connect E-MapReduce Hive and ApsaraDB for HBase. The analysis of HBase tables is based on the connection between Hive and ApsaraDB for HBase.

**Note:** ApsaraDB for HBase will be integrated into Spark. We recommend that you use Spark to analyze HBase data at that time.

## Preparations {#section_cd2_tgw_mgb .section}

-   Purchase a Pay-As-You-Go EMR cluster and create configurations based on the actual scenarios. Note: Make sure ApsaraDB for HBase and the EMR cluster are in the same VPC. We recommend that you do not enable High Availability for the cluster.
-   Add the IP addresses of all nodes in the EMR cluster to the whitelist of ApsaraDB for HBase.
-   You can view the endpoint of ZooKeeper that is built in Hive in the ApsaraDB for HBase console.
-   You need to contact the Alibaba Cloud team to open the HDFS ports of an ApsaraDB for HBase for you.

## Procedures {#section_pf2_l3w_mgb .section}

1.  Modify Hive configurations
    -   Go to the Hive configuration directory /etc/ecm/hive-conf/.
    -   Modify the hbase-site.xml file by setting the value of the hbase.zookeeper.quorum property to the endpoint of ZooKeeper that is built in HBase.

        ```
        <property>
                   <name>hbase.zookeeper.quorum</name> 
                   <value>hb-bp1mhyea7754bpigt-001.hbase.rds.aliyuncs.com,hb-bp1mhyea7754bpigt-002.hbase.rds.aliyuncs.com,hb-bp1mhyea7754bpigt-003.hbase.rds.aliyuncs.com</value> 
              </property>
        ```

2.  Connect to an HBase table using a Hive table

    Create a table in Hive using the HBase handler. By doing this, the same table is created in ApsaraDB for HBase as well.

    1.  Start the Hive command-line interface \(CLI\).

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/114385/155860457937680_en-US.png)

    2.  Use the following statement to create a table in Hive.

        ```
        CREATE TABLE hive_hbase_table(key int, value string)
        ```

        ```
        STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
        WITH SERDEPROPERTIES ("hbase.columns.mapping" = ":key,cf1:val")
        TBLPROPERTIES ("hbase.table.name" = "hive_hbase_table", "hbase.mapred.output.outputtable" = "hive_hbase_table");
        ```

    3.  Insert data to the HBase table in Hive.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/114385/155860457937681_en-US.png)

    4.  Verify that the HBase table has been created and the data has been inserted to the table.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/114385/155860457937682_en-US.png)

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/114385/155860457937683_en-US.png)

    5.  Write data to the HBase table using the put command.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/114385/155860457937684_en-US.png)

        Select all data from the table in Hive.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/114385/155860457937685_en-US.png)

    6.  Delete the table in Hive using the drop command. The table in HBase is deleted as well, which is to be verified in the subsequent step.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/114385/155860457937686_en-US.png)

        View the contents on the table in HBase using the scan command. An error message appears showing the table does not exist.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/114385/155860457937687_en-US.png)

        **Note:** 

        Existing HBase tables can be connected using the Hive external tables. Deleting a Hive external table does not cause the deletion of the corresponding HBase table.

    7.  Create a table in ApsaraDB for HBase and write test data to the table using the put command.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/114385/155860457937688_en-US.png)

    8.  Create a Hive external table to connect to an HBase table and select all data from the HBase table.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/114385/155860457937689_en-US.png)

    9.  Verify that deleting the Hive external table does not cause the deletion of the corresponding HBase table.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/114385/155860457937690_en-US.png)

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/114385/155860457937691_en-US.png)


## Summary {#section_f2n_3qw_mgb .section}

For more operations on HBase using Hive, see [HBase Integration](https://cwiki.apache.org/confluence/display/Hive/HBaseIntegration). The operations in this topic are based on Hive installed on an Alibaba Cloud EMR cluster. Operations based on Hive installed on a custom MapReduce cluster of ECS instances are similar. Note: Configuration items in the configuration file hbase-site.xml of Hive may be different from those of ApsaraDB for HBase. You only need to configure the hbase.zookeeper.quorum property for connecting to ApsaraDB for HBase using Hive.

## References {#section_mlh_jnl_ngb .section}

Alibaba Cloud Community: [Use E-MapReduce Hive to integrate with ApsaraDB for HBase](https://yq.aliyun.com/articles/652464?spm=a2c4e.11163080.searchblog.9.26032ec10hUrSR&do=login&accounttraceid=43f5cf8e-7606-472e-a12f-b5d694893b58&do=login) 

