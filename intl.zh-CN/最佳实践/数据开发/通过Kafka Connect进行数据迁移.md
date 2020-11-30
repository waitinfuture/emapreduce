# 通过Kafka Connect进行数据迁移

在流式数据处理过程中，E-MapReduce经常需要在Kafka与其他系统间进行数据同步或者在Kafka集群间进行数据迁移。本文向您介绍如何在E-MapReduce上通过Kafka Connect快速的实现Kafka集群间的数据迁移。

-   已开通E-MapReduce服务。
-   已完成云账号的授权，详情请参见[角色授权](/intl.zh-CN/集群管理/集群规划/角色授权.md)。

Kafka Connect是一种可扩展的、可靠的、用于Kafka与其他系统间进行数据同步或者在Kafka集群间进行流式数据传输的工具。例如，Kafka Connect可以获取数据库的binlog数据，将数据库数据同步至Kafka集群，从而达到迁移数据库数据的目的。由于Kafka集群可对接流式处理系统，所以还可以间接实现数据库对接下游流式处理系统的目的。同时，Kafka Connect还提供了REST API接口，方便您创建和管理Kafka Connect。

Kafka Connect分为Standalone和Distributed两种运行模式。在Standalone模式下，所有的worker都在一个进程中运行。相比于Standalone模式，Distributed模式更具扩展性和容错性，是最常用的方式，也是生产环境推荐使用的模式。

本文介绍如何在E-MapReduce上使用Kafka Connect的REST API接口在Kafka集群间进行数据迁移，Kafka Connect使用distributed模式。

## 步骤一：创建Kafka集群

在EMR上创建源Kafka集群和目的Kafka集群。Kafka Connect安装在Task节点上，所以目的Kafka集群必须创建Task节点。集群创建好后，Task节点上Kafka Connect服务会默认启动，端口号为8083。

1.  登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com/)。

2.  创建源Kafka集群和目的Kafka集群，详情请参见[创建集群](/intl.zh-CN/集群管理/集群配置/创建集群.md)。

    **说明：**

    -   源Kafka集群和目的Kafka集群创建在同一个安全组下。

        如果源Kafka集群和目的kafka集群不在同一个安全组下，则两者的网络默认是不互通的，您需要对两者的安全组分别进行相关配置，以使两者的网络互通。

    -   创建目的Kafka集群后，需要扩容Task节点，详情请参见[新增机器组](/intl.zh-CN/集群管理/变更配置/多机器组.md)。
    ![创建Kafka集群](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1042598951/p52756.png)


## 步骤二：准备待迁移数据Topic

在源Kafka集群上创建一个名称为**connect**的Topic。

1.  以SSH方式登录到源Kafka集群的Master节点（本例为**emr-header-1**）。

2.  以root用户运行如下命令，创建一个名称为**connect**的Topic。

    ```
    kafka-topics.sh --create --zookeeper emr-header-1:2181 --replication-factor 2 --partitions 10 --topic connect
    ```

    ![创建Topic](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1042598951/p53860.png)

    **说明：** 完成上述操作后，请保留该登录窗口，后续仍将使用。


## 步骤三：创建Kafka Connect的connector

在目的Kafka集群的Task节点上，使用`curl`命令，通过JSON数据创建一个Kafka Connect的connector。

