# ECS instances {#concept_axl_5m3_y2b .concept}

ECS instances

## ECS instance types available for EMR {#section_zds_wm3_y2b .section}

-   General-purpose type

    vCPU/Memory ratio is 1:4, for example, 32 core and 128 GB. This type uses cloud disks as storage.

-   Compute type

    vCPU/Memory ratio is 1:2, for example, 32 core and 64 GB. This type uses cloud disks as storage and provides more computing resources.

-   Memory type

    vCPU/Memory ratio is 1:8, for example, 32 core and 256 GB. This type uses cloud disks as storage and provides more memory resources.

-   Big data type

    Utilizing local SATA disks as a highly cost-effective data storage solution, it is a recommended ECS instance type applicable for use cases involving mass data volumes \(TB-level\).

-   Ephemeral SSD type

    Utilizing ephemeral SSDs, the ECS instance type has a high local IOPS and throughput.

-   Shared type \(entry level\)

    The ECS instance type with shared CPUs is not stable enough for use cases involving massive computing volumes. It is applicable for entry-level learning users rather than enterprise customers.

-   GPU

    It is a heterogeneous GPU-based ECS instance type applicable for machine learning use cases.


## ECS instances applicable for different scenarios {#section_xmd_ym3_y2b .section}

-   Master instances

    General-purpose or memory types are applicable for master instances, where data is directly stored on Alibaba Cloud’s cloud disks. It has three backups, which guarantees high data reliability.

-   Core instances

    General-purpose, compute, and memory types are applicable for small data volume use cases \(below TB level\) or when OSS is used for primary data storage. When data volume is large \(10 TB or more\), we recommend that you use the big data type for great cost-effectiveness. When utilizing ephemeral disks, data reliability is challenged, but it can be maintained and guaranteed by the EMR platform.

-   Task instances

    All types except the big data type are applicable for task instances to give additional computing power to the cluster. Currently, the ephemeral SSD type is not yet supported, but will be added to the task instance soon.


