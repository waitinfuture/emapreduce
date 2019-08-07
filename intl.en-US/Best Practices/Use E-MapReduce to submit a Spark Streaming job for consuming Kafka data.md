# Use E-MapReduce to submit a Spark Streaming job for consuming Kafka data {#task_1339980 .task}

This topic describes how to create a Hadoop cluster and Kafka cluster by using E-MapReduce \(EMR\) and run a Spark Streaming job to consume Kafka data.

-   You have registered an Alibaba Cloud account. For more information, see [Create an Alibaba Cloud account](https://partners-intl.aliyun.com/help/doc-detail/50482.htm).
-   You have activated EMR.
-   You have authorized the Alibaba Cloud account. For more information, see [Role authorization](../../../../reseller.en-US/Cluster Planning and Configurations/Cluster planning/Role authorization.md#).

You always consume Kafka data in practical applications. In EMR, you can run a Spark Streaming job to consume Kafka data.

## Step 1: Create a Hadoop cluster and Kafka cluster {#section_box_g41_qc5 .section}

We recommend that you specify the same security group for the Hadoop cluster as that of the Kafka cluster when creating the two clusters. If the clusters are linked to different security groups, the two clusters are not accessible by each other. You must modify the required settings of the security groups to allow mutual access.

1.  Log on to the [Alibaba Cloud EMR console](https://partners-intl.console.aliyun.com/#/emr).
2.  Create a Hadoop cluster. For more information, see [Create a cluster](../../../../reseller.en-US/Quick Start/Step 3: Create a cluster.md#). 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1068351/156514324252748_en-US.png)

3.  Create a Kafka cluster. For more information, see [Create a cluster](../../../../reseller.en-US/Quick Start/Step 3: Create a cluster.md#). 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1068351/156514324252756_en-US.png)


## Step 2: Download a JAR file and upload it to the Hadoop cluster {#section_d13_kc1_fqp .section}

In this example, the [Demo](https://github.com/aliyun/aliyun-emapreduce-demo) project is customized and compiled to create a new JAR file. You need to upload the JAR file to the **emr-header-1** instance of the Hadoop cluster.

1.  Download the JAR file from [this link](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/126974/cn_zh/1563960119361/examples-1.2.0-shaded-2.jar.zip).
2.  Log on to the [Alibaba Cloud EMR console](https://partners-intl.console.aliyun.com/#/emr).
3.  On the Cluster Management tab, click the **Cluster ID** of the target cluster to enter the Hadoop cluster.
4.  In the left-side navigation pane, select **Instances** and view the **IP address** of the **emr-header-1** instance in the Hadoop cluster.
5.  Log on to the **emr-header-1** instance by using SSH.
6.  Upload the JAR file to a directory of the **emr-header-1** instance. 

    **Note:** The /home/hadoop directory is specified as a repository for data storage in this case. After you upload the JAR file, we recommend that you keep the logon window open for later use.


## Step 3: Create a topic on the Kafka cluster {#section_pbz_vsa_646 .section}

You can create a topic in the EMR console. For more information, see [Manage Kafka metadata](../../../../reseller.en-US/Cluster Planning and Configurations/Configure clusters/Metadata management/Manage Kafka metadata.md#). You can also log on to the **emr-header-1** instance and create a topic by using the CLI. In this example, you can create a topic named test with 10 partitions, 2 replicas.

1.  Go to the [Alibaba Cloud EMR console](https://partners-intl.console.aliyun.com/#/emr).
2.  On the Cluster Management tab, click the **Cluster ID** of the target Kafka cluster to open the Details page of the cluster.
3.  In the left-side navigation pane, select **Instances** and view the **IP address** of the **emr-header-1** instance in the Kafka cluster.
4.  Open a new shell in the SSH client and log on to the **emr-header-1** instance in the new shell.
5.  Use the following command to create a topic. 

    ``` {#codeblock_119_g13_3pu}
    /usr/lib/kafka-current/bin/kafka-topics.sh --partitions 10 --replication-factor 2 --zookeeper emr-header-1:/kafka-1.0.0 --topic test --create
    ```

    **Note:** After you create the topic, we recommend that you keep this logon window open for later use.


## Step 4: Run a Spark Streaming job {#section_qfe_z7v_suo .section}

After performing the preceding steps, you can run a Spark Streaming job on the Hadoop cluster. The following is an example of running a job to count the number of words for a data stream.

1.  Go to the logon window of the **emr-header-1** instance in the Hadoop cluster. 

    If you close the window, you need to log on again. For more information about the logon procedure, see [Step 2: Download a JAR file and upload it to the Hadoop cluster](#section_d13_kc1_fqp).

2.  Use the following command to submit a job to the Kafka cluster for counting. 

    ``` {#codeblock_7dp_3o1_3ds}
    spark-submit --class com.aliyun.emr.example.spark.streaming.KafkaSample  /home/hadoop/examples-1.2.0-shaded-2.jar 192.168.xxx.xxx:9092 test 5
    ```

    In the preceding command, the parameters after the name of the JAR file are described as follows:

    -   192.168.xxx.xxx: indicates the internal or public IP address of a **broker** in the Kafka cluster. [Figure 1](#fig_q4m_t9y_c1d) shows an example.
    -   test: indicates the name of the topic.
    -   5: indicates the time interval.
    ![](images/52814_en-US.png "List of components in the Kafka cluster")


## Step 5: Use Kafka to publish messages {#section_0pl_c96_2ht .section}

When you perform this step, ensure that the Spark Streaming job is running. After you start a Kafka producer, the number of words is displayed in a shell on a client instance of the Hadoop cluster. The value is updated in real time when you enter words into a shell on a client instance of the Kafka cluster.

1.  Go to the logon window of the **emr-header-1** instance. 

    If you close the window, you need to log on again. For more information about the logon procedure, see [Step 3: Create a topic on the Kafka cluster](#section_pbz_vsa_646).

2.  In the logon window of the client instance in the Kafka cluster, use the following command to start a producer. 

    ``` {#codeblock_60n_8m8_azb}
    /bin/kafka-console-producer.sh --topic test --broker-list emr-worker-1:9092
    ```

3.  When you enter words in the Kafka logon window, the number of words is displayed and updated in the Hadoop logon window in real time. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1068351/156514324252840_en-US.png)


## Step 6: View the progress of the Spark Streaming job {#section_isq_ew4_y5q .section}

After you run a Spark Streaming job, you can view the status of the job in the EMR console.

1.  On the **Connect Strings** page, click the link next to the **Spark History Server UI** service name to view the status of the Spark Streaming job. For more information, see [Access links and ports](https://partners-intl.aliyun.com/help/doc-detail/89065.htm) 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1068351/156514324352852_en-US.png)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1068351/156514324352855_en-US.png)


