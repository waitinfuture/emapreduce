# KAFKA 监控 {#concept_jjd_cf5_qgb .concept}

## KAFKA服务监控概览页面 {#section_uwz_2f5_qgb .section}

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123496/155255101638688_zh-CN.png)

KAKFA 服务监控概览页面展示了 KAKFA 基础指标图表、KAKFA 服务最近一天的异常和告警列表和 KAKFA broker状态列表。

KAKFA broker状态列表列出了 broker 所在主机的 CPU 和内存使用情况，以及 broker 进程的 JVM 内存使用情况和文件描述符使用情况。主机名称可点击，是 broker 监控详情页面的入口。

## KAFKA Broker 监控详情页面 {#section_zxr_dg5_qgb .section}

-   KAFKA Broker 进程 JVM 指标

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123496/155255101638689_zh-CN.png)

-   KAFKA Broker 进程文件描述符信息

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123496/155255101638690_zh-CN.png)

    broker 文件描述符展示了 broker 进程可以使用的最大文件描述符数目和当前已经使用的文件描述符数目。

-   KAFKA Broker 进程核心性能指标

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123496/155255101738691_zh-CN.png)

    Broker核心性能指标包括：

    -   Failed Requests
        -   failed fetch requests per second
        -   failed fetch requests per second
    -   BytesIn/BytesOut
        -   bytes in per second
        -   bytes in per second
    -   Offline Count
        -   offline log directory
        -   offline replica
        -   offline partitiions
    -   Replica Manager Disk Usage
        -   max usage
        -   max usage
        -   mean usage
        -   mean usage
    -   Messsage
        -   message in per second
    -   Total Fetch Request
        -   total fetch requests per second
-   KAFKA Broker 进程启停历史

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123496/155255101738692_zh-CN.png)

    表格具体含义，参考[HDFS 监控 NameNode进程启停操作历史](cn.zh-CN/集群规划与配置/APM 监控大盘/服务监控/HDFS 监控.md#NameNode_history)。


