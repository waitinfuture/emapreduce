# Cluster specifications {#concept_h5h_vjp_y2b .concept}

The first step to use E-MapReduce is to configure an appropriate Hadoop cluster. To select a E-MapReduce configuration type, you must take into account the big data scenarios of the enterprise to estimate Inoata amount, the reliability requirement of services, and corporate budgets.

## Big data scenarios {#section_uvz_zjp_y2b .section}

At present, E-MapReduce product primarily meets the following requirements of big data scenarios for enterprises:

-   Batch processing features high disk throughput, high network throughput, and low real-time requirements. If there is large amounts of data to be processed, but the requirement for real-time processing is not high, you can select services such as MapReduce, Pig, and Spark. For scenarios with low memory requirements, consider the demand for CPU and memory for large jobs and the network demand of shuffle to select appropriate types.
-   Ad hoc queries: data scientists or data analysts use ad hoc query tools to retrieve data. For scenarios with features of high real-time query capacity, high disk throughput, and high network throughput, use EMR Impala and Presto services. For scenarios with high memory requirements, consider data volume and the number of concurrent queries.
-   For stream computing scenarios with high network throughput and compute-intensive requirements, use E-MapReduce Flink, Spark Streaming, and Storm services.
-   For scenarios of message queues, high disk throughput, high network throughput, large amounts of memory consumption, and the scenarios where the storage doesn’t depend on HDFS, use E-MapReduce Kafka. In order to avoid the impact on Hadoop, E-MapReduce will separate Kafka and Hadoop into two clusters.
-   For data cold backup scenarios, the requirements for computing and disk throughput are not high, but low costs of data cold backup are required. We recommend that you use E-MapReduce D1 instance to perform data cold backup. The instance storage cost in the local D1 disk is USD 0.003/month/GB.

## E-MapReduce node {#section_wvz_zjp_y2b .section}

E-MapReduce has three [Instance types](../../../../intl.en-US/User Guide/Configure clusters/Instance types.md#): Master, Core, and Task.

E-MapReduce storage can use ultra disks, SSD disks, and local disks. Disk performance comparison is as follows: SSD disks \> local disks \> ultra disks.

The E-MapReduce underlying storage supports OSS \(standard OSS only\) and HDFS. The data availability of OSS is higher than that of HDFS. The data availability of OSS is 99.99999999%, while that of HDFS is 99.99999%.

The storage price is estimated roughly as follows:

-   The cost of local disk instance storage is USD 0.003/GB/month.
-   The cost of OSS standard storage is USD 0.02/GB/month.
-   The cost of ultra disks is USD 0.05/GB/month.
-   The cost of SSD disks is USD 0.143/GB/month.

## Type selection of E-MapReduce {#section_yvz_zjp_y2b .section}

-   Type selection of master nodes

    Master nodes are used to deploy the master processes of Hadoop, such as NameNode and ResourceManager.

    We recommend that you enable HA on the production cluster. HA is available for components such as E-MapReduce HDFS, YARN, Hive, and HBase. We recommend that you enable **High Availability** in the “Hardware Configuration” step when you create the production cluster. If **High Availability** is not enabled when you purchase the product, it cannot be enabled later.

    The master node is primarily used to store HDFS metadata and component log files. It is compute-intensive without high requirement for disk IO. HDFS metadata is stored in memory, and it is recommended that the memory should be 16 GB or above based on the number of files.

-   Type selection of core nodes

    The core node is primarily used to store data, perform calculations, and run DataNode and Nodemanager.

    If the data volume of HDFS \(3 backups\) is greater than 60 TB, we recommend that you use local disk instance \(ECS.D1，ECS.D1NE\). The disk capacity is \(number of CPU cores/2\)\*5.5TB\*number of instances. For example, if you purchase four 8-core D1 instances, the disk capacity is 8/2\*5.5\*4=88 TB. Because HDFS uses 3 backups, you can buy at least 3 local disk instances. Considering data reliability and disk damage, we recommend that you purchase at least 4 instances.

    If the data volume of HDFS is less than 60TB, you can consider ultra disks and SSD disks.

-   Type selection of task nodes

    Task nodes are mainly used to supplement the CPU and memory, which can enhance the computing capability of Core nodes. The nodes do not store data or run DataNode. You can estimate the number of instances based on CPU and memory requirements.


## E-MapReduce lifecycle {#section_bwz_zjp_y2b .section}

E-MapReduce supports auto-scaling, which can quickly [Expand a cluster](../../../../intl.en-US/User Guide/Cluster/Expand a cluster.md#). It can flexibly adjust the configuration of cluster nodes or [upgrade or downgrade instance configurations](https://www.alibabacloud.com/help/doc-detail/25437.htm).

## Available zones {#section_cwz_zjp_y2b .section}

To ensure efficiency, it's better to deploy EMR and the business system in the same region and zone. For more information, see [Regions and zones](https://www.alibabacloud.com/help/doc-detail/40654.htm).

