# Use PyFlink jobs to process Kafka data

This topic describes how to run PyFlink jobs in a Hadoop cluster and a Kafka cluster created in the E-MapReduce \(EMR\) console to process Kafka data.

-   You have activated EMR.
-   You have authorized the Alibaba Cloud account. For more information, see [Authorize roles](/intl.en-US/Cluster Management/Cluster planning/Authorize roles.md).
-   A project is created. For more information, see [Manage projects](/intl.en-US/Data Development/Manage projects.md).
-   PuTTY and SSH Secure File Transfer Client are installed on your on-premises machine.

## Step 1: Create a Hadoop cluster and Kafka cluster

We recommend that you specify the same security group for the Hadoop cluster as that of the Kafka cluster when creating the two clusters. If the clusters are linked to different security groups, the two clusters are not accessible by each other. You must modify the required settings of the security groups to allow mutual access.

**Note:** EMR V3.29.0 is used as an example in this topic.

1.  Create a Hadoop cluster and select Flink from Optional Services.

    ![hadoop flink](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6817894061/p147905.png)

2.  Create a Kafka cluster.

    ![kafka](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7817894061/p148509.png)


## Step 2: Create a topic in the Kafka cluster

In this example, two topics named payment\_msg and results are created. Each topic has 10 partitions and 2 replicas.

1.  Log on to the master node of the Kafka cluster. For more information, see [Connect to the master node of an EMR cluster in SSH mode](/intl.en-US/Cluster Management/Configure clusters/Connect to a cluster/Connect to the master node of an EMR cluster in SSH mode.md).

2.  Run the following command to create a topic named payment\_msg:

    ```
    /usr/lib/kafka-current/bin/kafka-topics.sh --partitions 10 --replication-factor 2 --zookeeper emr-header-1:2181 /kafka-1.0.0 --topic payment_msg --create
    ```

3.  Run the following command to create a topic named results:

    ```
    /usr/lib/kafka-current/bin/kafka-topics.sh --partitions 10 --replication-factor 2 --zookeeper emr-header-1:2181 /kafka-1.0.0 --topic results --create
    ```


## Step 3: Prepare test data

In the command-line interface \(CLI\) of the Kafka cluster that is created in [Step 2](#section_25f_krc_a95), run the following commands to continuously generate test data:

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

## Step 4: Create and run a PyFlink job

1.  Log on to the master node of the Hadoop cluster. For more information, see [Connect to the master node of an EMR cluster in SSH mode](/intl.en-US/Cluster Management/Configure clusters/Connect to a cluster/Connect to the master node of an EMR cluster in SSH mode.md).

2.  Run the following commands to generate the lib.jar and job.py files:

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
    
    # Enter the following information based on the created Kafka cluster:
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

    Specify the following parameters in job.py based on the actual situation of the cluster.

    |Parameter|Description|
    |---------|-----------|
    |`kafka_servers`|The list of IP addresses for Kafka brokers in the Kafka cluster. All the IP addresses are the internal IP address of the Kafka cluster. The default port number is 9092. For more information about the IP addresses, see [List of components in the Kafka cluster](#fig_1).|
    |`kafka_zookeeper_servers`|The list of IP addresses for ZooKeeper components in the Kafka cluster. All the IP addresses are the internal IP address of the Kafka cluster. The default port number is 2181. For more information about the IP addresses, see [List of components in the Kafka cluster](#fig_1).|
    |`source_topic`|The Kafka topic of the source table. In this example, the topic is payment\_msg.|
    |`sink_topic`|The Kafka topic of the result table. In this example, the topic is results.|

    ![List of components in the Kafka cluster](../images/p52814.png "List of components in the Kafka cluster")

    The following figure provides an example of lib.jar and job.py.

    ![JAR locatioin](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7817894061/p148435.png)

3.  Use SSH Secure File Transfer Client to connect to the master node of the Hadoop cluster, and then download and save lib.jar and job.py to your on-premises machine that runs a Windows operating system.

4.  Upload lib.jar and job.py to the OSS console.

    1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

    2.  Create an OSS bucket and upload the two files to the bucket. For information about how to create an OSS bucket, see [Create buckets](/intl.en-US/Quick Start/Create buckets.md). For information about how to upload a file, see [Upload objects](/intl.en-US/Quick Start/Upload objects.md).

        In this example, the upload paths are oss://emr-logs2/test/lib.jar and oss://emr-logs2/test/job.py.

5.  Create a PyFlink job.

    1.  Log on to the [EMR console](https://emr.console.aliyun.com/).

    2.  In the top navigation bar, select the region where your cluster residesand select a resource group based on your business requirements.

    3.  Click the **Data Platform** tab.

    4.  In the **Projects** section of the page that appears, find the project you want to edit and click **Edit Job** in the Actions column.

    5.  In the **Edit Job** pane on the left, right-click the folder on which you want to perform operations and select **Create Job**.

    6.  In the Create Job dialog box, specify Name and Description and select **Flink** from the **Job Type** drop-down list.

    7.  Configure the job content. Example:

        ```
        run -m yarn-cluster -py ossref://emr-logs2/test/job.py -j ossref://emr-logs2/test/lib.jar
        ```

6.  Run the PyFlink job.

    1.  Click **Save** in the upper-right corner.

    2.  Click **Run** in the upper-right corner.

    3.  In the **Run Job** dialog box, select the created Hadoop cluster from the **Target Cluster** drop-down list.

    4.  Click **OK**.


## Step 5: View job details

1.  You can view the details of a PyFlink job on the web UI of YARN.

    You can use one of the following methods to access the web UI of YARN:

    -   Use an SSH tunnel: For more information, see [Create an SSH tunnel to access web UIs of open source components](/intl.en-US/Cluster Management/Configure clusters/Connect to a cluster/Create an SSH tunnel to access web UIs of open source components.md).
    -   Use Knox: For more information, see [Access open-source components](/intl.en-US/Cluster Management/Configure clusters/Access open-source components.md).
    Use Knox as an example to view the details of a PyFlink job.

2.  In the Hadoop console, click the ID of the job.

    You can view the running details of the job.

    ![flink_info](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7817894061/p148135.png)

    The following figure shows the running details.

    ![application_info](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7817894061/p148503.png)

3.  Click the link next to **Tracking URL** to go to the **Apache Flink Dashboard** page and view detailed job information.


## Step 6: View the output

1.  Log on to the master node of the Kafka cluster. For more information, see [Connect to the master node of an EMR cluster in SSH mode](/intl.en-US/Cluster Management/Configure clusters/Connect to a cluster/Connect to the master node of an EMR cluster in SSH mode.md).

2.  Run the following command to view the data in the results topic:

    ```
    kafka-console-consumer.sh --bootstrap-server emr-header-1:9092 --topic results
    ```

    The information shown in the following figure is returned.

    ![result](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7817894061/p148491.png)

    After you view the information, you can click **Stop** in the upper-right corner of the job page on the **Data Platform** tab to stop the running job.


