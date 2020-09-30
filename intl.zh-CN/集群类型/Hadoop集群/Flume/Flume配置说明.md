# Flume配置说明

E-MapReduce（以下简称EMR） 从EMR-3.16.0版本开始支持Apache Flume。本文介绍如何使用Flume将数据从EMR Kafka集群同步至EMR Hadoop集群的HDFS、Hive、HBase以及阿里云OSS。

## 准备工作

-   已登录[阿里云 E-MapReduce 控制台](https://emr.console.aliyun.com/)。
-   创建Hadoop集群时，在**可选服务**中选择Flume。

    **说明：** Flume软件安装目录在/usr/lib/flume-current下，其他常用文件路径获取方式请参见[E-MapReduce常用文件路径](/intl.zh-CN/快速入门/E-MapReduce常用文件路径.md)。

-   创建Kafka集群，并创建名称为flume-test的Topic，用于生成数据。

**说明：**

-   如果创建的是Hadoop高安全集群，消费标准Kafka集群的数据，在Hadoop集群配置Kerberos认证，详情请参见[兼容MIT Kerberos认证](/intl.zh-CN/集群类型/Hadoop集群/Kerberos/兼容 MIT Kerberos 认证.md)。
-   如果创建的是Kafka高安全集群，通过Flume将数据写入标准Hadoop集群，请参见 [Kerberos Kafka Source](#section_urk_t9b_4ud)。
-   如果创建的Hadoop集群和Kafka集群都是高安全集群，需配置跨域互信，详情请参见[跨域互信](/intl.zh-CN/集群类型/Hadoop集群/Kerberos/跨域互信.md)，其它配置请参见[跨域互信使用Flume](#section_oft_fjs_zfb)。

## Kafka-\>HDFS

1.  配置Flume。

    创建配置文件flume.properties，添加如下配置：

    -   a1.sources.source1.kafka.bootstrap.servers：Kafka集群Broker的Host和端口号。
    -   a1.sources.source1.kafka.topics：Flume消费Kafka数据的Topic。
    -   a1.sinks.k1.hdfs.path：Flume向HDFS写入数据的路径。
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

    如果配置项a1.sinks.k1.hdfs.path使用URL的形式，针对不同集群代码示例如下：

    -   高可用集群

        ```
        a1.sinks.k1.hdfs.path = hdfs://emr-cluster/tmp/flume/test-data
        ```

    -   标准集群

        ```
        a1.sinks.k1.hdfs.path = hdfs://emr-header-1:9000/tmp/flume/test-data
        ```

2.  启动服务。

    Flume默认配置文件存储在/etc/ecm/flume-conf下，使用该配置启动Flume Agent。

    ```
    flume-ng agent --name a1 --conf /etc/ecm/flume-conf  --conf-file flume.properties
    ```

    启动Agent时，因为使用了/etc/ecm/flume-conf下的log4j.properties，所以会在当前路径下生成日志logs/flume.log，您可根据实际情况对log4j.properties进行配置。

3.  测试数据写入情况。

    在Kafka集群使用kafka-console-producer.sh输入测试数据abc。

    ![test_abc](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9354449951/p33579.png)

    Flume会在HDFS中以当前时间的（毫秒）时间戳生成文件FlumeData.xxxx。文件内容是在Kafka中输入的数据。

    ![check_kafka_data](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9354449951/p33580.png)


## Kafka-\>Hive

1.  创建Hive表。

    Flume使用事务操作将数据写入Hive，需要在创建Hive表时设置transactional属性，例如创建flume\_test表。

    ```
    create table flume_test (id int, content string)
    clustered by (id) into 2 buckets
    stored as orc  TBLPROPERTIES('transactional'='true');
    ```

2.  配置Flume。

    创建配置文件flume.properties，添加如下配置：

    -   a1.sources.source1.kafka.bootstrap.servers：Kafka集群Broker的Host和端口号。
    -   a1.sinks.k1.hive.metastore：Hive metastore 的URI，配置为hive-site.xml中配置项 hive.metastore.uris的值。
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

3.  启动Flume。

    ```
    flume-ng agent --name a1 --conf /etc/ecm/flume-conf  --conf-file flume.properties
    ```

4.  生成数据。

    在Kafka集群中使用kafka-console-producer.sh，以逗号（,）为分隔符输入测试数据`1,a`。

5.  检测数据写入情况。

    查询Hive事务表并在客户端进行配置。

    ```
    hive.support.concurrency – true
    hive.exec.dynamic.partition.mode – nonstrict
    hive.txn.manager – org.apache.hadoop.hive.ql.lockmgr.DbTxnManager
    ```

    配置好后查询flume\_test表中的数据。

    ![check_flume_test](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9354449951/p33581.png)


## Kafka-\>HBase

1.  创建HBase表。

    创建HBase表flume\_test及列簇column。

    ![create_flume_test](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9354449951/p33582.png)

2.  配置Flume。

    创建配置文件flume.properties，添加如下配置：

    -   a1.sources.source1.kafka.bootstrap.servers：Kafka集群Broker的Host和端口号。
    -   a1.sinks.k1.table：HBase表名。
    -   a1.sinks.k1.columnFamily：列簇名。
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

3.  启动服务。

    ```
    flume-ng agent --name a1 --conf /etc/ecm/flume-conf  --conf-file flume.properties
    ```

4.  测试数据写入情况。

    在Kafka集群使用kafka-console-producer.sh生成数据后，在HBase查到数据。

    ![test](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9354449951/p33583.png)


## Kafka-\>OSS

1.  创建OSS路径。

    创建OSS Bucket及目录，例如oss://flume-test/result。

2.  配置Flume。

    Flume向OSS写入数据时，因为需要占用较大的JVM内存，所以可以改小OSS缓存或者增大Flume Agent的Xmx。

    -   修改OSS缓存大小。

        将hdfs-site.xml配置文件从/etc/ecm/hadoop-conf拷贝至/etc/ecm/flume-conf，改小配置项smartdata.cache.buffer.size的值，例如修改为1048576。

    -   修改Xmx。

        在Flume的配置路径/etc/ecm/flume-conf下，复制配置文件flume-env.sh.template并重命名为flume-env.sh，设置Xmx的值，例如设置为1g。

        ```
        export JAVA_OPTS="-Xmx1g"
        ```

    创建配置文件flume.properties，添加如下配置：

    -   a1.sources.source1.kafka.bootstrap.servers：Kafka集群Broker的Host和端口号。
    -   a1.sinks.k1.hdfs.path：OSS路径。
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

3.  启动Flume。

    如果配置Flume时修改了OSS缓存大小，需要使用`--classpath`参数传入OSS相关依赖和配置。

    ```
    flume-ng agent --name a1 --conf /etc/ecm/flume-conf  --conf-file flume.properties  --classpath "/opt/apps/extra-jars/*:/etc/ecm/flume-conf/hdfs-site.xml"
    ```

    如果修改了Flume Agent的Xmx，只需要传入OSS相关依赖。

    ```
    flume-ng agent --name a1 --conf /etc/ecm/flume-conf  --conf-file flume.properties  --classpath "/opt/apps/extra-jars/*"
    ```

4.  测试数据写入情况。

    在Kafka集群使用kafka-console-producer.sh生成数据后，在OSS的oss://flume-test/result路径下会以当前时间的（毫秒）时间戳为后缀生成文件FlumeData.xxxx。


## Kerberos Kafka source

消费高安全Kafka集群的数据时，需要完成额外的配置：

-   在Kafka集群配置Kerberos认证，将生成的keytab文件test.keytab拷贝至Hadoop集群的/etc/ecm/flume-conf路径下，详情请参见[兼容MIT Kerberos认证](/intl.zh-CN/集群类型/Hadoop集群/Kerberos/兼容 MIT Kerberos 认证.md)；将Kafka集群的 /etc/ecm/has-conf/krb5.conf文件拷贝至Hadoop集群的/etc/ecm/flume-conf路径下。
-   配置flume.properties。

    在flume.properties中添加如下配置。

    ```
    a1.sources.source1.kafka.consumer.security.protocol = SASL_PLAINTEXT
    a1.sources.source1.kafka.consumer.sasl.mechanism = GSSAPI
    a1.sources.source1.kafka.consumer.sasl.kerberos.service.name = kafka
    ```

-   配置Kafka client。
    -   在/etc/ecm/flume-conf下创建文件flume\\\_jaas.conf，内容如下。

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

        其中，$\{realm\} 替换为Kafka集群的Kerberos realm。获取方式如下。

        在Kafka集群执行命令hostname，得到形式为emr-header-1.cluster-xxx的主机名，例如emr-header-1.cluster-123456，最后的数字串123456即为realm。

    -   修改/etc/ecm/flume-conf/flume-env.sh。

        初始情况下，/etc/ecm/flume-conf/下没有flume-env.sh 文件，需要拷贝flume-env.sh.template并重命名为flume-env.sh。添加如下内容。

        ```
        export JAVA_OPTS="$JAVA_OPTS -Djava.security.krb5.conf=/etc/ecm/flume-conf/krb5.conf"
        export JAVA_OPTS="$JAVA_OPTS -Djava.security.auth.login.config=/etc/ecm/flume-conf/flume_jaas.conf"
        ```

-   设置域名。

    将Kafka集群各节点的长域名和IP的绑定信息添加到Hadoop集群的/etc/hosts。长域名的形式例如emr-header-1.cluster-123456。

    ![域名](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9354449951/p102866.png)

    **说明：** 图中标注①表示的是本集群（即Hadoop集群）域名；图中标注②表示新增加的Kafka集群域名。


## 跨域互信使用Flume

在配置了跨域互信后，其他配置如下：

-   参见[兼容MIT Kerberos认证](/intl.zh-CN/集群类型/Hadoop集群/Kerberos/兼容 MIT Kerberos 认证.md)在Kafka集群配置Kerberos认证，将生成的keytab文件test.keytab拷贝至Hadoop集群的/etc/ecm/flume-conf路径下。
-   配置flume.properties

    在flume.properties中添加如下配置。

    ```
    a1.sources.source1.kafka.consumer.security.protocol = SASL_PLAINTEXT
    a1.sources.source1.kafka.consumer.sasl.mechanism = GSSAPI
    a1.sources.source1.kafka.consumer.sasl.kerberos.service.name = kafka
    ```

-   配置Kafka client。
    -   在/etc/ecm/flume-conf下创建文件flume\\\_jaas.conf，内容如下。

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

        其中，$\{realm\}替换为Kafka集群的Kerberos realm。获取方式如下。

        在Kafka集群执行命令hostname，得到形式为emr-header-1.cluster-xxx的主机名，如emr-header-1.cluster-123456，最后的数字串123456即为realm。

    -   修改/etc/ecm/flume-conf/flume-env.sh。

        初始情况下，/etc/ecm/flume-conf/下没有flume-env.sh文件，需要拷贝flume-env.sh.template并重命名为flume-env.sh。添加如下内容。

        ```
        export JAVA_OPTS="$JAVA_OPTS -Djava.security.auth.login.config=/etc/ecm/flume-conf/flume_jaas.conf""
        ```


