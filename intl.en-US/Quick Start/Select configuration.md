# Select configuration {#concept_h5h_vjp_y2b .concept}

To use E-MapReduce \(EMR\), you must select appropriate Hadoop clusters. When selecting configurations for EMR, consider the use of big data in your enterprise, estimate the amount of data, the reliability of services, and your financial budget.

## Big data scenarios {#section_uvz_zjp_y2b .section}

EMR has been applied in the following enterprise big data scenarios:

-   Batch operations. This scenario requires high disk I/O throughput and high network throughput, but has low real-time capacity requirements. If you need to process large amounts of data but do not require real-time processing, you can use MapReduce, Pig, and Spark. This scenario does not require a high memory capacity. Therefore, you need to focus on the CPU, memory, and network requirements when you perform data shuffling.
-   Ad hoc queries. Data scientists and data analysts use ad hoc queries to retrieve data. This scenario requires real-time queries, high disk I/O throughput, and network throughput. In this scenario you can use Impala and Presto. This scenario also has high memory requirements. You need to consider the amount of data and concurrent queries.
-   Stream computing, high network throughput, and compute-intensive scenarios. For these scenarios, you can use Flink, Spark Streaming, and Storm.
-   Message queues. This scenario requires high disk I/O throughput and network throughput, consumes large amounts of memory, and the storage does not depend on HDFS. Therefore, you can choose to use Kafka clusters in EMR. EMR clusters are divided into Kafka clusters and Hadoop clusters to avoid impacting Hadoop.
-   Cold backup. This scenario does not require high disk I/O throughput or computing throughput and is low cost. We recommend that you use EMR d1 instances for cold backups. The storage cost of d1 instance is 0.03 USD/month/GB.

## EMR instances {#section_wvz_zjp_y2b .section}

An EMR cluster consists of [three types of instances](../../../../../reseller.en-US/User Guide/Configure clusters/Instance types.md#): master instances, core instances, and task instances.

You can select ultra disks, SSD disks, and local disks for EMR storage. Performance of different disks is: SSD disks \> local disks \> ultra disks.

EMR underlying storage supports OSS \(standard OSS only\) and HDFS. OSS has a higher data availability than HDFS. The data availability of OSS is 99.99999999%, while the data availability of HDFS depends on the reliability of cloud disk or local disk storage.

Storage prices are as follows:

-   Instance with local disks: USD 0.003/GB/month.
-   OSS standard storage: 0.02 USD/GB/month.
-   Ultra disk storage: 0.05 USD/GB/month.
-   SSD disk storage: 0.143 USD/GB/month.

## Select EMR configuration {#section_yvz_zjp_y2b .section}

-   Select master instance configuration

    -   Master instances are used to deploy the master processes of Hadoop, such as NameNode and ResourceManager.

    -   We recommend that you enable high availability on production clusters. High availability is available for EMR components such as HDFS, YARN, Hive, and HBase. We recommend that you enable high availability in the "Cluster configuration" step when you create the cluster. If high availability is not enabled when you create an EMR cluster, it cannot be enabled later.

    -   Master instances are used to store HDFS metadata and component log files. They are compute-intensive with low disk I/O requirements. HDFS metadata is stored in memory. The minimum recommended memory size is 16 GB based on the number of files.

-   Select core instance configuration

    -   Core instances are used to store data, run computing tasks and processes such as DataNode and NodeManager.

    -   If the amount of data stored in HDFS \(3 backups\) exceeds 60 TB, we recommend that you use instances with local disks \(ECS d1instances and ECS d1ne instances\). The local disk capacity is calculated as follows: \(the number of CPU cores/2\) \* 5.5 TB \* the number of instances. For example, if you purchase four d1 instances with 8 cores, the local disk capacity is: 8/2 \* 5.5 \* 4=88 TB. HDFS requires three backups. Therefore, you need to buy at least three instances that use local disks. We recommend that you purchase at least four instances for data reliability and disk recovery.

    -   If the amount of data stored in HDFS is less than 60 TB, you can use ultra disks or SSD disks.

-   Select task instance configuration

    -   Task instances are used when the CPUs and memory of core instances have insufficient computing capability. Task instances do not store data or run DataNode. You can estimate the number of instances based on CPU and memory requirements.


## EMR lifecycle {#section_bwz_zjp_y2b .section}

EMR supports auto-scaling, which allows you to quickly [scale up a cluster](../../../../../reseller.en-US/User Guide/Clusters/Expand a cluster.md#). You can flexibly adjust the configuration of cluster nodes, or [upgrade or downgrade ECS instance configuration](https://partners-intl.aliyun.com/help/doc-detail/25437.htm).

## Select a zone {#section_cwz_zjp_y2b .section}

We recommend that you deploy EMR and your business system in [the same zone and region](https://partners-intl.aliyun.com/help/doc-detail/40654.htm).

