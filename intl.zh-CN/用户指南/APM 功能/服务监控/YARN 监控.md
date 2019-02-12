# YARN 监控 {#concept_z25_ddp_qgb .concept}

## YARN监控概览页 {#section_k3h_4fp_qgb .section}

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123380/154996089938621_zh-CN.png)

YARN 监控概览页面包括集群 YARN 服务的基础指标图表、集群 YARN 服务最近的重要异常和告警、ResourceManage r状态列表、NodeManage r状态列表、JobHistory状态列表、YARN Scheduler Queue使用情况监控；

-   YARN 基础指标数据图表

    默认展示当天的统计数据，可以单击小图右上角放大按钮进行放大展示，自定义展示时间区间和粒度。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123380/154996089938624_zh-CN.png)

    -   告警数据

        展示集群 YARN 服务今天的告警统计数据

    -   VCores

        展示集群 YARN VCore 今天的统计数据，包括 Allocated VCores、Available VCores、Reserved VCores

    -   Memory

        展示集群 YARN Memory 今天的统计数据，包括 Allocated Memory、Available Memory、Reserved Memory

    -   NodeManager 分布数据统计

        包括：

        -   Active NodeManager 数量
        -   Decommissioned NodeManager 数量
        -   Rebooted NodeManager 数量
        -   Unhealthy NodeManager 数量
    -   Pending Resource 统计数据

        包括：

        -   Pending VCores
        -   Pending Memory
        -   Pending Containers
    -   Application 数据统计

        包括：

        -   Submitted Apps
        -   Running Apps
        -   Pending Apps
        -   Killed Apps
        -   Failed Apps
        -   Completed Apps
    -   YARN Container数据统计

        包括：

        -   Allocated Container 数目
        -   Reserved Container 数目
-   YARN最近告警

    展示该集群当天的与 YARN 服务相关的严重异常事件。

-   YARN ResourceManager 状态列表

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123380/154996089938625_zh-CN.png)

    ResourceManager 状态列表展示该集群当前最新的状态数据，包括：

    -   主机名：该 ResourceManager 的主机名，主机名可点击，是 ResourceManager 监控详情页面的入口；
    -   主备状态：对于 HA 集群，会有 Active 和 Standby 两种状态，非 HA 集群一般都会是 Active 状态；
    -   端口状态：展示 ResourceManager 进程所有相关端口的状态，绿色为端口可用，红色为端口不可用；
    -   GC Util: 以 jstat -gcutil 格式打印 ResourceManager 进程的 GC 统计数据；
    -   RPC Call Queue Length：展示当前 ResourceManager 各个 RPC 端口上的 RCP 调用队列长度；
    -   RPC Processing Time：展示当前 ResourceManager 各个 RPC 端口上的 RCP 请求平均处理时长；
    -   PRC Queue Time：展示当前 ResourceManager 各 个RPC 端口上的 RCP 请求平均排队时长；
    ResourceManager 状态列表支持回放功能。

-   YARN NodeManager 状态列表

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123380/154996089938626_zh-CN.png)

    NodeManager 状态列表展示当前集群各个 NodeManager 最新的状态信息，包括：

    -   主机名称：NodeManager 所在主机的主机名称，主机名称可点击，是具体 NodeManager 监控的详情页面的入口；
    -   状态：NodeManager 当前状态，NodeManager 状态的可能取值为 LOST、RUNNING、UNHEALTHY 三种；
    -   Rack：NodeManager 所在的机架信息；
    -   Node Address
    -   Node HTTP Address
    -   Last Health Update：最后一次心跳时间
    -   Health Report：健康报告，如果 NodeManager 异常这里会展示相应内容；
    -   Containers：当前 NodeManager 上的 container 数目；
    -   Memory Used
    -   Memory Available
    -   VCores Used
    -   VCores Available
    NodeManager状态列表支持回放功能，支持根据主机名过滤搜索功能，支持分页：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123380/154996090038627_zh-CN.png)

-   YARN JobHistory状态列表

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123380/154996090038628_zh-CN.png)

    JobHistory 状态列表列出集群所有 JobHistory 列表和最新状态，包括：

    -   主机名称：当前 JobHistory 所在主机的主机名称，主机名称可点击，是当前 JobHistory 监控详情页面的入口；
    -   端口状态：展示当前 JobHistory 端口的当前状态，绿色表示可用，红色表示不可用
    -   Process CUP Utilization：当前 JobHistory 进程的 CPU 使用量；
    -   Heap Memory：当前 JobHistory 进程的内存使用情况统计，包括 Heap Memory Used、Heap Memory Committed、Heap Memory Max、Heap Memory Init；
    -   Non Heap Memory：当前 JobHistory 进程的内存使用情况统计，包括 Non Heap Memory Used、Non Heap Memory Committed、Non Heap Memory Max、Non Heap Memory Init；
    -   GC Util：以 jstat -gcutil 格式展示当前 JobHistory 进程的 GC 统计数据。
    JobHistory状态列表支持回放功能。

-   YARN Scheduler Queue 实时状态和详情

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123380/154996090038629_zh-CN.png)

    Scheduler Queue 状态图展示了当前集群的 YARN Scheduler 各个队列资源的详细使用情况，单击具体的队列，可以展示当前队列的详情：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123380/154996090038630_zh-CN.png)

    YARN Scheduler Queue 实时状态和详情支持回放功能，同时支持Capacity Scheduler 和 Fair Scheduler 两种调度器：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123380/154996090038631_zh-CN.png)


