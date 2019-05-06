# How to use Flume {#concept_fxh_42r_zfb .concept}

E-MapReduce supports Apache Flume since V3.16.0. This topic describes how to use Flume to copy data from an EMR Kafka cluster to HDFS, Hive, HBase, and Alibaba Cloud OSS of an EMR Hadoop cluster.

## Preparations {#section_wbr_dfr_zfb .section}

-   Select Flume from the **optional services** when you create a Hadoop cluster.
-   Create a Kafka cluster and create a topic named flume-test for generating data.

**Note:** 

-   If you create a high-security Hadoop cluster that consumes data of a standard Kafka cluster, see [Authentication method compatible with MIT Kerberos](intl.en-US/Open Source Components /Kerberos authentication/Authentication compatible with MIT Kerberos.md#) for configuring Kerberos authentication in a Hadoop cluster.
-   If you create a high-security Kafka cluster that writes data to a standard Hadoop cluster by using Flume, see [Kerberos Kafka source](#section_l1j_3fs_zfb).
-   If your Hadoop and Kafka clusters are both high-security clusters. See [Cross-region access](intl.en-US/Open Source Components /Kerberos authentication/Cross-region access.md#) and the [Cross-region access using Flume](#section_oft_fjs_zfb) section.

## Kafka-\>HDFS {#section_fj5_tgr_zfb .section}

-   Configure Flume

    Create a configuration file named flume.properties and add the following configurations. Set the value of the a1.sources.source1.kafka.bootstrap.servers configuration item to the hostnames and port numbers of Kafka brokers. a1.sources.source1.kafka.topics refers to the Kafka topic that Flume consumes. a1.sinks.k1.hdfs.path refers to the HDFS path to which Flume writes data.

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

    **Note:** Assume that you specify the a1.sinks.k1.hdfs.path configuration item with a URL. Use the hdfs://emr-cluster prefix for a high-availability cluster. For example,

    ```
    a1.sinks.k1.hdfs.path = hdfs://emr-cluster/tmp/flume/test-data
    ```

    Use the hdfs://emr-header-1:9000 prefix for a standard cluster. For example,

    ```
    a1.sinks.k1.hdfs.path = hdfs://emr-header-1:9000/tmp/flume/test-data
    ```

-   Start Flume

    The default configuration file of Flume is stored in the /etc/ecm/flume-conf path. Use the configuration file to start a Flume agent.

    ```
    flume-ng agent --name a1 --conf /etc/ecm/flume-conf  --conf-file flume.properties 
    ```

    By using the log4j.properties file in the /etc/ecm/flume-conf path, the logs/flume.log log file is generated after the agent is started. You can configure the log4j.properties file as needed.

-   Test

    Use the kafka-console-producer.sh file on your cluster and enter test data abc.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/67072/155712226533579_en-US.png)

    Flume generates a file named FlumeData.xxxx in HDFS with a current timestamp in milliseconds. In the file, you can view the data that you enter on Kafka.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/67072/155712226533580_en-US.png)


## Kafka-\>Hive {#section_xmd_bjr_zfb .section}

-   Create a Hive table

    Flume uses transactions to write data to Hive. You need to specify the transactional property when creating a Hive table. Take creating table flume\_test as an example:

    ```
    create table flume_test (id int, content string)
    clustered by (id) into 2 buckets
    stored as orc  TBLPROPERTIES('transactional'='true'); 
    ```

-   Configure Flume

    Create a configuration file named flume.properties and add the following configurations. Set the value of the a1.sources.source1.kafka.bootstrap.servers configuration item to the hostnames and port numbers of Kafka brokers. a1.sinks.k1.hive.metastore refers to the URI of the Hive metastore. Set the value to the value of the hive.metastore.uris configuration item in the hive-site.xml file.

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

-   Start Flume

    ```
    flume-ng agent --name a1 --conf /etc/ecm/flume-conf  --conf-file flume.properties
    ```

-   Generate data

    Use the kafka-console-producer.sh file on the Kafka cluster. Enter test data 1 and a that are separated by commas \(,\).

-   Test data writing

    Perform the following configurations for querying Hive transaction tables.

    ```
    hive.support.concurrency – true
    hive.exec.dynamic.partition.mode – nonstrict
    hive.txn.manager – org.apache.hadoop.hive.ql.lockmgr.DbTxnManager
    ```

    Query the data in the flume\_test table after the configurations are complete.![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/67072/155712226533581_en-US.png)


## Kafka-\>HBase {#section_alr_zjr_zfb .section}

-   Create an HBase table

    Create HBase table flume\_test and column column.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/67072/155712226633582_en-US.png)

