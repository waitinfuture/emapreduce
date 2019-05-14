# APM 仪表盘 {#concept_rxg_wbz_pgb .concept}

仪表盘是您登录[阿里云 E-MapReduce 控制台](https://emr.console.aliyun.com/)打开监控大盘页面之后看到的默认视图，其展示了您所登录账号在指定地域中的所有集群的概览信息。

## 仪表盘顶部图表 {#section_afv_n12_qgb .section}

![仪表盘顶部图表](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/122847/155781644738433_zh-CN.png)

-   告警数量

    默认展示所登录账号指定地域中所有集群当天各小时段的告警数量柱状图。

    ![告警数量](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/122847/155781644738434_zh-CN.png)

    单击右上角放大按钮，可选择展示的时间范围和数据点粒度。

    ![选择时间](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/122847/155781644738435_zh-CN.png)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/122847/155781644738436_zh-CN.png)

    **说明：** 仪表盘中的所有小图均支持放大和自定义时间范围、数据点粒度功能，后续章节不再赘述。

-   作业状态

    显示当天作业运行状态（该账号该地域下所有集群的），默认为每小时一个数据点。同时展示了昨日的统计数据。

    ![作业状态](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/122847/155781644738437_zh-CN.png)

-   计算资源使用量（YARN）

    **说明：** 这里的计算资源只统计了 YARN 的Memory 和 VCore 数据，默认为每小时一个数据点。

    显示当天的集群计算资源用量情况（该账号该地域下所有集群的），图表下方展示了该地域下总的计算资源情况。

    ![计算资源使用量](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/122847/155781644838438_zh-CN.png)

-   存储资源使用量（HDFS）

    **说明：** 这里的存储资源为集群 HDFS 的存储容量不是集群所有主机的磁盘容量。

    显示当天的集群存储资源使用情况（该账号该地域下所有集群的），默认为每小时一个数据点。图表下方显示了存储资源周同比和日环比数据信息。

    ![存储资源使用量](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/122847/155781644838439_zh-CN.png)


## 近期事件列表 {#section_imf_qc2_qgb .section}

仪表盘页面的近期事件列表，展示了该账号该地域下所有集群中最近的严重异常事件信息列表。

![近期事件列表](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/122847/155781644938440_zh-CN.png)

## 集群状态 {#section_nsm_bd2_qgb .section}

监控大盘仪表盘页面的集群状态，展示了该账号该地域下所有集群的列表，同时展示了每个集群当天产生的告警数量。通过该列表可以快速进入集群的监控详情和集群管理页面。

![集群状态](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/122847/155781644938441_zh-CN.png)

## 近期作业 {#section_g5n_hd2_qgb .section}

监控大盘仪表盘页面的近期作业，展示了该账号该地域下所有集群上最近运行的且有优化建议的作业列表，通过该列表可以查看相关作业的详细优化建议。

![近期作业](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/122847/155781644938442_zh-CN.png)

