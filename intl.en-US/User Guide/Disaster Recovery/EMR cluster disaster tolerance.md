# EMR cluster disaster tolerance {#concept_ldy_w4j_z2b .concept}

## Data disaster tolerance {#section_bzh_y4j_z2b .section}

The Hadoop Distributed File System \(HDFS\) stores the data of each file in blocks, and each block has some copies \(Each block has three copies by default\). This makes sure these copies of data block can be stored in the different frameworks. In most situations, the storage strategy of HDFS is to store the first copy in the local framework, the second copy is stored in the same framework with the first one, but in different nodes, the last copy is stored in the different frameworks.

HDFS will scan the data copies regularly, if a data copy was lost, HDFS will make another data copy quickly to make sure the number of data copy is stable. If a node that stores a data copy was lost, HDFS will make another node to recover to data in that node. In Alibaba Cloud, if you use cloud disk, each cloud disk has three data copies in the backend, whenever any of them has some issues, the data copies will exchange and recover data to ensure the reliability of data.

HDFS is a file storage system that has stood the test of time and has high reliability. It can store massive data with high reliability. At the same time, based on the features in Alibaba Cloud, HDFS can make extra backups for the data stored in OSS, in this way, HDFS makes the data higher reliability.

## Service disaster tolerance {#section_czh_y4j_z2b .section}

The core components of HDFS will deploy the HA, that is, there are at least two nodes be the backups for each other, such as, YARN, HDFS, Hive Server, Hive Meta. In this way, whenever there’s a node with issues, the nodes will exchange and recover data to ensure the services has no impact.

