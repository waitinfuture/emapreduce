# JindoFS缓存模式使用说明

缓存模式（Cache）主要兼容原生OSS存储方式，文件以对象的形式存储在OSS上，每个文件根据实际访问情况会在本地进行缓存，提升EMR集群内访问OSS的效率，同时兼容了原有OSS原有文件形式，数据访问上能够与其他OSS客户端完全兼容。本文主要介绍JindoFS的缓存模式及其使用方式。

缓存模式最大的特点就是兼容性，保持了OSS原有的对象语义，集群中仅做缓存，因此和其他的各种OSS客户端是完全兼容的，对原有OSS上的存量数据也不需要做任何的迁移、转换工作即可使用。同时集群中的缓存也能一定程度上提升数据访问性能，缓解读写OSS的带宽压力。

## 配置使用方式

JindoFS缓存模式提供了以下两种基本使用方式，以满足不同的使用需求。

-   OSS Scheme

    详情请参见[配置OSS Scheme（推荐）](#section_f4w_h8l_gd0)。

-   JFS Scheme

    详情请参见[配置JFS Scheme](#section_kvg_dk2_4hm)。


## 配置OSS Scheme（推荐）

OSS Scheme保留了原有OSS文件系统的使用习惯，即直接通过`oss://<bucket_name>/<path_of_your_file>`的形式访问OSS上的文件。使用该方式访问OSS，无需进行额外的配置，创建EMR集群后即可使用，对于原有读写OSS的作业也无需做任何修改即可运行。

## 配置JFS Scheme

1.  进入SmartData服务。

    1.  登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com/)。

    2.  在顶部菜单栏处，根据实际情况选择地域（Region）和资源组。

    3.  单击上方的**集群管理**页签。

    4.  在**集群管理**页面，单击相应集群所在行的**详情**。

    5.  在左侧导航栏单击**集群服务** \> **SmartData**。

2.  进入**namespace**服务配置。

    1.  单击**配置**页签。

    2.  单击**namespace**。

        ![namespace](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0357459951/p161094.png)

3.  配置以下参数。

    JindoFS支持多命名空间，本文命名空间以test为例。

    1.  修改**jfs.namespaces**为**test**。

        **test**表示当前JindoFS支持的命名空间，多个命名空间时以逗号隔开。

    2.  单击**自定义配置**，在**新增配置项**对话框中增加以下参数。

        |参数|参数说明|示例|
        |--|----|--|
        |jfs.namespaces.test.oss.uri|表示test命名空间的后端存储。|oss://<oss\_bucket\>/<oss\_dir\>/**说明：** 该配置必须配置到OSS Bucket下的具体目录，也可以直接使用根目录。 |
        |jfs.namespaces.test.mode|表示test命名空间为缓存模式。|cache|

4.  单击右上角的**保存**。

5.  单击右上角的**操作** \> **重启 Jindo Namespace Service**。

    重启后即可通过`jfs://test/<path_of_file>`的形式访问JindoFS上的文件。


## 启用缓存

启用缓存会利用本地磁盘对访问的热数据块进行缓存，默认状态为禁用，即所有OSS读取都直接访问OSS上的数据。

1.  在**集群服务** \> **SmartData**的**配置**页面，单击**client**页签。

2.  修改**jfs.cache.data-cache.enable**为**1**，表示启用缓存。

    此配置为客户端配置，不需要重启SmartData服务。


缓存启用后，Jindo服务会自动管理本地缓存备份，通过水位清理本地缓存，请您根据需求配置一定的比例用于缓存，详情请参见[磁盘空间水位控制](/intl.zh-CN/JindoFS/JindoFS基础使用（EMR-3.27.0及以上版本）/JindoFS块存储模式使用说明.md)。

## 磁盘空间水位控制

JindoFS后端基于OSS，可以提供海量的存储，但是本地盘的容量是有限的，因此JindoFS会自动淘汰本地较冷的数据备份。我们提供了`storage.watermark.high.ratio`和`storage.watermark.low.ratio`两个参数来调节本地存储的使用容量，值均为0～1的小数，表示使用磁盘空间的比例。

1.  修改磁盘水位配置。

    在**服务配置**区域的**storage**页签，修改如下参数。

    ![storage](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0257459951/p161207.png)

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


## 访问OSS bucket

在EMR集群中访问同账号、同区域的OSS Bucket时，默认支持免密访问，即无需配置任何AccessKey即可访问。如果访问非以上情况的OSS Bucket需要配置相应的AccessKey ID、AccessKey Secret以及Endpoint，针对两种使用方式相应的配置分别如下：

-   OSS Scheme
    1.  在**集群服务** \> **SmartData**的**配置**页面，单击**smartdata-site**页签。
    2.  单击**自定义配置**，在**新增配置项**对话框中增加以下参数，单击**确定**。

        |参数|参数说明|
        |--|----|
        |**fs.jfs.cache.oss-accessKeyId**|表示存储后端OSS的AccessKey ID。|
        |**fs.jfs.cache.oss-accessKeySecret**|表示存储后端OSS的AccessKey Secret。|
        |**fs.jfs.cache.oss-endpoint**|表示存储后端OSS的endpoint。|

-   JFS Scheme
    1.  在**集群服务** \> **SmartData**的**配置**页面，单击**bigboot**页签。
    2.  修改**jfs.namespaces**为**test**。
    3.  单击**自定义配置**，在**新增配置项**对话框中增加以下参数，单击**确定**。

        |参数|参数说明|
        |--|----|
        |**jfs.namespaces.test.oss.uri**|表示test命名空间的后端存储。示例：oss://<oss\_bucket.endpoint\>/<oss\_dir\>。 endpoint信息直接配置在oss.uri中。 |
        |**jfs.namespaces.test.oss.access.key**|表示存储后端OSS的AccessKey ID。|
        |**jfs.namespaces.test.oss.access.secret**|表示存储后端OSS的AccessKey Secret。|


-   OSS Scheme
-   JFS Scheme
    1.  在**集群服务** \> **SmartData**的**配置**页面，单击**namespace**页签。

## 高级配置

Cache模式还包含一些高级配置，用于性能调优，以下配置均为客户端配置，修改后无需重启SmartData服务。

-   在**服务配置**区域的**client**页签，配置以下参数。

    |参数|参数说明|
    |--|----|
    |**client.oss.upload.threads**|每个文件写入流的OSS上传线程数。默认值：4。|
    |**client.oss.upload.max.parallelism**|进程级别OSS上传总并发度上限，防止过多上传线程造成过大的带宽压力以及过大的内存消耗。默认值：16。|

-   在**服务配置**区域的**smartdata-site**页签，配置以下参数。

    |参数|参数说明|
    |--|----|
    |**fs.jfs.cache.copy.simple.max.byte**|rename过程使用普通copy接口的文件大小上限（小于阈值的使用普通 copy接口，大于阈值的使用multipart copy接口以提高copy效率）。 **说明：** 如果确认已开通OSS fast copy功能，参数值设为**-1**，表示所有大小均使用普通copy接口，从而有效利用fast copy获得最优的rename性能。 |
    |**fs.jfs.cache.write.buffer.size**|文件写入流的buffer大小，参数值必须为2的幂次，最大为8MB，如果作业同时打开的写入流较多导致内存使用过大，可以适当调小此参数。默认值：1048576。|
    |**fs.oss.committer.magic.enabled**|启用Jindo Job Committer，避免Job Committer的rename操作，来提升性能。默认值：true。 **说明：** 针对Cache模式下，由于OSS这类对象存储rename操作性能较差的问题，推出了Jindo Job Committer。 |


