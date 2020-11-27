# JindoFS Block模式使用说明

Block模式提供了最为高效的数据读写能力和元数据访问能力。数据以Block形式存储在后端存储OSS上，本地提供缓存加速，元数据则由本地Namespace服务维护，提供高效的元数据访问性能。本文主要介绍JindoFS的Block模式及其使用方式。

JindoFS Block模式具有以下几个特点：

-   海量弹性的存储空间，基于OSS作为存储后端，存储不受限于本地集群，而且本地集群能够自由弹性伸缩。
-   能够利用本地集群的存储资源加速数据读取，适合具有一定本地存储能力的集群，能够利用有限的本地存储提升吞吐率，特别对于一写多读的场景效果显著。
-   元数据操作效率高，能够与HDFS相当，能够有效规避OSS文件系统元数据操作耗时以及高频访问下可能引发不稳定的问题。
-   能够最大限度保证执行作业时的数据本地化，减少网络传输的压力，进一步提升读取性能。

## 配置使用方式

1.  进入SmartData服务。

    1.  登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com/)。

    2.  在顶部菜单栏处，根据实际情况选择地域（Region）和资源组。

    3.  单击上方的**集群管理**页签。

    4.  在**集群管理**页面，单击相应集群所在行的**详情**。

    5.  在左侧导航栏单击**集群服务** \> **SmartData**。

2.  进入**namespace**服务配置。

    1.  单击**配置**页签。

    2.  单击**namespace**。

        ![namespace](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0357459951/p161094.png)

3.  配置以下参数。

    JindoFS支持多命名空间，本文命名空间以test为例。

    1.  修改**jfs.namespaces**为**test**。

        **test**表示当前JindoFS支持的命名空间，多个命名空间时以逗号隔开。

    2.  单击**自定义配置**，在**新增配置项**对话框中增加以下参数，单击**确定**。

        |参数|参数说明|示例|
        |--|----|--|
        |jfs.namespaces.test.oss.uri|表示test命名空间的后端存储。|oss://<oss\_bucket\>/<oss\_dir\>/**说明：** 推荐配置到OSS bucket下的某一个具体目录，该命名空间即会将Block模式的数据块存放在该目录下。 |
        |jfs.namespaces.test.mode|表示test命名空间为块存储模式。|block|
        |jfs.namespaces.test.oss.access.key|表示存储后端OSS的AccessKey ID。|xxxx**说明：** 考虑到性能和稳定性，推荐使用同账户、同Region下的OSS bucket作为存储后端，此时，E-MapReduce集群能够免密访问OSS，无需配置AccessKey ID和AccessKey Secret。 |
        |jfs.namespaces.test.oss.access.secret|表示存储后端OSS的AccessKey Secret。|

    3.  单击**确定**。

4.  单击右上角的**保存**。

5.  单击右上角的**操作** \> **重启 Jindo Namespace Service**。

    重启后即可通过`jfs://test/<path_of_file>`的形式访问JindoFS上的文件。


## 磁盘空间水位控制

JindoFS后端基于OSS，可以提供海量的存储，但是本地盘的容量是有限的，因此JindoFS会自动淘汰本地较冷的数据备份。我们提供了`storage.watermark.high.ratio`和`storage.watermark.low.ratio`两个参数来调节本地存储的使用容量，值均为0～1的小数，表示使用磁盘空间的比例。

1.  修改磁盘水位配置。

    在**服务配置**区域的**storage**页签，修改如下参数。

    ![storage](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0257459951/p161207.png)

    |参数|描述|
    |--|--|
    |**storage.watermark.high.ratio**|表示磁盘使用量的上水位比例，每块数据盘的JindoFS数据目录占用的磁盘空间到达上水位即会触发清理。默认值：0.4。|
    |**storage.watermark.low.ratio**|表示使用量的下水位比例，触发清理后会自动清理冷数据，将JindoFS数据目录占用空间清理到下水位。默认值：0.2。|

    **说明：** 您可以通过设置上水位比例调节期望分给JindoFS的磁盘空间，下水位必须小于上水位，设置合理的值即可。

2.  保存配置。

    1.  单击右上角的**保存**。

    2.  在**确认修改**对话框中，输入执行原因，开启**自动更新配置**。

    3.  单击**确定**。

3.  重启Jindo Storage Service使配置生效。

    1.  单击右上角的**操作** \> **重启Jindo Storage Service**。

    2.  在**执行集群操作**对话框中，设置相关参数。

    3.  单击**确定**。

    4.  在**确认**对话框中，单击**确定**。


