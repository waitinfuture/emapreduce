# Use Spark Streaming jobs to process Kafka data

This topic describes how to run Spark Streaming jobs to process Kafka data in the E-MapReduce \(EMR\) console.

-   EMR is activated.
-   The Alibaba Cloud account is authorized. For more information, see [Authorize roles](/intl.en-US/Cluster Management/Cluster planning/Authorize roles.md).
-   PuTTY and SSH Secure File Transfer Client are installed on your on-premises machine.

## Step 1: Create a Hadoop cluster and a Kafka cluster

Create a Hadoop cluster and a Kafka cluster that belong to the same security group. For more information, see [Create a cluster](/intl.en-US/Quick Start/Create a cluster.md).

1.  Log on to the [Alibaba Cloud EMR console](https://emr.console.aliyun.com/).

2.  Create a Hadoop cluster.

    ![create hadoop cluster](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4307136061/p52748.png)

3.  Create a Kafka cluster.

    ![Software Settings step](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4307136061/p52756.png)


## Step 2: Obtain the required JAR package and upload it to the Hadoop cluster

1.  Obtain the JAR package [examples-1.2.0-shaded-2.jar.zip](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/126974/cn_zh/1563960119361/examples-1.2.0-shaded-2.jar.zip).

2.  Use SSH Secure File Transfer Client to upload the JAR package to the /home/hadoop path of the master node in the Hadoop cluster.

    ![ssh_client](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9409794061/p135108.png)


## Step 3: Create a topic on the Kafka cluster

In this example, a topic named test is created. The topic has 10 partitions and 2 replicas.

1.  Log on to the master node of the Kafka cluster. For more information, see [Connect to the master node of an EMR cluster in SSH mode](/intl.en-US/Cluster Management/Configure clusters/Connect to a cluster/Connect to the master node of an EMR cluster in SSH mode.md).

2.  Run the following command to create a topic:

    ```
    /usr/lib/kafka-current/bin/kafka-topics.sh --partitions 10 --replication-factor 2 --zookeeper emr-header-1:2181 /kafka-1.0.0 --topic test --create
    ```

    **Note:** After you create the topic, keep the logon window open for later use.


## Step 4: Run a Spark Streaming job

In this example, a WordCount job is run for streaming data.

1.  Log on to the master node of the Hadoop cluster. For more information, see [Connect to the master node of an EMR cluster in SSH mode](/intl.en-US/Cluster Management/Configure clusters/Connect to a cluster/Connect to the master node of an EMR cluster in SSH mode.md).

2.  Run the following command to submit a WordCount job for streaming data:

    ```
    spark-submit --class com.aliyun.emr.example.spark.streaming.KafkaSample  /home/hadoop/examples-1.2.0-shaded-2.jar 192.168.xxx.xxx:9092 test 5
    ```

    The following table describes the parameters.

    |Parameter|Description|
    |---------|-----------|
    |`192.168.xxx.xxx`|The internal IP address of a Kafka broker component in the Kafka cluster. For more information, see [Figure 1](#fig_q4m_t9y_c1d).|
    |`test`|The name of the topic.|
    |`5`|The time interval.|

    ![kafka Broker](../images/p52814.png "List of components in the Kafka cluster")


## Step 5: Use Kafka to publish messages

1.  In the command-line interface \(CLI\) of the Kafka cluster, run the following command to start the Kafka producer:

    ```
    /usr/lib/kafka-current/bin/kafka-console-producer.sh --topic test --broker-list emr-worker-1:9092
    ```

2.  Enter text information in the logon window of the Kafka cluster. Text statistics are displayed in the logon window of the Hadoop cluster in real time.

    Enter the information shown in the following figure in the logon window of the Kafka cluster.

    ![Kafka ssh](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9409794061/p135102.png)

    The information shown in the following figure is displayed in the logon window of the Hadoop cluster.

    ![hadoop ssh](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4307136061/p135103.png)


## Step 6: View the status of the Spark Streaming job

1.  Click the **Cluster Management** tab in the EMR console.

2.  On the Cluster Management page, find the Hadoop cluster you created and click **Details** in the Actions column.

3.  In the left-side navigation pane of the Cluster Overview page, click **Connect Strings**.

4.  Click the link of **Spark History Server UI**.

    ![Connect Strings](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/1068351/156879433652852_en-US.png)

5.  On the **History Server** page, click the **App ID** of the Spark Streaming job that you want to view.

    You can view the status of the Spark Streaming job.

    ![job status](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4307136061/p135106.png)


