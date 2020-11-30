# 通过PyFlink作业处理Kafka数据

本文介绍如何使用阿里云E-MapReduce创建的Hadoop和Kafka集群，运行PyFlink作业以消费Kafka数据。

-   已开通E-MapReduce服务。
-   已完成云账号的授权，详情请参见[角色授权](/intl.zh-CN/集群管理/集群规划/角色授权.md)。
-   已创建项目，详情请参见[项目管理](/intl.zh-CN/数据开发/项目管理.md)。
-   本地安装了PuTTY和文件传输工具（SSH Secure File Transfer Client）。

## 步骤一：创建Hadoop集群和Kafka集群

创建同一个安全组下的Hadoop和Kafka集群。创建详情请参见[创建集群](/intl.zh-CN/快速入门/创建集群.md)。

**说明：** 本文以EMR-3.29.0为例介绍。

1.  登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com/)。

2.  创建Hadoop集群，并选择Flink服务。

    ![hadoop flink](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8042598951/p147905.png)

3.  创建Kafka集群。

    ![kafka](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8042598951/p148509.png)


## 步骤二：在Kafka集群上创建Topic

本示例将创建一个分区数为10、副本数为2、名称为payment\_msg和results的Topic。

1.  登录Kafka集群的Master节点，详情请参见[使用SSH连接主节点](/intl.zh-CN/集群管理/集群配置/连接集群/使用SSH连接主节点.md)。

2.  执行如下命令，创建名称为payment\_msg的Topic。

    ```
    /usr/lib/kafka-current/bin/kafka-topics.sh --partitions 10 --replication-factor 2 --zookeeper emr-header-1:2181 /kafka-1.0.0 --topic payment_msg --create
    ```

3.  执行如下命令，创建名称为results的Topic。

    ```
    /usr/lib/kafka-current/bin/kafka-topics.sh --partitions 10 --replication-factor 2 --zookeeper emr-header-1:2181 /kafka-1.0.0 --topic results --create
    ```

    **说明：** 创建Topic后，请保留该登录窗口，后续步骤仍将使用。


## 步骤三：准备测试数据

