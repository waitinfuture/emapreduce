# Instructions for using Flume {#concept_fxh_42r_zfb .concept}

E-MapReduce version 3.16.0 and later support Apache Flume. This topic describes how to use Flume to synchronize E-MapReduce Kafka cluster data to HDFS, Hive, and HBase running on E-MapReduce Hadoop clusters, and to Alibaba Cloud OSS.

## Prerequisites {#section_wbr_dfr_zfb .section}

-   You must have selected Flume in the **Optional Service** menu when you created a Hadoop cluster.
-   You must have created a Kafka cluster and created a topic named flume-test to generate data.

**Note:** 

-   If you have created a high security mode Hadoop cluster to consume standard Kafka cluster data, and you need to configure Kerberos authentication on the Hadoop cluster, see [Authentication method compatible with MIT Kerberos](intl.en-US/User Guide/Kerberos authentication/Authentication compatible with MIT Kerberos.md#).
-   If you have created a high security mode Kafka cluster and you need to write data to a standard Hadoop cluster using Flume, see [Kerberos Kafka Source in this topic](#section_l1j_3fs_zfb).
-   If you have created a high security mode Hadoop cluster and a high security mode Kafka cluster, and you need to configure Kerberos, see [Cross-region access](intl.en-US/User Guide/Kerberos authentication/Cross-region access.md#) and [Cross-region access using Flume](#section_oft_fjs_zfb).

## Kafka-\>HDFS {#section_fj5_tgr_zfb .section}

-   Configure Flume

    Create a configuration file named flume.properties, and add the following configurations for the file, where a1.sources.source1.kafka.bootstrap.servers indicates the host and port for a Kafka broker, a1.sources.source1.kafka.topics indicates the Kafka topic where Flume is used to consume data, and a1.sinks.k1.hdfs.path indicates the path where Flume writes data to HDFS.

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

-   Start Flume

    Flume's default configuration file is stored in /etc/ecm/flume-conf. Use the following configuration file to start a Flume agent.

    ```
    flume-ng agent --name a1 --conf /etc/ecm/flume-conf  --conf-file flume.properties
    ```

    After the agent is started, the log logs/flume.log will be generated in the current path due to log4j.properties being in /etc/ecm/flume-conf. You can configure log4j.properties according to your requirements.

-   Test

    Use the kafka-console-producer.sh command and input the test data abc in your Kafka cluster.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/67072/154684598533579_en-US.png)

    Flume generates a file FlumeData.xxx with a timestamp \(in milliseconds\) suffix based on the current time. When you view the file content, you can see the data that you input in Kafka.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/67072/154684598533580_en-US.png)


## Kafka-\>Hive {#section_xmd_bjr_zfb .section}

-   Create a Hive table

    Before Flume writes data into Hive using transactions, you need to set the transactional property when creating a Hive table. The following example shows how to create a table named flume\_test table.

    ```
    create table flume_test (id int, content string)
    clustered by (id) into 2 buckets
    stored as orc  TBLPROPERTIES('transactional'='true');
    ```

-   Configure Flume

    Create a configuration file flume.properties and add the following configurations for the file, where a1.sources.source1.kafka.bootstrap.servers indicates the host and port for a Kafka broker and a1.sinks.k1.hive.metastore indicates a Hive metastore URI. Then, configure the value of hive.metastore.uris in the hive-site.xml file:

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

    Use the kafka-console-producer.sh command and input the comma-separated test data 1,a in your Kafka cluster.

-   Verify the input data

    Note that quering Hive transaction tables require configuration on the Hive client:

    ```
    hive.support.concurrency – true
    hive.exec.dynamic.partition.mode – nonstrict
    hive.txn.manager – org.apache.hadoop.hive.ql.lockmgr.DbTxnManager
    ```

    After the preceding configurations are set, you can query data in the flume\_test table.![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/67072/154684598533581_en-US.png)


## Kafka-\>HBase {#section_alr_zjr_zfb .section}

-   Create a HBase table

    Create a HBase table flume\_test and a column family column.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/67072/154684598533582_en-US.png)

-   Configure Flume

    Create a configuration file flume.properties and add the following configurations, where a1.sources.source1.kafka.bootstrap.servers indicates the host and port for a Kafka broker, a1.sinks.k1.table indicates the name of the HBase table, and a1.sinks.k1.columnFamily indicates the name of the column family:

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

-   Start Flume

    ```
    flume-ng agent --name a1 --conf /etc/ecm/flume-conf  --conf-file flume.properties
    ```

-   Test

    After data is generated using kafka-console-producer.sh in your Kafka cluster, you can query data in HBase.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/67072/154684598533583_en-US.png)


## Kafka-\>OSS {#section_cys_5kr_zfb .section}

-   Create an OSS path

    Create an OSS bucket and directory, such as oss://flume-test/result.

