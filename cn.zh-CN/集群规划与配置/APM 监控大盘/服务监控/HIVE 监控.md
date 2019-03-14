# HIVE 监控 {#concept_nqk_xts_qgb .concept}

## HIVE 监控概览页 {#section_lqv_25s_qgb .section}

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123389/155255094638648_zh-CN.png)

HIVE 监控概览页展示了该集群 HIVE 服务相关监控的概览信息，包括：HIVE 服务相关基础指标监控图片、HIVE 服务相关的最近异常和告警列表、MetaStore 状态列表、HiveServer2 状态列表。整体内容与 HDFS 服务和 YARN 服务监控概览页类似。状态列表均支持回放功能。

## HIVE MetaStore 监控详情页面 {#section_y22_l5s_qgb .section}

在 HIVE 服务监控概览页面，单击对应 metastore 的主机名称，可以进入 metastore 服务监控详情页面：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123389/155255094638649_zh-CN.png)

-   HIVE MetaStore 进程 JVM 指标

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123389/155255094638650_zh-CN.png)

    详细监控指标项和含义，与[HDFS 监控 NameNode进程JVM指标](cn.zh-CN/集群规划与配置/APM 监控大盘/服务监控/HDFS 监控.md#ul_bvw_5q4_qgb)章节类似，支持自定义时间范围区间和时间粒度。

-   HIVE MetaStore 进程文件描述符信息

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123389/155255094638651_zh-CN.png)

    HIVE metastore进程的文件描述符数据：

    -   Max File Descriptor：metastore 进程能使用的最大文件描述符数目
    -   Open File Descriptor：metastore 进程已经使用的文件描述符数目
-   HIVE MetaStore进程的线程数目统计

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123389/155255094638652_zh-CN.png)

    -   Waiting Thread Num
    -   Blocked Thread Num
    -   Terminated Thread Num
    -   New Thread Num
    -   Daemon Thread Num
    -   Deadlock Threaad Num
    -   Timed Waiting Thread Num
    -   Runnable Thread Num
-   HIVE MetaStore 进程启停历史

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123389/155255094638653_zh-CN.png)

    表格具体含义，参考[HDFS 监控 NameNode进程启停操作历史](cn.zh-CN/集群规划与配置/APM 监控大盘/服务监控/HDFS 监控.md#NameNode_history)。


##  HIVE hiveserver2 监控详情页面 {#section_ndh_kvs_qgb .section}

在 HIVE 服务概览页面，单击 hiveserver2 组件列表中的主机名称，进入 hiveserver2 监控详情页面：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123389/155255094638654_zh-CN.png)

-   HIVE hiveserver2 进程 JVM 指标

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123389/155255094638655_zh-CN.png)

    详细监控指标项和含义，与[HDFS 监控 NameNode进程JVM指标](cn.zh-CN/集群规划与配置/APM 监控大盘/服务监控/HDFS 监控.md#ul_bvw_5q4_qgb)类似，支持自定义时间范围区间和时间粒度。

-   HIVE hiveserver2 进程文件描述符信息

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123389/155255094638656_zh-CN.png)

    参考 [HIVE MetaStore 进程文件描述符信息](#Hive_MetaStore)章节的说明

-   HIVE hiveserver2 进程线程信息

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123389/155255094638657_zh-CN.png)

    参考 [HIVE MetaStore 进程的线程数目统计](#hive_Metastore_thread)章节说明

-   HIVE hiveserver2 进程启停历史

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123389/155255094738659_zh-CN.png)

    表格具体含义，参考[HDFS 监控 NameNode进程启停操作历史](cn.zh-CN/集群规划与配置/APM 监控大盘/服务监控/HDFS 监控.md#NameNode_history)。


