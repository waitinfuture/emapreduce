# HDFS 监控 {#concept_nyp_zyn_qgb .concept}

## HDFS 监控概览页 {#section_tqb_fzn_qgb .section}

![HDFS 监控概览页](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123379/154996076838590_zh-CN.png)

HDFS 监控概览页面展示 HDFS 服务相关状态的概览情况，包括 HDFS 核心指标数据图表、HDFS 服务相关的异常和告警、HDFS overview 信息、HDFS DataNodevolume failures、HDFS Namenode状态列表和HDFS DataNode状态列表。

-   HDFS 基础指标数据图表

    ![HDFS 基础指标数据图表](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123379/154996076838591_zh-CN.png)

    -   告警数据

        默认展示当天的与 HDFS 服务相关的告警数据。

    -   HDFS 容量

        显示该集群 HDFS 容量使用情况，默认显示当天的数据，右上角可以对图表进行放大，选择不同的时间区间和粒度查看。展示的数据包括 HDFS Total Capacity、HDFS Used Capacity、HDFS Remaining Capacity和Non DFS Used Capacity。

    -   HDFS Blocks

        显示该集群 HDFS Blocks 统计数据，默认显示当天的数据，右上角可以对图表进行放大，选择不同的时间区间和粒度查看。展示的指标数据包括 Block Capacity、Block Total、Corrupted Blocks、Excessed Blocks、Missing Blocks、Pending Deletion、Pending Replication、Postponed Misreplicated、Scheduled Replication、Under Replicated。

    -   HDFS 文件总数

        显示该集群 HDFS 上文件总数目，默认显示当天的，可以单击图表右上角进行放大选择不同的时间区间和粒度查看。

-   HDFS 相关告警

    ![HDFS 相关告警](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123379/154996076838593_zh-CN.png)

    展示该集群当天的与HDFS服务相关的严重异常事件。

-   HDFS Overview 信息

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123379/154996076838594_zh-CN.png)

    显示的内容包括：

    -   HDFS 服务启动时间
    -   HDFS 服务软件版本
    -   HDFS 服务编译的代码分支
    -   集群 ID
    -   Block Pool ID
    -   配置的 HDFS 容量
    -   已经使用的 HDFS 容量
    -   已经使用的 Non DFS 容量
    -   剩余的 HDFS 容量
    -   各个 DataNode 上 HDFS 容量使用情况，包括最小值、中位数值、最大值和方差，可以根据此判断整个HDFS集群的数据容量是否分布均衡
    -   存活的节点数
    -   死亡的节点数
    -   迁移中的（Decommissioning）节点数
    -   总的Datanode Volume Failure 数目
    HDFS Overview信息支持回放功能，可以在排查问题时候回放到历史的时间节点。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123379/154996076938595_zh-CN.png)

-   HDFS Datanode Volume Failures 信息

    Datanode Volume Failures 列出具体的 Volume Failure 的列表信息。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123379/154996076938596_zh-CN.png)

-   HDFS NameNode 状态列表

    NameNode 状态列表，列出了当前 NameNode 列表以及当前的状态，包括主机名、主备状态、是否进入安全模式、当前端口状态、NameNode 进程当前的 CPU 使用率、NameNode 进程当前的内存使用情况、NameNode 进程当前的 JVM GC 统计情况（以 jstat -gcutil 格式）、NameNode 服务端口当前的 RPC 处理时间和排队时间。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123379/154996076938597_zh-CN.png)

    -   主机名称

        当前 NameNode 的主机名称

    -   主备状态

        当前 NameNode 的主备状态，HA 集群会有 Active 和 Standby 区分，非 HA 集群正常都是 Active 状态

    -   是否进入安全模式
    -   端口状态

        显示当前 NameNode 进程的端口是否正常，绿色表示正常，红色表示异常

    -   Process CPU Utilization

        当前 namenode 进程的 CPU 使用率

    -   Memory

        当前 NameNode 进程的内存使用情况，包括 Heap Committed、Heap Init、Heap Max、Heap Used、NonHeap Committed、NonHeap Init、NonHeap Used

    -   GC Util

        使用 jstat -gcutil 的格式展示了当前 NameNode java 进程的 GC 统计数据：

        -   O: Old space utilization as a percentage of the space's current capacity
        -   E: Eden space utilization as a percentage of the space's current capacity.
        -   YGCT: Full garbage collection time.
        -   FGCT: Young generation garbage collection time.
        -   GCT: Total garbage collection time.
        -   YTC: Number of young generation GC events.
        -   FGC: Number of full GC events.
    -   RPC Call Queue Length

        显示当前 NameNode RPC 端口上的调用队列长度

    -   RPC Processing Time

        显示当前 NameNode RPC端口上的处理时间

    -   RPC Queue Time

        显示当前 NameNode RPC端口上的排队时间

    NameNode状态列表支持回放功能。