-   Configure Flume

    Flume requires a large amount of JVM memory when writing data to OSS. To resolve this issue, you can:

    -   Reduce the OSS cache size

        Copy the file hdfs-site.xml from /etc/ecm/hadoop-conf to /etc/ecm/flume-conf, and reduce the value of the configuration term smartdata.cache.buffer.size, for example, to 1048576.

    -   Increase the Flume agent's heap size \(Xmx\)

        In the Flume configuration path /etc/ecm/flume-conf, copy configuration file flume-env.sh.template, paste it to the /etc/ecm/flume-conf path , rename it flume-env.sh, and set Xmx, for example, to 1G:

        ```
        export JAVA_OPTS="-Xmx1g"
        ```

    Create a configuration file flume.properties and add the following configurations, where a1.sources.source1.kafka.bootstrap.servers indicates the host and port for a Kafka broker, and a1.sinks.k1.hdfs.path indicates an OSS path:

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

    After data is generated using kafka-console-producer.sh in your Kafka cluster, in the OSS path oss://flume-test/result, a file FlumeData.xxxx is generated with a timestamp \(in milliseconds\) suffix based on the current time.


## Kerberos Kafka source {#section_l1j_3fs_zfb .section}

If high security Kafka cluster data is consumed, you must configure the following variables:

-   In your Kafka cluster, configure Kerberos authentication and copy the generated keytab file test.keytab to the Hadoop cluster path /etc/ecm/flume-conf, and copy the Kafka cluster file /etc/ecm/has-conf/krb5.conf to the Hadoop cluster path /etc/ecm/flume-conf. For more information, see [Authentication method compatible with MIT Kerberos](intl.en-US/User Guide/Kerberos authentication/Authentication compatible with MIT Kerberos.md#).
-   Configure flume.properties

    In the flume.properties file, add the following configurations:

    ```
    a1.sources.source1.kafka.consumer.security.protocol = SASL_PLAINTEXT
    a1.sources.source1.kafka.consumer.sasl.mechanism = GSSAPI
    a1.sources.source1.kafka.consumer.sasl.kerberos.service.name = kafka
    ```

-   Configure the Kafka client
    -   In /etc/ecm/flume-conf, create the file flume\\\_jaas.conf with the following content:

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

        where, $\{realm\} is replaced with a Kerberos realm of your Kafka cluster. To obtain a Kerberos realm, in your Kafka cluster, run the command hostname to obtain a hostname in the form of emr-header-1.cluster-xxx, such as mr-header-1.cluster-123456. The last numeric string 123456 is a Kerberos realm.

    -   Change /etc/ecm/flume-conf/flume-env.sh

        By default, in /etc/ecm/flume-conf/, the file flume-env.sh does not exist. You need to copy flume-env.sh.template, paste it to /etc/ecm/flume-conf/, rename it flume-env.sh, and add the following content:

        ```
        export JAVA_OPTS="$JAVA_OPTS -Djava.security.krb5.conf=/etc/ecm/flume-conf/krb5.conf"
        export JAVA_OPTS="$JAVA_OPTS -Djava.security.auth.login.config=/etc/ecm/flume-conf/flume_jaas.conf"
        ```

-   Set domain name

    Add domain names and IP binding information of each Kafka cluster node to /etc/hosts of the Hadoop cluster. The form of the long domain name is emr-header-1.cluster-123456.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/67072/154684598533590_en-US.png)


## Cross-region access using Flume {#section_oft_fjs_zfb .section}

After cross-region access is configured, you need to set other configurations as follows:

-   In your Kafka cluster, configure Kerberos authentication and copy the generated keytab file test.keytab to the Hadoop cluster path /etc/ecm/flume-conf. For more information, see [Authentication method compatible with MIT Kerberos](intl.en-US/User Guide/Kerberos authentication/Authentication compatible with MIT Kerberos.md#).
-   Configure flume.properties

    In the flume.properties file, add the following configurations:

    ```
    a1.sources.source1.kafka.consumer.security.protocol = SASL_PLAINTEXT
    a1.sources.source1.kafka.consumer.sasl.mechanism = GSSAPI
    a1.sources.source1.kafka.consumer.sasl.kerberos.service.name = kafka
    ```

-   Configure the Kafka client
    -   In /etc/ecm/flume-conf, create the file flume\\\_jaas.conf with the following content:

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

        where, $\{realm\} is replaced with a Kerberos realm of your Kafka cluster. To obtain a Kerberos realm, in your Kafka cluster, run the command hostname to obtain a hostname in the form of emr-header-1.cluster-xxx, such as emr-header-1.cluster-123456. The last numeric string 123456 is a Kerberos realm.

    -   Change /etc/ecm/flume-conf/flume-env.sh

        By default, in /etc/ecm/flume-conf/, the file flume-env.sh does not exist. You need to copy flume-env.sh.template, paste it to /etc/ecm/flume-conf/, rename it flume-env.sh, and add the following content:

        ```
        export JAVA_OPTS="$JAVA_OPTS -Djava.security.auth.login.config=/etc/ecm/flume-conf/flume_jaas.conf""
        ```


