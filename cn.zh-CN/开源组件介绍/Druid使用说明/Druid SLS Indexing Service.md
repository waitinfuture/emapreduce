# Druid SLS Indexing Service {#concept_zt2_q5v_hhb .concept}

SLS Indexing Service 是 E-MapReduce 推出的一个 Druid 插件，用于从 SLS 消费数据。

## 背景介绍 {#section_vrr_wvv_hhb .section}

SLS Indexing Service 消费原理与 Kafka Indexing Service 类似，因此也支持 Kafka Indexing Service 一样的 Exactly-Once 语义。其综合了 SLS 与 Kafka Indexing Service 两个服务的优点：

-   极为便捷的数据采集，可以利用 SLS 的多种数据采集方式实时将数据导入 SLS。
-   不用额外维护一个 Kafka 集群，省去了数据流的一个环节。
-   支持 Exactly-Once 语义。
-   消费作业高可靠保证，作业失败重试，集群重启/升级业务无感知等。

## 准备工作 {#section_zk1_dwv_hhb .section}

-   如果您还没有开通 SLS 服务，请先开通 SLS 服务，并配置好相应的 Project 和 Logstore。
-   准备好以下配置项内容：
    -   SLS 服务的 endpoint（注意要用内网服务入口）
    -   可访问 SLS 服务的 AccessKeyId 和对应的 AccessKeySecret 

## 使用 SLS Indexing Service {#section_h5r_nxv_hhb .section}

1.  准备数据格式描述文件

    如果您熟悉 Kafka Indexing Service，那么 SLS Indexing Service 会非常简单。参考 [Kafka Indexing Service](cn.zh-CN/开源组件介绍/Druid使用说明/Kafka Indexing Service.md#) 小节的介绍，我们用同样的数据进行索引，那么数据源的数据格式描述文件如下（将其保存为 metrics-sls.json）：

    ```
    {
        "type": "sls",
        "dataSchema": {
            "dataSource": "metrics-sls",
            "parser": {
                "type": "string",
                "parseSpec": {
                    "timestampSpec": {
                        "column": "time",
                        "format": "auto"
                    },
                    "dimensionsSpec": {
                        "dimensions": ["url", "user"]
                    },
                    "format": "json"
                }
            },
            "granularitySpec": {
                "type": "uniform",
                "segmentGranularity": "hour",
                "queryGranularity": "none"
            },
            "metricsSpec": [{
                    "type": "count",
                    "name": "views"
                },
                {
                    "name": "latencyMs",
                    "type": "doubleSum",
                    "fieldName": "latencyMs"
                }
            ]
        },
        "ioConfig": {
            "project": <your_project>,
            "logstore": <your_logstore>,
            "consumerProperties": {
                "endpoint": "cn-hangzhou-intranet.log.aliyuncs.com", (以杭州为例，注意使用内网服务入口)
                "access-key-id": <your_access_key_id>,
                "access-key-secret": <your_access_key_secret>,
                "logtail.collection-mode": "simple"/"other"
            },
            "taskCount": 1,
            "replicas": 1,
            "taskDuration": "PT1H"
        },
        "tuningConfig": {
            "type": "sls",
            "maxRowsInMemory": "100000"
        }
    }
    ```

    对比 Kafka Indexing Service 一节中的介绍，我们发现两者基本上是一样的。这里简要列一下需要注意的字段：

    -   type: sls
    -   dataSchema.parser.parseSpec.format：与 ioConfig.consumerProperties.logtail.collection-mode 有关，也就是与 SLS 日志的收集模式有关。如果是极简模式（simple）收集，那么该出原本文件是什么格式，就填什么格式。如果是非极简模式（other）收集，那么此处取值为 json。
    -   ioConfig.project：您要收集的日志的 project
    -   ioConfig.logstore： 你要收集的日志的 logstore
    -   ioConfig.consumerProperties.endpoint： SLS 内网服务地址，例如杭州对应 cn-hangzhou-intranet.log.aliyuncs.com
    -   ioConfig.consumerProperties.access-key-id：账户的 AccessKeyID
    -   ioConfig.consumerProperties.access-key-secret： 账户的 AccessKeySecret
    -   ioConfig.consumerProperties.logtail.collection-mode： SLS 日志收集模式，极简模式填 simple，其他情况填 other
2.  执行下述命令添加 SLS supervisor：

    ```
    curl --negotiate -u:druid -b ~/cookies -c ~/cookies -XPOST -H 'Content-Type: application/json' -d @metrics-sls.json http://emr-header-1.cluster-1234:18090/druid/indexer/v1/supervisor
    ```

    **说明：** 其中 --negotiate、-u、-b、-c 等选项是针对安全 Druid 集群。

3.  向 SLS 中导入数据

    您可以采用多种方式向 SLS 中导入数据。具体请参考 [SLS](https://help.aliyun.com/product/28958.html)文档。

4.  在 Druid 端进行相关查询

