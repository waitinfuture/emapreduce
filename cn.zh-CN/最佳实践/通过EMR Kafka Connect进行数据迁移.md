# 通过EMR Kafka Connect进行数据迁移 {#task_1443351 .task}

在流式数据处理过程中，经常需要在Kafka与其他系统间进行数据同步或者在Kafka集群间进行数据迁移。本节向您介绍如何通过EMR Kafka Connect方便快速的实现Kafka集群间的数据同步或者数据迁移。

-   已注册阿里云账号，详情请参见[注册云账号](http://help.aliyun.com/knowledge_detail/5974387.html)。
-   已开通E-MapReduce服务。
-   已完成云账号的授权，详情请参见[角色授权](../../../../cn.zh-CN/集群规划与配置/集群规划/角色授权.md#)。

Kafka Connect是一种可扩展的、可靠的，用于在Kafka和其他系统之间快速的进行流式数据传输的工具。例如，使用Kafka Connect获取数据库的binglog数据，然后将数据库的数据迁入Kafka集群，以同步数据库的数据或者对接下游的流式处理系统。同时，Kafka Connect还提供了REST API接口，方便您创建和管理Kafka Connect。

Kakfa Connect分为standalone和distributed两种运行模式。在standalone模式下，所有的worker都在一个进程中运行。相比于standalone模式，distributed模式更具扩展性和容错性，是最常用的方式，也是生产环境推荐使用的模式。

本文介绍如何使用EMR Kafka Connect的REST API接口在Kafka集群间进行数据迁移，Kakfa Connect使用distributed模式。

## 步骤一 创建Kafka集群 {#section_jky_y72_92b .section}

在EMR上创建源Kafka集群和目的Kafka集群。EMR Kafka Connect安装在Task节点上，所以目的Kafka集群必须创建Task节点。集群创建好后，Task节点上EMR Kafka Connect服务会默认启动，端口号为8083。

推荐您将源Kafka集群和目的Kafak集群创建在同一个安全组下。如果源Kafka集群和目的Kafak集群不在同一个安全组下，则两者的网络默认是不互通的，您需要对两者的安全组分别进行相关配置，以使两者的网络互通。

1.  登录[阿里云 E-MapReduce 控制台](https://emr.console.aliyun.com)。
2.  创建源Kafka集群和目的Kafka集群，详情请参见[创建集群](../../../../cn.zh-CN/集群规划与配置/集群配置/创建集群.md#)。 

    **说明：** 创建目的Kafka集群时，必须开启**Task实例**，即创建Task节点。

    ![创建Kafka集群](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1068351/156456600752756_zh-CN.png)


## 步骤二 准备待迁移数据Topic {#section_qgf_nnt_g9d .section}

在源Kafka集群上创建一个名称为**connect**的Topic。

1.  以SSH方式登录到源Kafka集群的header节点（本例为emr-header-1）。
2.  以root用户运行如下命令创建一个名称为**connect**的Topic。 

    ``` {#codeblock_mk0_gig_nfk}
    kafka-topics.sh --create --zookeeper emr-header-1:2181 --replication-factor 2 --partitions 10 --topic connect
    ```

    ![创建Topic](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1148224/156456600753860_zh-CN.png)

    **说明：** 完成上述操作后，请保留该登录窗口，后续仍将使用。


## 步骤三 创建Kafka Connect {#section_2ks_6mz_rym .section}

在目的Kafka集群的Task节点上，使用curl命令通过JSON数据创建一个Kafka Connect。

1.  以SSH方式登录到目的Kafka集群的Task节点（本节为**emr-worker-3**）。
2.  自定义Kafka Connect配置。 

    进入目的Kafka集群**Kafka**服务的配置页面，在**connect-distributed.properties**中自定义**offset.storage.topic**、**config.storage.topic**和**status.storage.topic**三个配置项，详情请参见[组件参数配置](../../../../cn.zh-CN/集群规划与配置/第三方软件/组件参数配置.md#)。

    Kafka Connect会将offsets、configs和任务状态保存在Topic中，Topic名对应**offset.storage.topic**、**config.storage.topic**和**status.storage.topic**三个配置项。Kafka Connect会自动使用默认的partition和replication factor创建这三个Topic，保存路径为/etc/ecm/kafka-conf/connect-distributed.properties。

3.  以root用户运行如下命令创建一个Kafka Connect。 

    ``` {#codeblock_pzk_uyf_q97}
    curl -X POST -H "Content-Type: application/json" --data '{"name": "connect-test", "config": { "connector.class": "EMRReplicatorSourceConnector", "key.converter": "org.apache.kafka.connect.converters.ByteArrayConverter", "value.converter": "org.apache.kafka.connect.converters.ByteArrayConverter", "src.kafka.bootstrap.servers": "${src-kafka-ip}:9092", "src.zookeeper.connect": "${src-kafka-curator-ip}:2181", "dest.zookeeper.connect": "${dest-kakfa-curator-ip}:2181", "topic.whitelist": "${source-topic}", "topic.rename.format": "${dest-topic}", "src.kafka.max.poll.records": "300" } }' http://emr-worker-3:8083/connectors
    ```

    在JSON数据中，name字段代表创建的Kafka Connect的名称，本例为connect-test；config字段需要根据实际情况进行配置，关键变量的说明如下：

    |变量|说明|
    |--|--|
    |**$\{source-topic\}**|源Kafka集群中需要同步的Topic，多个Topic需用英文逗号（,）隔开，例如**connect**。|
    |**$\{dest-topic\}**|目的Kafka集群中同步后的Topic，例如**connect.replica**。|
    |**$\{src-kafka-curator-hostname\}**|源Kafka集群中安装了ZooKeeper服务的节点的内网IP地址。|
    |**$\{dest-kakfa-curator-hostname\}**|目的Kafka集群中安装了ZooKeeper服务的节点的内网IP地址。|

    **说明：** 完成上述操作后，请保留该登录窗口，后续仍将使用。


## 步骤四 查看Kafka Connect和Task节点状态 {#section_lo0_pff_1qq .section}

查看Kafka Connect和Task节点信息，确保两者的状态正常。

1.  返回到目的Kafka集群的Task节点（本节为**emr-worker-3**）的登录窗口。
2.  在以root用户运行如下命令查看所有的Kafka Connect。 

    ``` {#codeblock_vjq_att_cp8}
    curl emr-worker-3:8083/connectors
    ```

    ![查看所有Kafka Connect](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1148224/156456600753871_zh-CN.png)

3.  在以root用户运行如下命令查看本例创建的Kafka Connect（本例为**connect-test**）的状态。 

    ``` {#codeblock_gb3_i3t_cmf}
    curl emr-worker-3:8083/connectors/connect-test/status
    ```

    ![查看connect-test状态](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1148224/156456600753874_zh-CN.png)

    确保Kafka Connect（本例为**connect-test**）的状态为**RUNNING**。

4.  在以root用户运行如下命令查看Task节点信息。 

    ``` {#codeblock_zst_gq6_q73}
    curl emr-worker-3:8083/connectors/connect-test/tasks
    ```

    ![查看Task节点信息](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1148224/156456600853876_zh-CN.png)

    确保Task节点的返回信息中无错误信息。


## 步骤五 生成待同步数据 {#section_hz9_ok8_10n .section}

通过命令向源集群中的**connect** Topic发送待同步的数据。

1.  返回到源Kafka集群的header节点（本例为emr-header-1）的登录窗口。
2.  在以root用户运行如下命令向**connect** Topic发送数据。 

    ``` {#codeblock_uph_1g2_7zf}
    kafka-producer-perf-test.sh --topic connect --num-records 100000 --throughput 5000 --record-size 1000 --producer-props bootstrap.servers=emr-header-1:9092
    ```

    ![生成待同步数据](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1148224/156456600853877_zh-CN.png)


## 步骤六 查看数据同步结果 {#section_4ot_7td_mgr .section}

生成待同步数据后，Kafka Connect会自动将这些数据同步到目的集群的相应文件（本例为**connect.replica**）中。

1.  返回到目的Kafka集群的Task节点（本节为**emr-worker-3**）的登录窗口。
2.  在以root用户运行如下命令查看数据同步是否成功。 

    ``` {#codeblock_q9p_czo_yxi}
    kafka-consumer-perf-test.sh --topic connect.replica --broker-list emr-header-1:9092 --messages 100000
    ```

    ![查看数据同步结果](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1148224/156456600853881_zh-CN.png)

    从上述返回结果可以看出，在源Kafka集群发送的100000条数据已经迁移到了目的Kakfa集群。


## 小结 {#section_taj_fni_lf8 .section}

本文介绍并演示了使用EMR Kafka Connect在Kafka集群间进行数据迁移的方法。如果需要了解Kafka Connect更详细的使用方法，请参见[Kafka官网资料](https://kafka.apache.org/documentation/#connect)和[REST API](https://docs.confluent.io/current/connect/references/restapi.html)。

