# DRUID 监控 {#concept_w1c_vlt_qgb .concept}

## DRUID 服务监控概览页面 {#section_hln_bmt_qgb .section}

DRUID 服务监控概览页面，展示了 DRUID 服务基础指标图表、近期告警与异常、核心组件状态列表（包括Broker、Coordinator、Historical、Overlord、Middle Manager）以及DRUID Query性能指标和Druid Ingestion性能指标。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123488/154996097938669_zh-CN.png)

-   DRUID 服务基础指标图表

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123488/154996097938670_zh-CN.png)

    DRUID基础指标图表包括：

    -   DRUID 服务告警数目

        默认展示当天的数量曲线图，显示昨日总量和今日总量。图表右上角图标支持放大查看和自定义选择时间范围和时间粒度；

    -   DRUID Task数目

        默认展示当天的task（realtime 节点起的 indexing task）数量曲线图，显示昨日总量和今日总量。图表右上角图标支持放大查看和自定义选择时间范围和时间粒度；

    -   Segment数量

        默认展示当天 segment 数量曲线图，图表右上角图标支持放大查看和自定义选择时间范围和时间粒度；

    -   Historical 缓存用量

        默认展示当天 historical 缓存用量曲线图，图表右上角图标支持放大查看和自定义选择时间范围和时间粒度；

-   DRUID 服务最近异常和告警

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123488/154996098038671_zh-CN.png)

    默认展示该集群最近一天 DRUID 服务相关的异常和告警（如果有的话）；

-   DRUID 服务 Broker 节点状态列表

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123488/154996098038672_zh-CN.png)

    -   主机名称：broker 节点的主机名称，可点击，是 broker 组件监控详情页面入口
    -   端口状态：broker 端口状态，绿色为可用，红色为不可用
    -   Process CPU Utilization：broker 组件的 CPU使用率
    -   Heap Memory：
        -   Heap Memory Init
        -   Heap Memory Committed
        -   Heap Memory Used
        -   Heap Memory Used
    -   Non Heap Memory：
        -   Non Heap Memory Init
        -   Non Heap Memory Committed
        -   Non Heap Memory Used
        -   Non Heap Memory Max
    -   GC Util: 以 jstat -gcuti l格式打印 broker 进程的 GC 统计数据
    DRUID 服务 Broker 节点状态列表支持指定主机名搜索过滤和回放功能。主机名称可点击，是对应broker监控详情页面入口。列表支持分页。

-   DRUID服务 Coordinator 节点状态列表

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123488/154996098038673_zh-CN.png)

    具体表格内容说明，参考 [DRUID 服务 Broker 节点状态列表](#Druid_service_status)章节。

-   DRUID 服务 Historical 节点状态列表

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123488/154996098038677_zh-CN.png)

    具体表格内容说明，参考 [DRUID 服务 Broker 节点状态列表](#Druid_service_status)章节。

-   DRUID 服务 Overlord 节点状态列表

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123488/154996098038679_zh-CN.png)

    具体表格内容说明，参考 [DRUID 服务 Broker 节点状态列表](#Druid_service_status)章节。

-   DRUID 服务 Middle Manager 节点状态列表

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123488/154996098038680_zh-CN.png)

    具体表格内容说明，参考 [DRUID 服务 Broker 节点状态列表](#Druid_service_status)章节。

-   Druid Queries 核心性能指标

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123488/154996098038681_zh-CN.png)

    展示 Druid 查询的性能指标，Average Queries 是指一分钟的平均 query 次数，Average Qeury Time 表示平均每次 query 的耗时，可以选择不同的 datasource 和 query 类型，默认是所有 datasource 和所有 query 类型：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123488/154996098038682_zh-CN.png)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123488/154996098038683_zh-CN.png)

    Druid query 核心性能指标图表可以自定义选择不同的时间范围区间和时间聚合粒度。

-   Druid Ingestion 核心性能指标

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123488/154996098038685_zh-CN.png)

    Druid Ingestion 性能指标，展示了 Druid 集群在数据摄取方面的性能，Events Processed 和 Rows Output 都是展示一分钟内的次数，可以根据不同的 datasource 进行过滤：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123488/154996098038686_zh-CN.png)

    Druid Ingestion 核心性能指标曲线图可以自定义选择时间区间范围和时间聚合粒度。


## Druid 各组件监控详情页面 {#section_cr3_r25_qgb .section}

在 DRUID 服务监控概览页面，单击个组件状态列表中的主机名，可以进入个组件（包括 Broker、Coordinator、Historical、Overlord、Middle Manager）监控详情页面：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123488/154996098038687_zh-CN.png)

详情页面包括组件进程 JVM 指标监控、组件进程文件描述符监控和组件进程的启停历史。监控详情页面内容说明与其它服务组件监控详情页面类似。