在[步骤二](#section_25f_krc_a95)中Kafka集群的命令行窗口，执行如下命令，不断生成测试数据。

```
python3 -m pip install kafka
rm -rf produce_data.py
cat>produce_data.py<<EOF
import random
import time, calendar
from random import randint
from kafka import KafkaProducer
from json import dumps
from time import sleep


def write_data():
    data_cnt = 20000
    order_id = calendar.timegm(time.gmtime())
    max_price = 100000

    topic = "payment_msg"
    producer = KafkaProducer(bootstrap_servers=['emr-worker-1:9092'],
                             value_serializer=lambda x: dumps(x).encode('utf-8'))

    for i in range(data_cnt):
        ts = time.strftime("%Y-%m-%d %H:%M:%S", time.localtime())
        rd = random.random()
        order_id += 1
        pay_amount = max_price * rd
        pay_platform = 0 if random.random() < 0.9 else 1
        province_id = randint(0, 6)
        cur_data = {"createTime": ts, "orderId": order_id, "payAmount": pay_amount, "payPlatform": pay_platform, "provinceId": province_id}
        producer.send(topic, value=cur_data)
        sleep(0.5)


if __name__ == '__main__':
    write_data()

EOF
python3 produce_data.py
```

## 步骤四：创建并运行PyFlink作业

1.  登录Hadoop集群的Master节点，详情请参见[使用SSH连接主节点](/intl.zh-CN/集群管理/集群配置/连接集群/使用SSH连接主节点.md)。

2.  执行如下命令，生成lib.jar和job.py。

    ```
    rm -rf job.py
    cat>job.py<<EOF
    import os
    from pyflink.datastream import StreamExecutionEnvironment, TimeCharacteristic
    from pyflink.table import StreamTableEnvironment, DataTypes, EnvironmentSettings
    from pyflink.table.udf import udf
    
    
    provinces = ("beijing", "shanghai", "hangzhou", "shenzhen", "jiangxi", "chongqing", "xizang")
    
    
    @udf(input_types=[DataTypes.INT()], result_type=DataTypes.STRING())
    def province_id_to_name(id):
        return provinces[id]
    
    #请根据创建的Kafka集群，输入以下信息。
    def log_processing():
        kafka_servers = "xx.xx.xx.xx:9092,xx.xx.xx.xx:9092,xx.xx.xx.xx:9092"
        kafka_zookeeper_servers = "xx.xx.xx.xx:2181,xx.xx.xx.xx:2181,xx.xx.xx.xx:2181"
        source_topic = "payment_msg"
        sink_topic = "results"
        kafka_consumer_group_id = "test_3"
    
        env = StreamExecutionEnvironment.get_execution_environment()
        env.set_stream_time_characteristic(TimeCharacteristic.EventTime)
        env_settings = EnvironmentSettings.Builder().use_blink_planner().build()
        t_env = StreamTableEnvironment.create(stream_execution_environment=env, environment_settings=env_settings)
        t_env.get_config().get_configuration().set_boolean("python.fn-execution.memory.managed", True)
    
        source_ddl = f"""
                CREATE TABLE payment_msg(
                    createTime VARCHAR,
                    rt as TO_TIMESTAMP(createTime),
                    orderId BIGINT,
                    payAmount DOUBLE,
                    payPlatform INT,
                    provinceId INT,
                    WATERMARK FOR rt as rt - INTERVAL '2' SECOND
                ) WITH (
                  'connector.type' = 'kafka',
                  'connector.version' = 'universal',
                  'connector.topic' = '{source_topic}',
                  'connector.properties.bootstrap.servers' = '{kafka_servers}',
                  'connector.properties.zookeeper.connect' = '{kafka_zookeeper_servers}',
                  'connector.properties.group.id' = '{kafka_consumer_group_id}',
                  'connector.startup-mode' = 'latest-offset',
                  'format.type' = 'json'
                )
                """
    
        es_sink_ddl = f"""
                CREATE TABLE es_sink (
                province VARCHAR,
                pay_amount DOUBLE,
                rowtime TIMESTAMP(3)
                ) with (
                  'connector.type' = 'kafka',
                  'connector.version' = 'universal',
                  'connector.topic' = '{sink_topic}',
                  'connector.properties.bootstrap.servers' = '{kafka_servers}',
                  'connector.properties.zookeeper.connect' = '{kafka_zookeeper_servers}',
                  'connector.properties.group.id' = '{kafka_consumer_group_id}',
                  'connector.startup-mode' = 'latest-offset',
                  'format.type' = 'json'
                )
        """
    
        t_env.sql_update(source_ddl)
        t_env.sql_update(es_sink_ddl)
    
        t_env.register_function('province_id_to_name', province_id_to_name)
    
        query = """
        select province_id_to_name(provinceId) as province, sum(payAmount) as pay_amount, tumble_start(rt, interval '5' second) as rowtime
        from payment_msg
        group by tumble(rt, interval '5' second), provinceId
        """
    
        t_env.sql_query(query).insert_into("es_sink")
    
        t_env.execute("payment_demo")
    
    
    if __name__ == '__main__':
        log_processing()
    EOF
    
    rm -rf lib
    mkdir lib
    cd lib
    wget https://maven.aliyun.com/nexus/content/groups/public/org/apache/flink/flink-sql-connector-kafka_2.11/1.10.1/flink-sql-connector-kafka_2.11-1.10.1.jar
    wget https://maven.aliyun.com/nexus/content/groups/public/org/apache/flink/flink-json/1.10.1/flink-json-1.10.1-sql-jar.jar
    cd ../
    zip -r lib.jar lib/*
    ```

    请您根据集群的实际情况，修改job.py中如下参数。

    |参数|描述|
    |--|--|
    |`kafka_servers`|Kafka集群中Kafka Broker组件的地址列表。IP地址为Kafka集群的内网IP地址，端口号默认为9092。IP地址如[Kafka集群组件](#fig_1)所示。|
    |`kafka_zookeeper_servers`|Kafka集群中Zookeeper服务的地址列表。IP地址为Kafka集群的内网IP地址，端口号默认为2181。IP地址如[Kafka集群组件](#fig_1)所示。|
    |`source_topic`|源表的Kafka Topic，本文示例为payment\_msg。|
    |`sink_topic`|结果表的Kafka Topic，本文示例为results。|

    ![Kafka集群组件](../images/p52814.png "Kafka集群组件")

    本地以Windows为例，生成lib.jar和job.py示例如下。

    ![JAR locatioin](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8042598951/p148435.png)

3.  使用文件传输工具链接Hadoop集群的Master节点，下载生成的lib.jar和job.py至本地。

4.  上传生成的lib.jar和job.py至OSS管理控制台。

    1.  登录[OSS管理控制台](https://oss.console.aliyun.com/)。

    2.  创建存储空间和上传文件，详情请参见[创建存储空间](/intl.zh-CN/快速入门/创建存储空间.md)和[上传文件](/intl.zh-CN/快速入门/上传文件.md)。

        本示例的上传路径分别为oss://emr-logs2/test/lib.jar和oss://emr-logs2/test/job.py。

5.  创建PyFlink作业。

    1.  登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com/)。

    2.  在顶部菜单栏处，根据实际情况选择地域（Region）和资源组。

    3.  单击上方的**数据开发**页签。

    4.  在**项目列表**页面，单击待编辑项目所在行的**作业编辑**。

    5.  在**作业编辑**区域，在需要操作的文件夹上，右键选择**新建作业**。

    6.  输入作业名称、作业描述，在**作业类型**下拉列表中选择**Flink**。

    7.  配置作业内容，示例如下。

        ```
        run -m yarn-cluster -py ossref://emr-logs2/test/job.py -j ossref://emr-logs2/test/lib.jar
        ```

6.  运行PyFlink作业。

    1.  单击右上角的**保存**。

    2.  单击右上角的**运行**。

    3.  在**运行作业**对话框中，从**执行集群**列表，选择新建的Hadoop集群。

    4.  单击**确定**。


## 步骤五：查看作业信息

1.  通过Yarn UI查看Flink作业的信息。

    访问Yarn UI支持如下两种方式：

    -   SSH隧道方式：详情请参见[通过SSH隧道方式访问开源组件Web UI](/intl.zh-CN/集群管理/集群配置/连接集群/通过SSH隧道方式访问开源组件Web UI.md)。
    -   Knox方式：详情请参见[访问链接与端口](/intl.zh-CN/集群管理/集群配置/访问链接与端口.md)。
    本示例通过Knox方式查看Flink作业的信息。

2.  在Hadoop控制台，单击作业的**ID**。

    您可以查看作业运行详情。

    ![flink_info](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8042598951/p148135.png)

    详细信息如下。

    ![application_info](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8042598951/p148503.png)

3.  单击**Tracking URL**后面的链接，进入**Apache Flink Dashboard**页面。

    您可以查看详情的作业信息。


## 步骤六：查看输出数据

1.  登录Kafka集群的Master节点。详情请参见[使用SSH连接主节点](/intl.zh-CN/集群管理/集群配置/连接集群/使用SSH连接主节点.md)。

2.  执行如下命令，查看results的数据。

    ```
    kafka-console-consumer.sh --bootstrap-server emr-header-1:9092 --topic results
    ```

    返回信息如下。

    ![result](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8042598951/p148491.png)

    查看完信息后，您可以在**数据开发**的Flink作业页面，单击右上角的**停止**，停掉正在运行的Flink作业。


