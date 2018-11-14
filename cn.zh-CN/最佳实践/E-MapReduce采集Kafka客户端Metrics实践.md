# E-MapReduce采集Kafka客户端Metrics实践 {#concept_ach_3gj_gfb .concept}

本文介绍使用E-MapReduce产品从Kafka客户端采集Metrics数据从而有效地进行性能监控。

## 背景 {#section_knb_vgj_gfb .section}

我们知道Kafka提供一套非常完善的Metrics数据，覆盖Broker，Consumer，Producer，Stream以及Connect。E-MapReduce通过Ganglia收集了Kafka Broker metrics信息，可以很好地监控Broker运行状态。但完整的Kafka应用包括Kafka Broker和Kafka 客户端这两个角色，当发生读写性能问题时，常常从Broker角度难以发现问题，需要结合客户端的运行状况来联合分析才行。那么Kafka客户端metrics就是一类非常重要的数据。

## 实现 {#section_nqd_xgj_gfb .section}

-   如何采集

    metricsKafka提供了Metrics Reporter的插件扩展功能，默认提供了JmxReporter实现，也即我们已经可以通过JMX工具来查看Kafka的Metrics。所以，我们可以自己实现一套Metrics Reporter（实现org.apache.kafka.common.metrics.MetricsReporter），来自定义获取这些Metrics。

-   如何存放metrics

    以上我们可以实现自定义的采集Kafka Metrics，还需要选择一个存储系统将这些metrics存储起来，以便后续的使用和分析。考虑到Kafka自身就是一种存储系统，我们可以将Metrics存储到Kafka中，这有几种优势： 无需第三方存储系统支持 数据很方便地接入到其他系统中 所以，完整的客户端metrics采集方案如下图：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21764/154216140512649_zh-CN.png)

    E-MapReduce提供了一个开源实现emr-kafka-client-metrics，源码[地址](https://github.com/aliyun/aliyun-emapreduce-sdk/tree/master-2.x/external/emr-kafka)。


## 测试 {#section_q2p_shj_gfb .section}

我们无需自己编译，E-MapReduce已经向Maven发布了Jar包，直接下载最新版本即可，[地址](http://mvnrepository.com/artifact/com.aliyun.emr/emr-kafka-client-metrics?spm=a2c4e.11153940.blogcont624050.20.24d04bcauktP9S)。

-   配置

    |配置项|说明|
    |---|--|
    |metric.reporters|使用emr的实现：org.apache.kafka.clients.reporter.EMRClientMetricsReporter|
    |emr.metrics.reporter.bootstrap.servers|metrics存储Kafka集群的bootstrap.servers|
    |emr.metrics.reporter.zookeeper.connect|metrics存储Kafka集群的Zookeeper地址|

-   如何加载
    -   将emr-kafka-client-metrics的jar包放在机器上，客户端程序的Classpath可以加载到即可
    -   直接将emr-kafka-client-metrics依赖打进客户端程序jar包中
-   环境准备
    -   本文使用阿里云EMR服务自动化搭建Kafka集群，详细过程请参考[创建集群](../../../../intl.zh-CN/快速入门/创建 E-MapReduce/创建集群.md#)。

        本文使用的EMR Kafka版本信息如下：

        -   EMR版本: EMR-3.12.1
        -   集群类型: Kafka
        -   软件信息: Kafka-Manager \(1.3.3.16\) / Kafka \(2.11-1.0.1\) / ZooKeeper \(3.4.12\)/ Ganglia \(3.7.2\) 
        -   Kafka集群使用专有网络，区域为华东1（杭州），主实例组ECS计算资源配置公网及内网IP，具体配置如下所示：

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21764/154216140512651_zh-CN.png)

-   操作步骤

    1.  下载最新版本emr-kafka-client-metrics包。

        ```
        wget http://central.maven.org/maven2/com/aliyun/emr/emr-kafka-client-metrics/1.4.3/emr-kafka-client-metrics-1.4.3.jar
        ```

    2.  将emr-kafka-client-metrics包放到Kafka的lib中 。

        ```
        cp emr-kafka-client-metrics-1.4.3.jar /usr/lib/kafka-current/libs/
        ```

    3.  创建一个测试Topic 。

        ```
        kafka-topics.sh --zookeeper emr-header-1:2181/kafka-1.0.1 --partitions 10 --replication-factor 2 --topic test-metrics  --create
        ```

    4.  向测试topic写入数据，这里我们将producer的配置项配置到本地文件client.conf中。 

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

    5.  查看这次客户端的metrics，注意默认的metrics topic是\_emr-client-metrics 。

        ```
        kafka-console-consumer.sh --topic _emr-client-metrics --bootstrap-server emr-worker-1:9092 
        --from-beginning
        ```

        返回的消息如下所示:

        ```
        {prefix=kafka.producer, client.ip=192.168.xxx.xxx, client.process=25536@emr-header-1.cluster-xxxx, 
        attribute=request-rate, value=894.4685104965012, timestamp=1533805225045, group=producer-metrics, 
        tag.client-id=producer-1}
        ```

    |字段名|说明|
    |---|--|
    |client.ip|客户端所在机器IP|
    |client.process|客户端程序进程ID|
    |attribute|metric属性名|
    |value|metric值|
    |timestamp|metric采集的时间戳|
    |tag.xxx|metric其他tag信息|

    **说明：** 限制条件

    -   只支持Java类客户端程序 
    -   只支持0.10之后版本Kafka客户端 

