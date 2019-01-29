# 使用 E-MapReduce 采集 Kafka 客户端 Metrics 数据 {#concept_ach_3gj_gfb .concept}

本文介绍通过使用 E-MapReduce 产品从 Kafka 客户端采集 Metrics 数据从而有效地进行性能监控。

## 背景信息 {#section_knb_vgj_gfb .section}

Kafka 提供了一套非常完善的 Metrics 数据，覆盖 Broker，Consumer，Producer，Stream 以及 Connect 。E-MapReduce 通过 Ganglia 收集了 Kafka Broker Metrics 信息，可以很好地监控 Broker 运行状态。但完整的 Kafka 应用包括 Kafka Broker 和 Kafka 客户端这两个角色，当发生读写性能问题时，仅从 Broker 角度难以发现问题，需要结合客户端的运行状况来联合分析才行。那么 Kafka 客户端 Metrics 就是一类非常重要的数据。

## 实现原理 {#section_nqd_xgj_gfb .section}

-   如何采集 Metrics

    Metrics Kafka 提供了 Metrics Reporter 的插件扩展功能，默认提供了 JmxReporte 实现，即我们已经可以通过 JMX 工具来查看 Kafka 的 Metrics 。所以，我们可以自己实现一套 Metrics Reporter（实现org.apache.kafka.common.metrics.MetricsReporter），来自定义获取这些 Metrics。

-   如何存放 Metrics

    以上我们实现了自定义采集 Kafka Metrics，还需要选择一个存储系统将这些 Metrics存储起来，以便后续的使用和分析。考虑到 Kafka 自身就是一种存储系统，我们可以将 Metrics 存储到 Kafka 中，这样做有以下几种优势：

    -   无需第三方存储系统支持；
    -   数据很方便地接入到其他系统中；
    所以，完整的客户端 Metrics 采集方案如下图所示：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21764/154874697812649_zh-CN.png)

    E-MapReduce 提供了一个开源实现 emr-kafka-client-metrics 的接口，源码[地址](https://github.com/aliyun/aliyun-emapreduce-sdk/tree/master-2.x/external/emr-kafka)。


## 环境准备 {#section_erw_jwr_ngb .section}

-   限制条件
    -   只支持 Java 类客户端程序； 
    -   只支持 0.10 之后版本Kafka客户端 ；
-   无需自己编译，E-MapReduce 已经向 Maven 发布了Jar包，直接下载[最新版本](https://mvnrepository.com/artifact/com.aliyun.emr/emr-kafka-client-metrics?spm=a2c4e.11153940.blogcont624050.20.24d04bcauktP9S)即可。

-   本文使用阿里云EMR服务自动化搭建 Kafka 集群，详细过程请参考[创建集群](../../../../../intl.zh-CN/快速入门/创建 E-MapReduce/创建集群.md#)。

    本文使用的 EMR Kafka 版本信息如下：

    -   EMR版本: EMR-3.12.1
    -   集群类型: Kafka
    -   软件信息: Kafka-Manager \(1.3.3.16\)/Kafka \(2.11-1.0.1\)/ZooKeeper \(3.4.12\)/ Ganglia \(3.7.2\)
    -   Kafka 集群使用专有网络，区域为华东1（杭州），主实例组 ECS 计算资源配置公网及内网 IP，具体配置如下所示：

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21764/154874697912651_zh-CN.png)

    -   配置

        |配置项|说明|
        |---|--|
        |metric.reporters|使用 EMR 的实现：org.apache.kafka.clients.reporter.EMRClientMetricsReporter|
        |emr.metrics.reporter.bootstrap.servers|Metrics 存储 Kafka 集群的 bootstrap.servers|
        |emr.metrics.reporter.zookeeper.connect|Metrics 存储Kafka集群的Zookeeper 地址|

    -   如何加载 Metrics
        -   将 emr-kafka-client-metrics 的 jar 包放在客户端程序的 Classpath 可以加载到的机器上即可;
        -   直接将 emr-kafka-client-metrics 依赖打进客户端程序jar包中;

## 实施步骤 {#section_q2p_shj_gfb .section}

1.  下载最新版本 emr-kafka-client-metrics 包。

    ```
    wget http://central.maven.org/maven2/com/aliyun/emr/emr-kafka-client-metrics/1.4.3/emr-kafka-client-metrics-1.4.3.jar
    ```

2.  将 emr-kafka-client-metrics 包放到 Kafka 的 lib 中 。

    ```
    cp emr-kafka-client-metrics-1.4.3.jar /usr/lib/kafka-current/libs/
    ```

3.  创建一个测试 Topic。

    ```
    kafka-topics.sh --zookeeper emr-header-1:2181/kafka-1.0.1 --partitions 10 --replication-factor 2 --topic test-metrics  --create
    ```

4.  向测试 Topic 写入数据，这里我们将 producer 的配置项配置到本地文件 client.conf 中。 

    ```
    ## client.conf:
    metric.reporters=org.apache.kafka.clients.reporter.EMRClientMetricsReporter
    emr.metrics.reporter.bootstrap.servers=emr-worker-1:9092
    emr.metrics.reporter.zookeeper.connect=emr-header-1:2181/kafka-1.0.1
    bootstrap.servers=emr-worker-1:9092
    ## 命令：
    kafka-producer-perf-test.sh --topic test-metrics --throughput 1000 --num-records 100000 
    --record-size 1024 --producer.config client.conf
    ```

5.  查看这次客户端的 Metrics，注意默认的 Metrics Topic 是 \_emr-client-metrics 。

    ```
    kafka-console-consumer.sh --topic _emr-client-metrics --bootstrap-server emr-worker-1:9092 
    --from-beginning
    ```

    返回的消息如下所示：

    ```
    {prefix=kafka.producer, client.ip=192.168.xxx.xxx, client.process=25536@emr-header-1.cluster-xxxx, 
    attribute=request-rate, value=894.4685104965012, timestamp=1533805225045, group=producer-metrics, 
    tag.client-id=producer-1}
    ```


**说明：** 

|字段名|说明|
|---|--|
|client.ip|客户端所在机器 IP|
|client.process|客户端程序进程 ID|
|attribute|Metric 属性名|
|value|Metric 值|
|timestamp|Metric 采集的时间戳|
|tag.xxx|Metric 其他 tag 信息|

