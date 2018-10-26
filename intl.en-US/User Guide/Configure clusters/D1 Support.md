# D1 Support {#concept_m2g_z43_y2b .concept}

To meet the storage needs in the big data-type use case, Alibaba Cloud launched an instance series using ephemeral disks in the cloud: the D1 series. The D1 series uses ephemeral disks instead of cloud disks for data storage. This solves the high cost problem caused by keeping multiple copies of redundant data in the cloud disks. In addition, as no data must be transferred by the network, the disk throughput is improved. Furthermore, the series can also take advantage of Hadoop’s proximity computing.

Compared with cloud disks, the series greatly enhances storage performance and reduces storage prices, reaching nearly the same cost as offline physical hosts.

Along with numerous advantages of using ephemeral disks, the problem of data reliability occurs. For cloud disks, because of Alibaba Cloud’s default multi-disk backup policy, you do not need consider disk damages. Cloud disks automatically guarantee data reliability. However, when you use ephemeral disks, such guarantee requires upper-level software. If disk and node failures appear, manual operations and maintenance must be performed.

## The EMR + D1 solution {#section_odk_1p3_y2b .section}

A complete set of automated maintenance solution like D1 is available in EMR for ephemeral disks. This allows Alibaba Cloud users to utilize instances using ephemeral disks without considering the entire maintenance process, as data reliability and service availability are guaranteed.

Highlights are as follow:

-   Highly reliable distribution of required nodes
-   Ephemeral disk and node faults monitoring
-   Automatic determination of data migration opportunities
-   Automatic failed node migration and data balancing
-   Automatic HDFS data detection
-   Network topology optimization

With automated maintenance of the entire back-end management and control system, EMR helps you make better use of ephemeral disks, and develop cost-effective big data system.

**Note:** If you want to set up a Hadoop cluster using the D1 series, open a ticket so we may assist in your operations.

