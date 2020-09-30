# Configure Flume

Apache Flume is supported in EMR V3.16.0 and later. This topic describes how to use Flume to copy data from an EMR Kafka cluster to HDFS, Hive, HBase, and an Object Storage Service \(OSS\) bucket for an EMR Hadoop cluster.

## Prerequisites

-   You have logged on to the [EMR console](https://emr.console.aliyun.com/).
-   An EMR Hadoop cluster is created, and Flume is selected from the **optional services** during the cluster creation.

    **Note:** The Flume software package is placed in the /usr/lib/flume-current directory. For information about the paths of common EMR files, see [Common EMR file paths](/intl.en-US/Quick Start/Common EMR file paths.md).

-   An EMR Kafka cluster is created, and a topic named flume-test is created for the cluster to generate data.

**Note:**

-   If you create a high-security Hadoop cluster that consumes data of a standard Kafka cluster, you must configure Kerberos authentication in the Hadoop cluster. For more information, see [Compatible with the MIT Kerberos authentication protocol](/intl.en-US/Cluster types/Hadoop cluster/Kerberos/Compatible with the MIT Kerberos authentication protocol.md).
-   If you want to copy data from a high-security Kafka cluster to a standard Hadoop cluster, you must perform the configurations in [Kerberos Kafka Source](#section_urk_t9b_4ud).
-   If your Hadoop and Kafka clusters are both high-security clusters, you must configure cross-region access. For more information, see [Cross-region access](/intl.en-US/Cluster types/Hadoop cluster/Kerberos/Cross-region access.md). For other related configurations, see [Use Flume with cross-region access configured](#section_oft_fjs_zfb).

## Kafka-\>HDFS

1.  Configure Flume.

    Create a configuration file named flume.properties and add the following configurations to the file:

    -   a1.sources.source1.kafka.bootstrap.servers: the hostnames and port numbers of Kafka brokers
    -   a1.sources.source1.kafka.topics: the Kafka topics that Flume consumes
    -   a1.sinks.k1.hdfs.path: the HDFS path to which Flume copies data
    ```
    a1.sources = source1
    a1.sinks = k1
    a1.channels = c1
    
    a1.sources.source1.type = org.apache.flume.source.kafka.KafkaSource
    a1.sources.source1.channels = c1
    a1.sources.source1.kafka.bootstrap.servers = kafka-host1:port1,kafka-host2:port2...
    a1.sources.source1.kafka.topics = flume-test
    a1.sources.source1.kafka.consumer.group.id = flume-test-group
    
    # Describe the sink
    a1.sinks.k1.type = hdfs
    a1.sinks.k1.hdfs.path = /tmp/flume/test-data
    a1.sinks.k1.hdfs.fileType=DataStream
    
    # Use a channel which buffers events in memory
    a1.channels.c1.type = memory
    a1.channels.c1.capacity = 100
    a1.channels.c1.transactionCapacity = 100
    
    # Bind the source and sink to the channel
    a1.sources.source1.channels = c1
    a1.sinks.k1.channel = c1
    ```

    Examples for setting a1.sinks.k1.hdfs.path to a URL:

    -   HA cluster

        ```
        a1.sinks.k1.hdfs.path = hdfs://emr-cluster/tmp/flume/test-data
        ```

    -   Standard cluster

        ```
        a1.sinks.k1.hdfs.path = hdfs://emr-header-1:9000/tmp/flume/test-data
        ```

2.  Start Flume.

    By default, the configuration file of Flume is stored in the /etc/ecm/flume-conf directory. Use the configuration file to start a Flume agent.

    ```
    flume-ng agent --name a1 --conf /etc/ecm/flume-conf  --conf-file flume.properties
    ```

    The log4j.properties file in the /etc/ecm/flume-conf directory is used. As a result, the logs/flume.log file is generated in the same directory when the Flume agent is started. You can configure the log4j.properties file as needed.

3.  Test data writes.

    Use the kafka-console-producer.sh tool for your Kafka cluster to write test data abc.

    ![test_abc](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1459149751/p33579.png)

    Flume generates a file named FlumeData.xxxx in HDFS. xxxx indicates the current timestamp in milliseconds. The file content is the test data.

    ![check_kafka_data](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1459149751/p33580.png)


## Kafka-\>Hive

1.  Create a Hive table.

    Flume uses transactions to write data to Hive. You must specify the transactional property when you create a Hive table. For example, create a table named flume\_test.

    ```
    create table flume_test (id int, content string)
    clustered by (id) into 2 buckets
    stored as orc  TBLPROPERTIES('transactional'='true');
    ```

2.  Configure Flume.

    Create a configuration file named flume.properties and add the following configurations to the file:

    -   a1.sources.source1.kafka.bootstrap.servers: the hostnames and port numbers of Kafka brokers
    -   a1.sinks.k1.hive.metastore: the URI of Hive metastore. Set this parameter to the value of the hive.metastore.uris configuration item in the hive-site.xml file.
    ```
    a1.sources = source1
    a1.sinks = k1
    a1.channels = c1
    
    a1.sources.source1.type = org.apache.flume.source.kafka.KafkaSource
    a1.sources.source1.channels = c1
    a1.sources.source1.kafka.bootstrap.servers = kafka-host1:port1,kafka-host2:port2...
    a1.sources.source1.kafka.topics = flume-test
    a1.sources.source1.kafka.consumer.group.id = flume-test-group
    
    # Describe the sink
    a1.sinks.k1.type = hive
    a1.sinks.k1.hive.metastore = thrift://xxxx:9083
    a1.sinks.k1.hive.database = default
    a1.sinks.k1.hive.table = flume_test
    a1.sinks.k1.serializer = DELIMITED
    a1.sinks.k1.serializer.delimiter = ","
    a1.sinks.k1.serializer.serdeSeparator = ','
    a1.sinks.k1.serializer.fieldnames =id,content
    
    a1.channels.c1.type = memory
    a1.channels.c1.capacity = 100
    a1.channels.c1.transactionCapacity = 100
    
    a1.sources.source1.channels = c1
    a1.sinks.k1.channel = c1
    ```

3.  Start Flume.

    ```
    flume-ng agent --name a1 --conf /etc/ecm/flume-conf  --conf-file flume.properties
    ```

4.  Generate data.

    Use the kafka-console-producer.sh tool for your Kafka cluster to write test data. Ensure that the data entries are separated with commas \(,\), such as `1,a`.

5.  Test data writes.

    Configure the following information on the client:

    ```
    hive.support.concurrency – true
    hive.exec.dynamic.partition.mode – nonstrict
    hive.txn.manager – org.apache.hadoop.hive.ql.lockmgr.DbTxnManager
    ```

    Query the data in the flume\_test table.

    ![check_flume_test](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1459149751/p33581.png)


## Kafka-\>HBase

1.  Create an HBase table.

    Create an HBase table named flume\_test and a column family named column.

    ![create_flume_test](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1459149751/p33582.png)

2.  Configure Flume.

    Create a configuration file named flume.properties and add the following configurations to the file:

    -   a1.sources.source1.kafka.bootstrap.servers: the hostnames and port numbers of Kafka brokers
    -   a1.sinks.k1.table: the name of the HBase table
    -   a1.sinks.k1.columnFamily: the column family name
    ```
    a1.sources = source1
    a1.sinks = k1
    a1.channels = c1
    
    a1.sources.source1.type = org.apache.flume.source.kafka.KafkaSource
    a1.sources.source1.channels = c1
    a1.sources.source1.kafka.bootstrap.servers = kafka-host1:port1,kafka-host2:port2...
    a1.sources.source1.kafka.topics = flume-test
    a1.sources.source1.kafka.consumer.group.id = flume-test-group
    
    a1.sinks.k1.type = hbase
    a1.sinks.k1.table = flume_test
    a1.sinks.k1.columnFamily = column
    
    
    # Use a channel which buffers events in memory
    a1.channels.c1.type = memory
    a1.channels.c1.capacity = 1000
    a1.channels.c1.transactionCapacity = 100
    
    # Bind the source and sink to the channel
    a1.sources.source1.channels = c1
    a1.sinks.k1.channel = c1
    ```

3.  Start Flume.

    ```
    flume-ng agent --name a1 --conf /etc/ecm/flume-conf  --conf-file flume.properties
    ```

4.  Test data writes.

    Use the kafka-console-producer.sh tool for your Kafka cluster to write data. Then, you can query the data from the HBase table.

    ![test](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1459149751/p33583.png)


## Kafka-\>OSS

1.  Create an OSS path.

    Create an OSS path, which consists of an OSS bucket name and its directory, such as oss://flume-test/result.

2.  Configure Flume.

    When you use Flume to write data to OSS, a large JVM memory space is occupied. You can reduce the OSS cache size or increase the value of the -Xmx option for Flume agents.

    -   Modify the OSS cache size.

        Copy the hdfs-site.xml configuration file from /etc/ecm/hadoop-conf to /etc/ecm/flume-conf, and set the smartdata.cache.buffer.size parameter to a smaller value, such as 1048576.

    -   Change the value of the -Xmx option.

        In the /etc/ecm/flume-conf directory of Flume, copy the flume-env.sh.template file and rename it flume-env.sh in the same directory. Change the value of the -Xmx option in the flume-env.sh file to 1g.

        ```
        export JAVA_OPTS="-Xmx1g"
        ```

    Create a configuration file named flume.properties and add the following configurations to the file.

    -   a1.sources.source1.kafka.bootstrap.servers: the hostnames and port numbers of Kafka brokers
    -   a1.sinks.k1.hdfs.path: the OSS path
    ```
    a1.sources = source1
    a1.sinks = k1
    a1.channels = c1
    
    a1.sources.source1.type = org.apache.flume.source.kafka.KafkaSource
    a1.sources.source1.channels = c1
    a1.sources.source1.kafka.bootstrap.servers = kafka-host1:port1,kafka-host2:port2...
    a1.sources.source1.kafka.topics = flume-test
    a1.sources.source1.kafka.consumer.group.id = flume-test-group
    
    a1.sinks.k1.type = hdfs
    a1.sinks.k1.hdfs.path = oss://flume-test/result
    a1.sinks.k1.hdfs.fileType=DataStream
    
    # Use a channel which buffers events in memory
    a1.channels.c1.type = memory
    a1.channels.c1.capacity = 100
    a1.channels.c1.transactionCapacity = 100
    
    # Bind the source and sink to the channel
    a1.sources.source1.channels = c1
    a1.sinks.k1.channel = c1
    ```

3.  Start Flume.

    If you have modified the OSS cache size when you configure Flume, use the `--classpath` parameter to pass in OSS-related dependencies and configurations:

    ```
    flume-ng agent --name a1 --conf /etc/ecm/flume-conf  --conf-file flume.properties  --classpath "/opt/apps/extra-jars/*:/etc/ecm/flume-conf/hdfs-site.xml"
    ```

    If you have modified the -Xmx option for Flume agents when you configure Flume, pass in only OSS-related dependencies:

    ```
    flume-ng agent --name a1 --conf /etc/ecm/flume-conf  --conf-file flume.properties  --classpath "/opt/apps/extra-jars/*"
    ```

4.  Test data writes.

    After the Kafka cluster uses kafka-console-producer.sh to generate data, Flume generates a file named FlumeData.xxxx in the oss://flume-test/result path. xxxx indicates the current timestamp in milliseconds.


## Kerberos Kafka source

If you consume data of a high-security Kafka cluster, you must also perform the following configurations:

-   Configure Kerberos authentication for your Kafka cluster and copy the generated file test.keytab to the /etc/ecm/flume-conf directory of the Hadoop cluster. For more information about how to configure Kerberos authentication, see [Compatible with the MIT Kerberos authentication protocol](/intl.en-US/Cluster types/Hadoop cluster/Kerberos/Compatible with the MIT Kerberos authentication protocol.md). Copy the krb5.conf file in the /etc/ecm/has-conf/ directory of the Kafka cluster to the /etc/ecm/flume-conf directory of your Hadoop cluster.
-   Configure flume.properties.

    Add the following configurations to the flume.properties file:

    ```
    a1.sources.source1.kafka.consumer.security.protocol = SASL_PLAINTEXT
    a1.sources.source1.kafka.consumer.sasl.mechanism = GSSAPI
    a1.sources.source1.kafka.consumer.sasl.kerberos.service.name = kafka
    ```

-   Configure a Kafka client.
    -   Create a file named flume\\\_jaas.conf in the /etc/ecm/flume-conf directory. Configure the following information in the file:

        ```
        KafkaClient {
          com.sun.security.auth.module.Krb5LoginModule required
          useKeyTab=true
          storeKey=true
          keyTab="/etc/ecm/flume-conf/test.keytab"
          serviceName="kafka"
          principal="test@EMR.${realm}.COM";
        };
        ```

        Replace $\{realm\} with the Kerberos realm of the Kafka cluster. You can use the following method to obtain the Kerberos realm:

        Run the hostname command in the Kafka cluster. A hostname in the emr-header-1.cluster-xxx format is returned. For example, if emr-header-1.cluster-123456 is returned, the Kerberos realm is 123456.

    -   Modify the flume-env.sh file in the /etc/ecm/flume-conf/ directory.

        In the /etc/ecm/flume-conf/ directory, copy the flume-env.sh.template file and rename it flume-env.sh in the same directory. Then, add the following configurations to the flume-env.sh file:

        ```
        export JAVA_OPTS="$JAVA_OPTS -Djava.security.krb5.conf=/etc/ecm/flume-conf/krb5.conf"
        export JAVA_OPTS="$JAVA_OPTS -Djava.security.auth.login.config=/etc/ecm/flume-conf/flume_jaas.conf"
        ```

-   Configure domain names.

    Add the long domain names and IP addresses of the nodes in the Kafka cluster to the /etc/hosts file of the Hadoop cluster. Example of a long domain name: emr-header-1.cluster-123456.

    ![Domain names](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/7101160061/p102866.png)

    **Note:** In the preceding figure, the IP addresses and domain names of the Hadoop cluster are marked with 1 and those of the Kafka cluster are marked with 2.


## Use Flume after cross-region access is configured

After you have configured cross-region access, perform the following configurations to use Flume.

-   Configure Kerberos authentication for your Kafka cluster and copy the generated file test.keytab to the /etc/ecm/flume-conf directory of the Hadoop cluster. For more information about how to configure Kerberos authentication, see [Compatible with the MIT Kerberos authentication protocol](/intl.en-US/Cluster types/Hadoop cluster/Kerberos/Compatible with the MIT Kerberos authentication protocol.md).
-   Configure flume.properties.

    Add the following configurations to the flume.properties file.

    ```
    a1.sources.source1.kafka.consumer.security.protocol = SASL_PLAINTEXT
    a1.sources.source1.kafka.consumer.sasl.mechanism = GSSAPI
    a1.sources.source1.kafka.consumer.sasl.kerberos.service.name = kafka
    ```

-   Configure a Kafka client.
    -   Create a file named flume\\\_jaas.conf in the /etc/ecm/flume-conf directory. Configure the following information in the file:

```
KafkaClient {
  com.sun.security.auth.module.Krb5LoginModule required
  useKeyTab=true
  storeKey=true
  keyTab="/etc/ecm/flume-conf/test.keytab"
  serviceName="kafka"
  principal="test@EMR.${realm}.COM";
};
```

        Replace $\{realm\} with the Kerberos realm of the Kafka cluster. You can use the following method to obtain the Kerberos realm:

        Run the hostname command in the Kafka cluster. A hostname in the emr-header-1.cluster-xxx format is returned. For example, if emr-header-1.cluster-123456 is returned, the Kerberos realm is 123456.

    -   Modify the flume-env.sh file in the /etc/ecm/flume-conf/ directory.

        In the /etc/ecm/flume-conf/ directory, copy the flume-env.sh.template file and rename it flume-env.sh in the same directory. Then, add the following configurations to the flume-env.sh file:

        ```
        export JAVA_OPTS="$JAVA_OPTS -Djava.security.auth.login.config=/etc/ecm/flume-conf/flume_jaas.conf""
        ```


