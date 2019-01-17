# ECS instances {#concept_axl_5m3_y2b .concept}

This section contains information on the different ECS instance types.

## ECS instance types supported by E-MapReduce {#section_zds_wm3_y2b .section}

-   General-purpose

    This type uses cloud disks as storage. The ratio between vCPUs and memory is 1:4. For example, 32 cores and 128 GB memory.

-   Compute

    This type uses cloud disks as storage and provides more computing resources. The ratio between vCPUs and memory is 1:2. For example, 32 cores and 64 GB memory.

-   Memory

    This type uses cloud disks as storage and provides more memory resources. The ratio between vCPUs and memory is 1:8. For example, 32 cores and 256 GB memory.

-   Big data

    This type uses local SATA disks as storage, which is highly cost-effective. If you want to store massive amounts of data \(TB-level\), it is recommended that you use this type.

-   Ephemeral SSD

    This type uses ephemeral SSDs as storage, which provides high local IOPS and throughput.

-   Shared \(entry level\)

    This type shares CPUs and is not stable enough for most scenarios. It is applicable for entry-level users, not enterprise customers.

-   GPU

    This type is a heterogeneous GPU-based model applicable in machine learning scenarios.


## ECS instance types applicable in different scenarios {#section_xmd_ym3_y2b .section}

-   Master instances

    General-purpose and memory types are applicable in master instances, where data is directly stored on Alibaba Cloud's cloud disks. There are also three backups to guarantee high data reliability.

-   Core instances

    General-purpose, compute, and memory types are applicable for small data volumes \(not TB-level\) or when OSS is used as the primary data storage. When the amount of data is large \(10 TB or more\), it is recommended that you use the big data type, which is more cost-effective. Using an ephemeral disk makes it harder to ensure data reliability, but this can be maintained and guaranteed by the E-MapReduce platform.

-   Task instances

    All types except the big data type are suitable for task instances to give additional computing power to the cluster. Currently, the ephemeral SSD type is not supported, but will be added soon.


