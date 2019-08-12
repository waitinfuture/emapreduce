# Kafka集群磁盘异常处理 {#concept_wmq_jzt_xfb .concept}

常见磁盘异常包括磁盘写满和磁盘损坏，本节介绍这两种磁盘异常的解决方法。

## 磁盘写满 {#section_up1_tzt_xfb .section}

磁盘写满的解决方法如下：

1.  登录相应的机器。
2.  查找到写满的磁盘，然后按照以下原则清理数据。
    -   切勿直接删除Kafka的数据目录，否则会丢失所有数据。
    -   切勿清理Kafka的系统topic，例如consumer\_offsets和schema等。
    -   查找到占用空间较多或者明确不需要的topic，选择其中某些partition，从最早的日志数据开始删除。删除segment及相应的index和timeindex文件。
3.  重启这台机器的Kafka broker服务。

## 磁盘损坏 {#section_d35_p15_xfb .section}

磁盘损坏的解决方法如下：

-   磁盘损坏不超过25%时，无需处理。
-   磁盘损坏超过25%时，选择迁移机器的方式来处理。请提交工单到E-MapReduce，工程师将介入支持。