-   HDFS DataNode状态列表

    DataNode 状态列表会列出集群所有 DataNode 节点的状态，包括信息：

    -   Node： DataNode节点名称，支持前端页面排序（当前分页页面排序）
    -   Last Contact： 最近一次心跳是多少秒之前
    -   Admin State：DataNode节点状态，可能的取值为In Service、Decommission In Progress、Decommissioned、Entering Maintenance、In Maintenance。支持前端状态过滤（当前分页页面过滤）
    -   Capacity：当前 DataNode 配置的 HDFS 容量，支持前端页面排序；
    -   Used：当前 DataNode 已经使用的 HDFS 容量，支持前端页面排序；
    -   Non DFS Used：当前 DataNode 已经使用的Non DFS容量，支持前端页面排序；
    -   Remaining：当前 DataNode 剩余的 HDFS 容量，支持前端页面排序；
    -   Blocks：当前 DataNode 上 block 数量，支持前端页面排序；
    -   Block Pool Used：当前 DataNode 上 block pool 的使用量，支持前端页面排序；
    -   Failed Volumes：当前 DataNode上 failed volume 数量，支持前端页面排序；
    -   Version：HDFS 部署版本信息
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123379/154996076938598_zh-CN.png)

    DataNode 状态列表支持分页和选择每页显示条目的数量：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123379/154996076938599_zh-CN.png)

    DataNode状态列表同时支持搜索固定主机名和回放功能：![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123379/154996076938600_zh-CN.png)HDFS 的 NameNode Journal Status 和 NameNode Storage 监控功能正在开发中。


## HDFS NameNode 监控详情页 {#section_f41_pg4_qgb .section}

在 HDFS 监控概览页，单击 NameNode 状态列表的主机名，可以进入对应 NameNode 监控详情页面：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123379/154996076938601_zh-CN.png)

-   NameNode 进程 JVM 指标
    -   NameNode 进程 JVM GC 不同内存分区统计情况

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123379/154996076938603_zh-CN.png)

        -   S0：存活区0（Survivor 0）容量使用百分比
        -   S1：存活区1（Survivor 1）容量使用百分比
        -   E：Eden 区容量使用百分比
        -   O：老年代区（Old）容量使用百分比
        -   M：元数据区域（Metaspace）容量使用百分比
        -   CCS：压缩类区域（Compressed class space ）容量使用百分比
        图表支持自定义选择时间颗粒度和时间范围。

    -   NameNode 进程 GC 时间统计![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123379/154996076938607_zh-CN.png)

        -   YGCT：Young GC累计时间
        -   FGCT：Full GC累计时间
        -   GCT：总的GC累计时间
        图表支持自定义选择时间颗粒度和时间范围。

    -   NameNode 进程 GC 次数统计

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123379/154996076938608_zh-CN.png)

        -   YCG\_count: Young GC累计次数
        -   FGC\_count: Full GC累计次数
        图表支持自定义选择时间颗粒度和时间范围。

    -   NameNode 进程 heap memory统计

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123379/154996076938609_zh-CN.png)

        -   Heap Memory Max
        -   Heap Memory Init
        -   Heap Memory Init
        -   Heap Memory Init
        图表支持自定义选择时间颗粒度和时间范围。

    -   NameNode 进程 non-heap memory统计![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123379/154996076938610_zh-CN.png)

        -   Non Heap Memory Init
        -   Non Heap Memory Committed
        -   Non Heap Memory Used
        图表支持自定义选择时间颗粒度和时间范围。