-   Configure Flume

    Create a configuration file named flume.properties and add the following configurations. Set the value of the a1.sources.source1.kafka.bootstrap.servers configuration item to the hostnames and port numbers of Kafka brokers. a1.sinks.k1.table refers to the HBase table name. a1.sinks.k1.columnFamily refers to the column name.

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

-   Start Flume agent

    ```
    flume-ng agent --name a1 --conf /etc/ecm/flume-conf  --conf-file flume.properties
    ```

-   Test

    After data is generated using kafka-console-producer.sh in your Kafka cluster, you can query data in HBase.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/67072/155712226633583_en-US.png)


## Kafka-\>OSS {#section_cys_5kr_zfb .section}

-   Create an OSS path

    Create an OSS bucket and the folder such as oss://flume-test/result.

-   Configure Flume

    Flume writing data to OSS takes up much JVM memory. You can reduce the OSS cache size or increase the Xmx value for Flume agents.

    -   Modify the OSS cache size

        Copy the hdfs-site.xml file in the /etc/ecm/hadoop-conf path and paste it in the /etc/ecm/flume-conf path. Reduce the value of smartdata.cache.buffer.size. For example, 1048576.

    -   Modify Xmx

        In the Flume configuration path /etc/ecm/flume-conf, copy configuration file flume-env.sh.template, paste it to the /etc/ecm/flume-conf path , rename it flume-env.sh, and set Xmx, for example, to 1G:

        ```
        export JAVA_OPTS="-Xmx1g"
        ```

    Create a configuration file named flume.properties and add the following configurations. Set the value of the a1.sources.source1.kafka.bootstrap.servers configuration item to the hostnames and port numbers of Kafka brokers. Set the value of a1.sinks.k1.hdfs.path to the OSS path.

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

-   Start Flume

    If you modified the OSS cache size when configuring Flume, use the classpath parameter to pass OSS-related dependencies and configurations to Flume:

    ```
    flume-ng agent --name a1 --conf /etc/ecm/flume-conf  --conf-file flume.properties  --classpath "/opt/apps/extra-jars/*:/etc/ecm/flume-conf/hdfs-site.xml" 
    ```

    If you modified the Flume agent's Xmx, you only need to pass OSS-related dependencies:

    ```
    flume-ng agent --name a1 --conf /etc/ecm/flume-conf  --conf-file flume.properties  --classpath "/opt/apps/extra-jars/*"
    ```

-   Test

    After the Kafka cluster uses kafka-console-producer.sh to generate data, the FlumeData.xxxx file is generated with the current timestamp \(unit: milliseconds\) as the filename suffix in the oss://flume-test/result path.


## Kerberos Kafka source {#section_l1j_3fs_zfb .section}

When you consume data of high-security Kafka clusters, you need extra configurations.

