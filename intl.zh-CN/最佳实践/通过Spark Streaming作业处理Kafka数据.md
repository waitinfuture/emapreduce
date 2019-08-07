# 通过Spark Streaming作业处理Kafka数据 {#task_1339980 .task}

本节介绍如何使用阿里云E-MapReduce部署Hadoop集群和Kafka集群，并运行Spark Streaming作业消费Kafka数据。

-   已注册阿里云账号，详情请参见[注册云账号](https://www.alibabacloud.com/help/doc-detail/50482.htm)。
-   已开通E-MapReduce服务。
-   已完成云账号的授权，详情请参见[角色授权](../../../../intl.zh-CN/集群规划与配置/集群规划/角色授权.md#)。

在开发过程中，通常会遇到消费Kafka数据的场景。在阿里云E-MapReduce中，您可通过运行Spark Streaming作业来消费Kafka数据。

## 步骤一 创建Hadoop集群和Kafka集群 {#section_box_g41_qc5 .section}

推荐您将Hadoop集群和Kafka集群创建在同一个安全组下。如果Hadoop集群和Kafka集群不在同一个安全组下，则两者的网络默认是不互通的，您需要对两者的安全组分别进行相关配置，以使两者的网络互通。

1.  登录[阿里云 E-MapReduce 控制台](https://emr.console.aliyun.com)。
2.  创建Hadoop集群，详情请参见[创建集群](../../../../intl.zh-CN/快速入门/步骤三：创建集群.md#)。 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1068351/156514334652748_zh-CN.png)

3.  创建Kafka集群，详情请参见[创建集群](../../../../intl.zh-CN/快速入门/步骤三：创建集群.md#)。 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1068351/156514334652756_zh-CN.png)


## 步骤二 获取JAR包并上传到Hadoop集群 {#section_d13_kc1_fqp .section}

本例中的JAR包：对E-MapReduce的[Demo](https://github.com/aliyun/aliyun-emapreduce-demo)进行了一定的修改后，编译生成的JAR包。JAR包需要上传到Hadoop集群的**emr-header-1**主机中。

1.  获取JAR包（[本例JAR下载地址](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/126974/cn_zh/1563960119361/examples-1.2.0-shaded-2.jar.zip)）。
2.  返回到[阿里云 E-MapReduce 控制台](https://emr.console.aliyun.com)。
3.  在集群管理页面，单击Hadoop集群的**集群ID**，进入Hadoop集群。
4.  在左侧导航树中选择**主机列表**，然后在右侧查看Hadoop集群中**emr-header-1**主机的**IP信息**。
5.  通过SSH客户端登录**emr-header-1**主机。
6.  上传JAR包到**emr-header-1**主机的某个目录。 

    **说明：** 后续步骤中的代码有涉及到此路径，本例上传路径为/home/hadoop。上传JAR包，请保留该登录窗口，后续步骤仍将使用。


## 步骤三 在Kafka集群上创建Topic {#section_pbz_vsa_646 .section}

您可直接在E-MapReduce上以可视化的方式来创建Topic（详情请参见[Kafka 元数据管理](../../../../intl.zh-CN/集群规划与配置/集群配置/元数据管理/Kafka 元数据管理.md#)），也可登录Kafka集群的**emr-header-1**主机后以命令行的方式来创建Topic。本例以命令行方式创建一个分区数为10、副本数为2、名称为test的Topic。

1.  返回到[阿里云 E-MapReduce 控制台](https://emr.console.aliyun.com)。
2.  在集群管理页面，单击Kafka集群的**集群ID**，进入Kafka集群。
3.  在左侧导航树中选择**主机列表**，然后在右侧查看Kafka集群中**emr-header-1**主机的**IP信息**。
4.  在SSH客户端中新建一个命令窗口，登录Kafka集群的**emr-header-1**主机。
5.  通过以下命令创建Topic。 

    ``` {#codeblock_119_g13_3pu}
    /usr/lib/kafka-current/bin/kafka-topics.sh --partitions 10 --replication-factor 2 --zookeeper emr-header-1:2181 /kafka-1.0.0 --topic test --create
    ```

    **说明：** 创建Topic后，请保留该登录窗口，后续步骤仍将使用。


## 步骤四 运行Spark Streaming作业 {#section_qfe_z7v_suo .section}

完成上述操作后，您即可在Hadoop集群上运行Spark Streaming作业。本例将运行一个作业进行流式单词统计（WordCount）。

1.  返回到Hadoop集群的**emr-header-1**主机登录窗口。 

    如果误关闭了此窗口，请重新登录，详情请参见[步骤二 获取JAR包并上传到Hadoop集群](#section_d13_kc1_fqp)中的相关步骤。

2.  通过如下作业命令来进行流式单词统计（WordCount）。 

    ``` {#codeblock_7dp_3o1_3ds}
    spark-submit --class com.aliyun.emr.example.spark.streaming.KafkaSample  /home/hadoop/examples-1.2.0-shaded-2.jar 192.168.xxx.xxx:9092 test 5
    ```

    命令中JAR包后面的三个关键参数说明如下：

    -   192.168.xxx.xxx：Kafka集群中任一**Kafka Broker**组件的内网或外网IP地址，示例如[图 1](#fig_q4m_t9y_c1d)所示。
    -   test：Topic名称。
    -   5：时间间隔。
    ![](images/52814_zh-CN.png "Kafka集群组件")


## 步骤五 使用Kafka发布消息 {#section_0pl_c96_2ht .section}

进行本步骤操作时，需要保持Spark Streaming作业一直处于运行状态。运行Kafka的生产者（producer）后，在Kafka客户端的命令行中输入文本时，在Hadoop集群客户端的命令行中会实时显示单词统计结果。

1.  返回到Kafka集群的**emr-header-1**主机登录窗口。 

    如果误关闭了此窗口，请重新登录，详情请参见[步骤三 在Kafka集群上创建Topic](#section_pbz_vsa_646)中的相关步骤。

2.  在Kafka集群的登录窗口中，通过如下命令来运行生产者（producer）。 

    ``` {#codeblock_60n_8m8_azb}
    /bin/kafka-console-producer.sh --topic test --broker-list emr-worker-1:9092
    ```

3.  在Kafka登录窗口的命令行中不断输入文本，则在Hadoop集群登录窗口中实时显示文本的统计信息。 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1068351/156514334652840_zh-CN.png)


## 步骤六 查看Spark Streaming作业的进展 {#section_isq_ew4_y5q .section}

Spark Streaming作业开始运行后，您可在E-MapReduce上查看作业的状态。

1.  返回到[阿里云 E-MapReduce 控制台](https://emr.console.aliyun.com)。
2.  在Hadoop集群的**访问链接与端口**页面中，单击**Spark History Server UI**后的链接，查看Spark Streaming作业的状态。详情请参见[访问链接与端口](https://www.alibabacloud.com/help/doc-detail/89065.htm)。 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1068351/156514334752852_zh-CN.png)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1068351/156514334752855_zh-CN.png)


