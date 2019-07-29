# JindoFS user guide {#concept_473658 .concept}

JindoFS is a cloud-native file system that combines the advantages of OSS and local storage. JindoFS is also the next-generation storage system that provides efficient and reliable storage services for cloud computing. This topic describes how to configure and use JindoFS, and its scenarios.

## Overview {#section_ed7_ndi_45t .section}

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/381399/156436623248599_en-US.png)

JindoFS adopts a heterogeneous multi-backup mechanism. Storage Service provides data storage capacity. OSS is used as the back-end storage to ensure high reliability of data. Local storage is used to implement redundant backup. Backups stored in the local storage can accelerate read operations. Namespace Service serves as the local service that manages metadata of JindoFS. This way, metadata is queried from Namespace Service instead of OSS, which improves query performance. This query method of JindoFS is the same as that of HDFS. EMR V3.20.0 and later support Jindo FS. To use JindoFS, select the related services when creating an E-MapReduce cluster.

## Environment preparation {#section_dny_u6x_m00 .section}

-   Create E-MapReduce clusters

    Select EMR V3.20.0 or later. Select **SmartData** and **Bigboot**. For more information about how to create an E-MapReduce cluster, see [Create a cluster](../../../../reseller.en-US/Cluster Planning and Configurations/Configure clusters/Create a cluster.md#). Bigboot provides distributed data management in the E-MapReduce console. SmartData is based on Bigboot to provide JindoFS for the application layer.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/381399/156436623248600_en-US.png)

-   Configure E-MapReduce clusters

    JindoFS provided by SmartData uses OSS as the storage back end. Therefore, you need to configure OSS-related parameters before using JindoFS. The following section provides two configuration methods. If you use the first configuration method, you need to create an E-MapReduce cluster, modify Bigboot-related parameters, and restart SmartData for the configurations take effect. If you use the second configuration method, you need to add custom configurations when you create an E-MapReduce cluster. This method ensures that the related services are restarted based on custom parameters after the E-MapReduce cluster is created.

    -   Initialize parameters after an E-MapReduce cluster is created

        All JindoFS-related configurations are contained in Bigboot. As shown in the following figure, parameters in the red box are required. oss.access.bucket specifies the name of the bucket in OSS. oss.data-dir specifies the directory used by JindoFS in the OSS bucket. Note that the directory is the back-end storage directory of JindoFS. Data stored in this directory cannot be modified or deleted. Ensure that the directory can be used only as the back-end storage of JindoFS. The directory configured by users are automatically created when data is written to JindoFS. You can move the pointer over the information icons to view instructions of the oss.access.endpoint, oss.access.key, and oss.access.secret parameters. To maintain performance and stability, we recommend that you select an OSS bucket that is located in the same region as the E-MapReduce cluster when you configure the storage back end. This way, the E-MapReduce cluster can access OSS without using a password. In this case, you do not need to configure these parameters.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/381399/156436623248601_en-US.png)

        Save and deploy the configurations. Restart all components in SmartData to start using JindoFS.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/381399/156436623248602_en-US.png)

    -   Add custom configurations when creating an E-MapReduce cluster

        You can add custom configurations when you create an E-MapReduce cluster. The following section uses password-free access to OSS within the same region as an example. Turn on **Custom Software Settings**. Add the following configurations to the field in the Advanced Settings section.

        ``` {#codeblock_k7q_23k_z6p}
        [
            {         
            "ServiceName":"BIGBOOT",
            "FileName":"bigboot",
            "ConfigKey":"oss.data-dir",
            "ConfigValue":"jindoFS-1"
            },
            {
            "ServiceName":"BIGBOOT",
            "FileName":"bigboot",
            "ConfigKey":"oss.access.bucket",
            "ConfigValue":"oss-bucket-name"
            }
        ]
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/381399/156436623348604_en-US.png)


## Use JindoFS {#section_jca_e6y_zza .section}

The use of JindoFS is similar to HDFS. JindoFS also provides a prefix. To use JindoFS, you need only to replace hdfs with jfs. Example:

``` {#codeblock_d6a_6i7_cwk}
hadoop fs -ls jfs:/// hadoop fs -mkdir jfs:///test-dirhadoop fs -put test.log jfs:///test-dir/
```

Data can be read from JindoFS only when jobs of MapReduce, Hive, and Spark components are running in the E-MapReduce cluster.

## Scenarios {#section_qrf_zrj_sf6 .section}

E-MapReduce provides the following big data-based storage systems: EMR OssFileSystem, EMR HDFS, and EMR JindoFS. OssFileSystem and JindoFS are cloud-based storage solutions. The following table compares features of these storage systems and the Hadoop-Aliyun module.

|Feature|Hadoop-Aliyun module|EMR OssFileSystem|EMR HDFS|EMR JindoFS|
|-------|--------------------|-----------------|--------|-----------|
|Storage space|Capable of storing large amounts of data|Capable of storing large amounts of data|Dependent on the cluster scale|Capable of storing large amounts of data|
|Reliability|High|High|High|High|
|Factor affecting throughput|Server|I/O performance of disks in the E-MapReduce cluster|I/O performance of disks in the E-MapReduce cluster|I/O performance of disks in the E-MapReduce cluster|
|Metadata query efficiency|Slow|Medium|Quick|Quick|
|Scale-out operation|Easy|Easy|Easy|Easy|
|Scale-in operation|Easy|Easy|Decommissioning required|Easy|
|Data locality|None|Weak|Strong|Relatively strong|

JindoFS has the following features:

-   Infinitely scalable storage capable of storing large amounts of data. OSS is used as the storage back end that is independent of the local cluster. The local cluster can be scaled as needed.
-   Read operations can be accelerated if data can be read from the storage resources in the local cluster. This capacity applies to the local cluster that is capable of storing a certain amount of data. Based on this capacity, the throughput efficiency of the local cluster can be maximized. The throughput efficiency can be greatly improved for Write Once Read Many \(WORM\)-compliant storage solutions.
-   High efficiency of metadata-related queries. This Namespace Service-based query method of JindoFS is the same as that of HDFS. This method helps resolve the problems of inefficient queries from OSS and instability caused by frequent access.
-   Maximal data locality provided when jobs are executed in the E-MapReduce cluster. This method eases network transmission pressure and further improves read performance.

## Disk space usage-based control {#section_r89_879_ml3 .section}

The back end of JindoFS is based on OSS that is capable of storing large amounts of data. However, the storage capacity of local disks is limited. Therefore, JindoFS releases data backups that are less frequently accessed. Alibaba Cloud uses node.data-dirs.watermark.high.ratio and node.data-dirs.watermark.low.ratio to adjust the usage of local disk storage. The average value ranging from 0 to 1 indicates the storage usage. JindoFS uses the total storage of all data disks by default. The node.data-dirs.watermark.high.ratio parameter specifies the upper limit of storage usage of each data disk. Less frequently accessed data stored in disks is released if their storage used by JindoFS reaches the upper limit. The node.data-dirs.watermark.low.ratio parameter specifies the lower limit of storage usage. After storage usage of data disks reaches the upper limit, less frequently accessed data will be released until the storage usage of the disks reaches a value that is smaller than the lower limit. You can set the upper limit and lower limit to adjust and assign disk space to JindoFS. Ensure that the upper limit is greater than the lower limit.

