# Quick start with Kafka {#concept_ot1_5bx_y2b .concept}

E-MapReduce 3.4.0 and later versions support the Kafka service.

## Create a Kafka cluster {#section_w3z_vbx_y2b .section}

Set the cluster type to Kafka when creating a cluster on E-MapReduce. Then, a cluster containing only Kafka components is created by default. The components include the basic components and the Zookeeper, Kafka, and KafkaManager components. Only one Kafka broker is deployed on each node. We recommend you use a dedicated Kafka cluster, not mixing with Hadoop services.

## Ephemeral disk Kafka clusters {#section_x3z_vbx_y2b .section}

To better reduce unit costs and respond to larger storage needs, E-MapReduce 3.5.1 supports Kafka clusters on local disks \(D1 cluster models. For more information, see [ECS models](../../../../intl.en-US/Product Introduction/Instance type families.md#)\). Compared to cloud disks, local disk Kafka clusters have the following features:

-   High-volume local SATA HDD disks with high I/O throughput, 190 MB/s of sequential read and write performance on a single disk, and up to 5 GB/s of storage I/O ability.
-   Price of local storage is 97% lower than that of SSD cloud disks.
-   Higher network performance, up to 17 Gbit/s instances of network bandwidth which meet data interaction needs among business peak instances.

Local disk models also have the following features:

|Operation|Ephemeral disk data status|Description|
|:--------|:-------------------------|:----------|
|Restart within the operating system/restart or force restart in the ECS console|Retained|The local ephemeral disk's storage volume is retained and data is also retained.|
|Shut down within the operating system/Stop or force stop in the ECS console|Retained|The local ephemeral disk's storage volume is retained and data is also retained.|
|Release \(instances\) on the console|Erased|The local ephemeral disk's storage volume is erased and data is not retained.|

**Note:** 

-   When the host is down or the disk is corrupted, the data on the disk is lost.
-   Do not store business data on an local ephemeral disk for a long time period. Back up data in a timely manner and adopt high-availability architecture. For long-term storage, you are advised to store data on a cloud disk.

To be able to deploy the Kafka service on a local disk, E-MapReduce has default requirements:

1.  default.replication.factor = 3 indicates that the number of the topic's partitions and replicas is at least three. If a smaller number of replicas is set, the risk of data loss is increased.
2.  min.insync.replicas = 2 indicates that when the producer is required to set acks to all\(-1\), it is considered a successful writing to write at least two replicas at a time.

When a local disk corruption occurs, E-MapReduce performs:

1.  Remove the bad disk from the Broker configuration, restart Broker, and recover the lost data from the bad disk on the other available local disks. Data recovery time varies according to the amount of data that have been written on the broken disk.
2.  When the number of machine disks damaged \(over 20%\), E-MapReduce takes the initiative to migrate the machine and restore the abnormal disk.
3.  If there is not enough disk space available on the current machine to recover loss data on the damaged disk, Broker will be shut down abnormally. In this case, you can choose to clean up some data, free up disk space and restart the Broker service. You can also contact E-MapReduce for machine migration and recover abnormal disks.

## Parameter description {#section_fjz_vbx_y2b .section}

You can check the Kafka software configuration on the E-MapReduce cluster configuration management interface.

|Configuration Item|Description|
|:-----------------|:----------|
|zookeeper.connect|Zookeeper connection address configured on Kafka|
|kafka.heap.opts|Size of the heap memory of the Kafka broker|
|num.io.threads|Number of I/O threads of the Kafka broker, which is twice the number of CPU cores by default|
|num.network.threads|Number of network threads of the Kafka broker, which is the same as the number of CPU cores by default|