1.  自定义Kafka Connect配置。

    进入[阿里云E-MapReduce控制台](https://emr.console.aliyun.com/)目的Kafka集群**Kafka**服务的**配置**页面，在**connect-distributed.properties**中自定义**offset.storage.topic**、**config.storage.topic**和**status.storage.topic**三个配置项，详情请参见[组件参数配置](/intl.zh-CN/集群管理/集群配置/组件参数配置.md)。

    Kafka Connect会将offsets、configs和任务状态保存在Topic中，Topic名对应**offset.storage.topic**、**config.storage.topic**和**status.storage.topic**三个配置项。Kafka Connect会自动使用默认的partition和replication factor创建这三个Topic，其中partition和repication factor配置项保存在/etc/ecm/kafka-conf/connect-distributed.properties文件中。

2.  以SSH方式登录到目的Kafka集群的Master节点（本例为**emr-header-1**）。

3.  切换至Task节点（本例为**emr-worker-3**）。

    ![task](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9289930061/p167634.png)

4.  以root用户运行如下命令创建一个Kafka Connect。

    ```
    curl -X POST -H "Content-Type: application/json" --data '{"name": "connect-test", "config": { "connector.class": "EMRReplicatorSourceConnector", "key.converter": "org.apache.kafka.connect.converters.ByteArrayConverter", "value.converter": "org.apache.kafka.connect.converters.ByteArrayConverter", "src.kafka.bootstrap.servers": "${src-kafka-ip}:9092", "src.zookeeper.connect": "${src-kafka-curator-ip}:2181", "dest.zookeeper.connect": "${dest-kafka-curator-ip}:2181", "topic.whitelist": "${source-topic}", "topic.rename.format": "${dest-topic}", "src.kafka.max.poll.records": "300" } }' http://emr-worker-3:8083/connectors
    ```

    在JSON数据中，name字段代表创建的Kafka Connect的名称，本例为connect-test；config字段需要根据实际情况进行配置，关键变量的说明如下。

    |变量|说明|
    |--|--|
    |**$\{source-topic\}**|源Kafka集群中需要迁移的Topic，多个Topic需用英文逗号（,）隔开，例如**connect**。|
    |**$\{dest-topic\}**|目的Kafka集群中迁移后的Topic，例如**connect.replica**。|
    |**$\{src-kafka-curator-hostname\}**|源Kafka集群中安装了ZooKeeper服务的节点的内网IP地址。|
    |**$\{dest-kafka-curator-hostname\}**|目的Kafka集群中安装了ZooKeeper服务的节点的内网IP地址。|

    **说明：** 完成上述操作后，请保留该登录窗口，后续仍将使用。


## 步骤四：查看Kafka Connect和Task节点状态

查看Kafka Connect和Task节点信息，确保两者的状态正常。

1.  返回到目的Kafka集群的Task节点（本例为**emr-worker-3**）的登录窗口。

2.  以root用户运行如下命令查看所有的Kafka Connect。

    ```
    curl emr-worker-3:8083/connectors
    ```

    ![查看所有Kafka Connect](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1042598951/p53871.png)

3.  以root用户运行如下命令查看本例创建的Kafka Connect（本例为**connect-test**）的状态。

    ```
    curl emr-worker-3:8083/connectors/connect-test/status
    ```

    ![查看connect-test状态](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1042598951/p53874.png)

    确保Kafka Connect（本例为**connect-test**）的状态为**RUNNING**。

4.  以root用户运行如下命令查看Task节点信息。

    ```
    curl emr-worker-3:8083/connectors/connect-test/tasks
    ```

    ![查看Task节点信息](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2042598951/p53876.png)

    确保Task节点的返回信息中无错误信息。


## 步骤五：生成待迁移数据

通过命令向源集群中的**connect**Topic发送待迁移的数据。

1.  返回到源Kafka集群的header节点（本例为emr-header-1）的登录窗口。

2.  以root用户运行如下命令向**connect**的Topic发送数据。

    ```
    kafka-producer-perf-test.sh --topic connect --num-records 100000 --throughput 5000 --record-size 1000 --producer-props bootstrap.servers=emr-header-1:9092
    ```

    当提示如下信息，则表示已经成功生成待迁移数据。

    ![生成待同步数据](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9737280061/p53877.png)


## 步骤六：查看数据迁移结果

生成待迁移数据后，Kafka Connect会自动将这些数据迁移到目的集群的相应文件（本例为**connect.replica**）中。

1.  返回到目的Kafka集群的Task节点（本例为**emr-worker-3**）的登录窗口。

2.  以root用户运行如下命令查看数据迁移是否成功。

    ```
    kafka-consumer-perf-test.sh --topic connect.replica --broker-list emr-header-1:9092 --messages 100000
    ```

    ![查看数据同步结果](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2042598951/p53881.png)

    从上述返回结果可以看出，在源Kafka集群发送的100000条数据已经迁移到了目的Kafka集群。


## 小结

本文介绍并演示了使用Kafka Connect在Kafka集群间进行数据迁移的方法。如果需要了解Kafka Connect更详细的使用方法，请参见[Kafka官网资料](https://kafka.apache.org/documentation/#connect)和[REST API](https://docs.confluent.io/current/connect/references/restapi.html)。

