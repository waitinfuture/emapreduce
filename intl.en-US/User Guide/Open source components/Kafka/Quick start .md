# Quick start {#concept_ot1_5bx_y2b .concept}

E-MapReduce 3.4.0 and later support Kafka.

## Create a Kafka cluster {#section_w3z_vbx_y2b .section}

When creating a cluster on E-MapReduce, set the cluster type to Kafka. A cluster containing only Kafka components is created by default. The components include basic components, as well as Zookeeper, Kafka, and KafkaManager components. Only one Kafka broker is deployed on each node. We recommend that you use a dedicated Kafka cluster instead of mixing with Hadoop services.

## Ephemeral disk Kafka clusters {#section_x3z_vbx_y2b .section}

To better reduce unit costs and respond to larger storage needs, E-MapReduce 3.5.1 supports Kafka clusters on local disks \(D1 cluster models\). For more information, see [ECS models](../../../../../reseller.en-US/Product Introduction/Instance type families.md#). Compared to cloud disks, local disk Kafka clusters have the following features:

-   High-volume local SATA HDD disks with high I/O throughput, sequential read and write performance on a single disk of 190 MB/s, and up to 5 GB/s of storage I/O capability.
-   Cost of local storage is 97% lower than that of SSD cloud disks.
-   Higher network performance, with up to 17 Gbit/s instances of network bandwidth. This meets data interaction requirements for peak business instances.

Local disk models also have the following features:

|Operation|Ephemeral disk data status|Description|
|:--------|:-------------------------|:----------|
|Restart within the operating system/restart or force restart in the ECS console|Retained|The local ephemeral disk's storage volume is retained. Data is also retained.|
|Shut down within the operating system/Stop or force stop in the ECS console|Retained|The local ephemeral disk's storage volume is retained. Data is also retained.|
|Release \(instances\) on the console|Erased|The local ephemeral disk's storage volume is erased. Data is not retained.|

**Note:** 

-   When the host is down or the disk is corrupted, the data on the disk is lost.
-   Do not store business data on a local ephemeral disk for a long period of time. Back up data in a timely manner and adopt a high-availability architecture. For long-term storage, we recommend that you store data on a cloud disk.

To be able to deploy Kafka on a local disk, E-MapReduce has the following default requirements:

1.  default.replication.factor = 3 indicates that the number of partitions and replicas in the topic is at least three. If a smaller number of replicas is set, the risk of data loss is increased.
2.  min.insync.replicas = 2 indicates that when the producer is required to set acks to all \(-1\), it is considered successful to write at least two replicas at a time.

When a local disk corruption occurs, E-MapReduce performs the following:

1.  Removes the bad disk from the broker configuration, restarts Broker, and recovers the lost data from the bad disk on the other available local disks. The time it takes to perform data recovery varies according to the amount of data that has been written on the broken disk.
2.  When the number of damaged machine disks is over 20%, E-MapReduce takes the initiative to migrate the machine and restore the abnormal disk.
3.  If there is not enough disk space available on the current machine to recover lost data on the damaged disk, Broker is shut down abnormally. If this is the case, you can choose to clean some data, free up disk space, or restart the Broker service. You can also open a ticket with E-MapReduce for machine migration and to recover abnormal disks.

## Parameter description {#section_fjz_vbx_y2b .section}

You can check Kafka software configurations on the E-MapReduce cluster configuration management interface.

|Configuration item|Description|
|:-----------------|:----------|
|zookeeper.connect|Zookeeper connection address configured on Kafka.|
|kafka.heap.opts|Size of the heap memory of the Kafka broker.|
|num.io.threads|Number of the Kafka broker's I/O threads, which by default is twice the number of CPU cores.|
|num.network.threads|Number of the Kafka broker's network threads, which by default is the same as the number of CPU cores.|