## YARN ResourceManager 监控详情页 {#section_wph_zns_qgb .section}

在YARN 监控概览页面，单击 ResouceManager 状态列表的主机名，可以进入 ResourceManager 监控详情页：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123380/154996090038632_zh-CN.png)

-   ResourceManager 进程 JVM 指标

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123380/154996090038633_zh-CN.png)

    详细监控指标项和含义，与 [HDFS 监控 NameNode进程JVM指标](intl.zh-CN/用户指南/APM 功能/服务监控/HDFS 监控.md#ul_bvw_5q4_qgb)章节类似，支持自定义时间范围区间和时间粒度。

-   ResourceManager 进程文件描述符数目统计

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123380/154996090038634_zh-CN.png)

    展示 ResourceManager 进程能使用的最大文件描述符数目和当前已经使用的文件描述符数目，支持自定义时间范围区间和时间粒度。

-   ResourceManager RPC 性能指标统计

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123380/154996090038635_zh-CN.png)

    展示 ResourceManager 不同端口上的 RPC 关键性能指标，具体指标含义与[HDFS 监控 NameNode 进程RPC性能指标](intl.zh-CN/用户指南/APM 功能/服务监控/HDFS 监控.md#ul_ulg_xy4_qgb)章节类似。支持自定义时间范围区间和时间粒度。

-   ResourceManager 进程启停历史

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123380/154996090038637_zh-CN.png)

    表格具体含义，参考[HDFS 监控 NameNode进程启停操作历史](intl.zh-CN/用户指南/APM 功能/服务监控/HDFS 监控.md#NameNode_history)。


## NodeManager 监控详情页 {#section_hpj_fqs_qgb .section}

单击 YARN 服务监控概览页面的 NodeManager 状态列表中的主机名，可以进入 NodeManager 监控详情页面。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123380/154996090038638_zh-CN.png)

-   NodeManager Container 监控详情

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123380/154996090038639_zh-CN.png)

    包括：

    -   Containers Allocated
    -   Containers Completed
    -   Containers Failed
    -   Containers Initing
    -   Containers Killed
    -   Containers Launched
    -   Containers Running
-   NodeManager进程启停历史

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123380/154996090138640_zh-CN.png)

    表格具体含义，[HDFS 监控 NameNode进程启停操作历史](intl.zh-CN/用户指南/APM 功能/服务监控/HDFS 监控.md#NameNode_history)。


## JobHistory 监控详情页 {#section_yxb_2rs_qgb .section}

通过单击 HDFS 监控概览页面的 JobHistory 状态列表的主机名称，可以进入 JobHistory监控详情页面：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123380/154996090138641_zh-CN.png)

-   JobHistory进程 JVM 指标

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123380/154996090138642_zh-CN.png)

    详细监控指标项和含义，与 [HDFS 监控 NameNode进程JVM指标](intl.zh-CN/用户指南/APM 功能/服务监控/HDFS 监控.md#ul_bvw_5q4_qgb)章节类似，支持自定义时间范围区间和时间粒度。

-   JobHistory 进程文件描述符统计信息

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123380/154996090138643_zh-CN.png)

    展示 JobHistory 进程能使用的最大文件描述符数目和当前已经使用的文件描述符数目，支持自定义时间范围区间和时间粒度。

-   JobHistory 进程启停历史

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123380/154996090138644_zh-CN.png)

    表格具体含义，参考[HDFS 监控 NameNode进程启停操作历史](intl.zh-CN/用户指南/APM 功能/服务监控/HDFS 监控.md#NameNode_history)。


## Scheduler Queue 监控详情 {#section_cpq_2ss_qgb .section}

在 YARN 服务监控概览页面，选中 Scheduler 具体队列，展开队列详情之后，单击**查看选中 Queue 详情**可以进入 Scheduler Queue 监控详情页面：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123380/154996090138646_zh-CN.png)

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123380/154996090138647_zh-CN.png)

关键指标统计：

-   队列中运行时长统计
    -   Running\_0: Current number of running applications whose elapsed time are less than 60 minutes，即当前队列中运行时长小于 60 分钟的作业数目；
    -   Running\_60: Current number of running applications whose elapsed time are between 60 and 300 minutes，即当前队列中运行时长大于 60 分钟小于 300 分钟的作业数目；
    -   Running\_300: Current number of running applications whose elapsed time are between 300 and 1440 minutes，即当前队列中运行时长大于 300 分钟小于 1440 分钟的作业数目；
    -   Running\_1440: Current number of running applications elapsed time are more than 1440 minutes，即当前队列中运行时长大于 1440 分钟的作业数目；
-   队列 YARN VCore 信息统计
    -   Allocated VCores
    -   Reserved VCores
    -   Available VCores
    -   Pending VCores
-   队列 YARN Memory信息统计
    -   Allocated Memory
    -   Reserved Memory
    -   Available Memory
    -   Pending Memory
-   队列Container信息统计
    -   Allocated Containers
    -   Pending Containers
    -   Reserved Containers
-   活跃用户数和作业数统计
    -   Active Users
    -   Active Applications

