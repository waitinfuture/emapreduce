# Zookeeper 监控 {#concept_nv5_sxs_qgb .concept}

## Zookeeper 监控概览页面 {#section_t3n_1ys_qgb .section}

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123391/155255096738660_zh-CN.png)

Zookeeper 服务监控概览页面，展示了该集群 Zookeeper 服务相关的最近的告警和异常信息，以及 Zookeeper 各个节点状态的列表。Zookeeper各个节点状态列表包括：

-   主机名称：Zookeeper 节点的主机名，主机名可点击，是该 Zookeeper 节点监控详情页面入口；
-   主从状态：展示该 Zookeeper 当前的角色（即 leader 还是 follower）；
-   端口状态：展示该 Zookeeper 节点上端口的状态，绿色表示可用，红色表示不可用；
-   CPU 和内存信息：展示该 Zookeeper 节点上的 CPU 和内存使用情况。

Zookeeper 节点状态列表支持回放功能。

## Zookeeper 节点监控详情页面监控 {#section_qjz_kys_qgb .section}

在 Zookeeper 服务监控概览页面，单击 Zookeeper 节点状态列表中的主机名可以进入具体 Zookeeper 节点的监控详情页面：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123391/155255096738661_zh-CN.png)

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123391/155255096738662_zh-CN.png)

-   Zookeeper 核型指标
    -   Latency
        -   Min Latency：最小延时
        -   Min Latency：最小延时
        -   Average Latency：平均延时
    -   Packets
        -   Packets Received：收到的数据包数目
        -   Packets Sent：发送的数据包数目
    -   Alive Connections：活跃的连接数目
    -   Outstanding Connections：堆积的连接数
    -   File Descriptors
        -   Max File Descriptor：zookeeper进程最大能使用的文件描述符数目
        -   Open File Descriptor：zookeeper进程已经使用的文件描述符数目
-   Zookeeper进程启停历史

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123391/155255096738664_zh-CN.png)

    表格具体含义，[HDFS 监控 NameNode进程启停操作历史](cn.zh-CN/集群规划与配置/APM 监控大盘/服务监控/HDFS 监控.md#NameNode_history)。