-   In your Kafka cluster, configure Kerberos authentication and copy the generated keytab file test.keytab to the Hadoop cluster path /etc/ecm/flume-conf, and copy the Kafka cluster file /etc/ecm/has-conf/krb5.conf to the Hadoop cluster path /etc/ecm/flume-conf. For more information, see [Authentication method compatible with MIT Kerberos](intl.en-US/Open Source Components /Kerberos authentication/Authentication compatible with MIT Kerberos.md#).
-   Configure flume.properties

    Add the following configurations in the flume.properties file.

    ```
    a1.sources.source1.kafka.consumer.security.protocol = SASL_PLAINTEXT
    a1.sources.source1.kafka.consumer.sasl.mechanism = GSSAPI
    a1.sources.source1.kafka.consumer.sasl.kerberos.service.name = kafka
    ```

-   Configure Kafka clients
    -   Create the flume\\\_jaas.conf file in the /etc/ecm/flume-conf path. Enter the following configurations.

        ```
        KafkaClient {
          com.sun.security.auth.module.Krb5LoginModule required
          useKeyTab=true
          storeKey=true
          keyTab="/etc/ecm/flume-conf/test.keytab"
          serviceName="kafka"
          principal="test@EMR.${realm}. COM";
        };
        ```

        Replace $\{realm\} with the Kerberos realm of the Kafka cluster. Run the hostname command on the Kafka cluster and a hostname in the emr-header-1.cluster-xxx format is returned such as emr-header-1.cluster-123456. “123456” is the realm.

    -   Modify /etc/ecm/flume-conf/flume-env.sh

        Initially, the flume-env.sh file is not in the /etc/ecm/flume-conf/ path. You need to copy and paste the flume-env.sh.template and rename it flume-env.sh. Enter the following configurations.

        ```
        export JAVA_OPTS="$JAVA_OPTS -Djava.security.krb5.conf=/etc/ecm/flume-conf/krb5.conf"
        export JAVA_OPTS="$JAVA_OPTS -Djava.security.auth.login.config=/etc/ecm/flume-conf/flume_jaas.conf"
        ```

-   Set the domains

    Add the domain names and IP addresses of the nodes in the Kafka cluster to the /etc/hosts file on the Hadoop cluster. An example of long domains is emr-header-1.cluster-123456.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/67072/155712226633590_en-US.png)


## Use Flume with cross-region access {#section_oft_fjs_zfb .section}

After configuring cross-region access, perform the following steps.

-   On your Kafka cluster, configure Kerberos authentication and copy the generated keytab file test.keytab to the Hadoop cluster path /etc/ecm/flume-conf. For more information, see [Authentication method compatible with MIT Kerberos](intl.en-US/Open Source Components /Kerberos authentication/Authentication compatible with MIT Kerberos.md#).
-   Configure flume.properties

    Add the following configurations in the flume.properties file.

    ```
    a1.sources.source1.kafka.consumer.security.protocol = SASL_PLAINTEXT
    a1.sources.source1.kafka.consumer.sasl.mechanism = GSSAPI
    a1.sources.source1.kafka.consumer.sasl.kerberos.service.name = kafka
    ```

-   Configure Kafka clients
    -   Create the flume\\\_jaas.conf file in the /etc/ecm/flume-conf path. Enter the following configurations.

```
KafkaClient {
  com.sun.security.auth.module.Krb5LoginModule required
  useKeyTab=true
  storeKey=true
  keyTab="/etc/ecm/flume-conf/test.keytab"
  serviceName="kafka"
  principal="test@EMR.${realm}. COM";
};
```

        Replace $\{realm\} with the Kerberos realm of the Kafka cluster. Run the hostname command on the Kafka cluster and a hostname in the emr-header-1.cluster-xxx format is returned such as emr-header-1.cluster-123456. “123456” is the realm.

    -   Modify /etc/ecm/flume-conf/flume-env.sh

        Initially, the flume-env.sh file is not in the /etc/ecm/flume-conf/ path. You need to copy and paste the flume-env.sh.template and rename it flume-env.sh. Enter the following configurations.

        ```
        export JAVA_OPTS="$JAVA_OPTS -Djava.security.auth.login.config=/etc/ecm/flume-conf/flume_jaas.conf""
        ```


