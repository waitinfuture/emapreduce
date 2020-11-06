# Select configurations

To use E-MapReduce \(EMR\), you must select appropriate clusters. When you select configurations for EMR, consider the use of big data in your enterprise and your financial budget, and estimate the volume of data and the reliability of services that you require.

## Big data scenarios

EMR applies to the following enterprise big data scenarios:

-   Batch operations

    This scenario requires both high disk throughput and high network throughput, supports processing of large volumes of data, but has low requirements on the timeliness of data processing. You can use MapReduce, Pig, or Spark for this scenario. This scenario does not require a large memory capacity. Therefore, you need to focus on the requirements of jobs on the vCPU and memory, and the network requirements when you perform data shuffling.

-   Ad hoc queries

    Data scientists and data analysts use ad hoc query tools to retrieve data. This scenario requires real-time queries, high disk throughput, and high network throughput. You can use Impala or Presto for this scenario. This scenario requires a large memory capacity. You must consider the volume of both data and concurrent queries your business needs to handle.

-   Stream computing, high network throughput, and compute-intensive scenarios

    You can use Flink, Spark Streaming, or Storm for these scenarios.

-   Message queues

    In this scenario, high disk throughput and high network throughput are required, the memory usage is high, and data storage does not depend on HDFS. You can use Kafka.

-   Cold backup

    This scenario does not require high disk or high computing throughput but requires low cost. We recommend that you use EMR d1 instances for cold backups. The storage cost of each d1 instance is USD 0.003/month/GB.


## EMR nodes

An EMR cluster consists of three types of nodes: master, core, and task nodes. For more information, see [Node categories](/intl.en-US/Cluster Management/Cluster planning/Node categories.md).

You can select ultra disks, local disks, standard SSDs, or local SSDs for EMR storage. These disks are ranked in descending order of performance: local SSDs \> standard SSDs \> local disks \> ultra disks.

EMR underlying storage supports OSS \(OSS Standard storage only\) and HDFS. OSS has a higher data availability than HDFS. The data availability of OSS is 99.99999999%, while the data availability of HDFS depends on the reliability of cloud disk or local disk storage.

Storage prices:

-   Local disk storage: USD 0.003/GB/month
-   OSS Standard storage: USD 0.02/GB/month
-   Ultra disk storage: USD 0.05/GB/month
-   Standard SSD storage: USD 0.143/GB/month

## Select configurations for EMR

-   Select master node configurations.
    -   Master nodes are used to deploy the master processes of Hadoop, such as NameNode and ResourceManager.
    -   EMR services such as HDFS, YARN, Hive, and HBase use the high availability architecture. We recommend that you enable high availability for production clusters in the **Hardware Settings** step. If high availability is not enabled when you create an EMR cluster, it cannot be enabled later.
    -   Master nodes are used to store HDFS metadata and component log files. These nodes are compute-intensive with low disk I/O requirements. HDFS metadata is stored in memory. The minimum recommended memory size is 16 GB based on the number of files.

-   Select core node configurations.

    -   Core nodes are used to store data, run computing tasks, and run processes such as DataNode and NodeManager.
    -   If the volume of data stored in HDFS exceeds 60 TB, we recommend that you use instances with local disks, such as ECS d1 and ECS d1ne instances. The local disk capacity is calculated by using the following formula: \(Number of vCPUs/2\) × 5.5 TB × Number of instances.

        For example, if you purchase four d1 instances with eight vCPUs, the local disk capacity is 88 TB, which is calculated by using the following formula: 8/2 × 5.5 × 4. HDFS requires three replicas. Therefore, you must purchase at least three instances that use local disks. To ensure data reliability and disaster recovery, we recommend that you purchase at least four instances.

    -   If the volume of data stored in HDFS is less than 60 TB, you can use ultra disks or standard SSDs.
-   Select task node configurations.

    Task nodes are used if the computing capabilities of vCPUs and the memory of core nodes are insufficient. Task nodes do not store data or run DataNode. You can estimate the number of task nodes based on your vCPU and memory requirements.


## EMR lifecycle

EMR supports auto scaling. You can scale out a cluster by following the instructions provided in [Scale out a cluster](/intl.en-US/Cluster Management/Configure clusters/Scale out a cluster.md) or scale ECS instances up or down by following the instructions provided in [Overview of instance upgrade and downgrade](/intl.en-US/Instance/Change configurations/Overview of instance upgrade and downgrade.md).

## Select a zone

We recommend that you deploy both EMR and your business system in the same zone of the same region to ensure high efficiency. For more information, see [Regions and zones]().

