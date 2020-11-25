# 通过Spark Streaming作业处理Kafka数据

本文介绍如何使用阿里云E-MapReduce创建的Hadoop和Kafka集群，运行Spark Streaming作业以消费Kafka数据。

-   已注册阿里云账号，详情请参见[t12832.md\#]()。
-   已开通E-MapReduce服务。
-   已完成云账号的授权，详情请参见[角色授权](/intl.zh-CN/集群管理/集群规划/角色授权.md)。
-   本地安装了PuTTY和文件传输工具（SSH Secure File Transfer Client）。

## 步骤一：创建Hadoop集群和Kafka集群

创建同一个安全组下的Hadoop和Kafka集群。创建详情请参见[创建集群](/intl.zh-CN/快速入门/创建集群.md)。

1.  登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com/)。

2.  创建Hadoop集群。

    ![create hadoop cluster](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3042598951/p52748.png)

3.  创建Kafka集群。

    ![软件配置](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1042598951/p52756.png)


## 步骤二：获取JAR包并上传到Hadoop集群

1.  获取JAR包（[examples-1.2.0-shaded-2.jar.zip](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/126974/cn_zh/1563960119361/examples-1.2.0-shaded-2.jar.zip)）。

2.  使用文件传输工具，上传JAR包至Hadoop集群Master节点的/home/hadoop路径下。

    ![ssh_client](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9932598951/p135108.png)


## 步骤三：在Kafka集群上创建Topic

本示例将创建一个分区数为10、副本数为2、名称为test的Topic。

1.  登录Kafka集群的Master节点，详情请参见[使用SSH连接主节点](/intl.zh-CN/集群管理/集群配置/连接集群/使用SSH连接主节点.md)。

2.  通过如下命令创建Topic。

    ```
    /usr/lib/kafka-current/bin/kafka-topics.sh --partitions 10 --replication-factor 2 --zookeeper emr-header-1:2181 /kafka-1.0.0 --topic test --create
    ```

    **说明：** 创建Topic后，请保留该登录窗口，后续步骤仍将使用。


## 步骤四：运行Spark Streaming作业

本示例将运行一个流式单词统计（WordCount）的作业。

1.  登录Hadoop集群的Master节点，详情请参见[使用SSH连接主节点](/intl.zh-CN/集群管理/集群配置/连接集群/使用SSH连接主节点.md)。

2.  执行如下作业命令，进行流式单词统计（WordCount）。

    ```
    spark-submit --class com.aliyun.emr.example.spark.streaming.KafkaSample  /home/hadoop/examples-1.2.0-shaded-2.jar 192.168.xxx.xxx:9092 test 5
    ```

    关键参数说明如下：

    |参数|描述|
    |--|--|
    |`192.168.xxx.xxx`|Kafka集群中任一**Kafka Broker**组件的内网IP地址。IP地址如[图 1](#fig_q4m_t9y_c1d)所示。|
    |`test`|Topic名称。|
    |`5`|时间间隔。|

    ![kafka Broker](../images/p52814.png "Kafka集群组件")


## 步骤五：使用Kafka发布消息

1.  在Kafka集群的命令行窗口，执行如下命令运行Kafka的生产者。

    ```
    /usr/lib/kafka-current/bin/kafka-console-producer.sh --topic test --broker-list emr-worker-1:9092
    ```

2.  如果在Kafka集群的登录窗口中输入文本，则在Hadoop集群的登录窗口中，会实时显示文本的统计信息。

    Kafka集群登录窗口输入如下信息。

    ![Kafka ssh](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9932598951/p135102.png)

    Hadoop集群登录窗口会输出如下信息。

    ![hadoop ssh](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9932598951/p135103.png)


## 步骤六：查看Spark Streaming作业状态

1.  在E-MapReduce控制台的**集群管理**页面。

2.  单击创建的Hadoop集群所在行的**详情**。

3.  在左侧导航栏，单击**访问链接与端口**。

4.  单击**Spark History Server UI**所在行的链接。

    ![访问链接与端口](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0042598951/p52852.png)

5.  在**History Server**页面，单击待查看的**App ID**。

    您可以查看Spark Streaming作业的状态。

    ![job status](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0042598951/p135106.png)