-   NameNode 进程文件描述符统计信息

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123379/154996077038611_zh-CN.png)

    -   Max File Descriptor：进程可以使用的最大文件描述符数目
    -   Open File Descriptor：进程已经占用的文件描述符数目
    图表支持自定义选择时间颗粒度和时间范围。

-    NameNode 进程RPC性能指标

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123379/154996077038614_zh-CN.png)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123379/154996077038615_zh-CN.png)

    -   Call Queue Length: Current length of the call queue，当前 namenode RPC 端口上的RPC调用队列长度，可以反应RPC的请求处理的堆积情况；
    -   Received Bytes： Total number of received bytes，当前 namenode RPC 端口上总的接收数据量大小；
    -   Sent Bytes：Total number of sent bytes，当前namenode RPC端口上总的发送数据量大小；
    -   Open Connections：当前 namenode PRC端口上打开的连接数；
    -   Average Queue Time：Average queue time in milliseconds，RCP 请求的平均排队时间；
    -   Average Processing Time：Average Processing time in milliseconds，RPC 请求的平均处理时间；
-   NameNode 进程启停操作历史

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123379/154996077038616_zh-CN.png)

    所有通过 EMR 控制台对进程的启动、停止操作以及进程由于异常退出被 EMR agent 自动拉起的记录，都会在这里列出。

    -   时间：操作发生的时间点
    -   启动/重启/停止：说明本次对组件操作的类型，包括启动、停止和重启；
    -   是否自动拉起：说明本次操作是否是有 EMR 的保活机制自动拉起，对于异常退出的组件，EMR Agent 自动拉起保证服务的可用性；
    -   启动用户：本次操作的 linux 用户，对于停止操作无该信息；
    -   PID：本次操作产生的进程的进程 ID，对于停止操作无该信息；
    -   PID：本次操作产生的进程的进程 ID，对于停止操作无该信息；
    -   详细参数：本次操作产生的进程的详细启动参数，对于停止操作无该信息；

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123379/154996077038617_zh-CN.png)


## HDFS DataNode 监控详情页 {#section_y53_s1p_qgb .section}

在 HDFS 监控概览页，单击 DataNode 状态列表的主机名，可以进入对应 DataNode 监控详情页面：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123379/154996077038618_zh-CN.png)

-   DataNode 核心指标

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123379/154996077038619_zh-CN.png)

    -   Bytes Read/Written: DataNode 从上次启动之后数据的读写量
    -   Block Operation Count: DataNode 的block操作数统计
        -   Blocks Written
        -   Blocks Written
        -   Blocks Replicated
        -   Blocks Removed
        -   Blocks Verified
        -   Blocks Verified Failures
        -   Blocks Verified Failures
        -   Blocks Uncached
    -   Operation time: 操作时间统计
        -   Read Block Operation Average Time
        -   Read Block Operation Average Time
        -   Block Checksum Operation Average Time
        -   Copy Block Operation Average Time
        -   Replace Block Operation Average Time
        -   Heartbeats Average Time
        -   Block Report Average Time
        -   IncrementalBlock Report Average Time
        -   Cache Report Average Time
        -   Packet Ack Round Trip Average Time
        -   Flush Operation Average Time
        -   Fsync Operation Average Time
        -   Send Data Packet Blocked on Network Average Time: Average waiting time of sending packets in nanoseconds
        -   Send Data Packet Transfer Average Time: Average transfer time of sending packets in nanoseconds
-   DataNode 进程启停历史

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123379/154996077038620_zh-CN.png)

    表格具体含义，参考 [NameNode 进程启停操作历史说明](#NameNode_history)。


