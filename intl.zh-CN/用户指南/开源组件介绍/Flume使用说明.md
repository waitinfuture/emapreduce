# Flume使用说明 {#concept_fxh_42r_zfb .concept}

E-MapReduce从3.16.0版本开始支持Apache Flume。本文介绍使用Flume将数据从EMR Kafka集群同步至EMR Hadoop集群的HDFS、Hive和HBase，以及阿里云的OSS。

## 准备工作 {#section_wbr_dfr_zfb .section}

-   创建Hadoop集群时，在**可选服务**中选择Flume。
-   创建Kafka集群，并创建名称为flume-test的topic，用于生成数据。

**说明：** 

-   如果创建的是Hadoop高安全集群，消费标准Kafka集群的数据，参照[兼容MIT Kerberos认证](intl.zh-CN/用户指南/Kerberos认证/兼容MIT Kerberos认证.md#)在Hadoop集群配置Kerberos认证；
-   如果创建的是Kafka高安全集群，通过Flume将数据写入标准Hadoop集群，参见[Kerberos Kafka Source](#section_l1j_3fs_zfb)部分；
-   如果创建的Hadoop集群和Kafka集群都是高安全集群，参照[跨域互信](intl.zh-CN/用户指南/Kerberos认证/跨域互信.md#)进行配置，参见[跨域互信使用Flume](#section_oft_fjs_zfb)；

## Kafka-\>HDFS {#section_fj5_tgr_zfb .section}

-   配置Flume

    创建配置文件flume.properties，添加如下配置。其中，配置项a1.sources.source1.kafka.bootstrap.servers为Kafka集群broker的host和端口号，a1.sources.source1.kafka.topics为Flume消费Kafka数据的topic，a1.sinks.k1.hdfs.path为Flume向HDFS写入数据的路径：

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

-   启动服务

    Flume的默认配置文件放在/etc/ecm/flume-conf下，使用该配置启动Flume Agent：

    ```
    flume-ng agent --name a1 --conf /etc/ecm/flume-conf  --conf-file flume.properties
    ```

    启动Agent后，因为使用了/etc/ecm/flume-conf下的log4j.properties，会在当前路径下生成日志logs/flume.log，可根据实际使用对log4j.properties进行配置。

-   测试

    在Kafka集群使用kafka-console-producer.sh输入测试数据abc

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/67072/154442413833579_zh-CN.png)

    Flume会在HDFS中以当前时间的\(毫秒\)时间戳生成文件FlumeData.xxxx，查看文件内容，会看到在Kafka中输入的数据

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/67072/154442413833580_zh-CN.png)


## Kafka-\>Hive {#section_xmd_bjr_zfb .section}

-   创建Hive表

    Flume使用事务操作将数据写入Hive，需要在创建Hive表时设置transactional属性，如创建flume\_test表：

    ```
    create table flume_test (id int, content string)
    clustered by (id) into 2 buckets
    stored as orc  TBLPROPERTIES('transactional'='true');
    ```

-   配置Flume

    创建配置文件flume.properties，添加如下配置。其中，配置项a1.sources.source1.kafka.bootstrap.servers填写Kafka集群broker的host和端口号，a1.sinks.k1.hive.metastore为Hive metastore URI，配置为hive-site.xml中配置项hive.metastore.uris的值:

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

-   启动Flume

    ```
    flume-ng agent --name a1 --conf /etc/ecm/flume-conf  --conf-file flume.properties
    ```

-   生成数据

    在Kafka集群中使用kafka-console-producer.sh，以逗号为分隔符输入测试数据 1,a

-   检测数据写入

    查询Hive事务表需要在客户端进行配置：

    ```
    hive.support.concurrency – true
    hive.exec.dynamic.partition.mode – nonstrict
    hive.txn.manager – org.apache.hadoop.hive.ql.lockmgr.DbTxnManager
    ```

    配置好后查询flume\_test表中的数据![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/67072/154442413833581_en-US.png)


## Kafka-\>HBase {#section_alr_zjr_zfb .section}

-   创建HBase表

    创建HBase表flume\_test及列簇column

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/67072/154442413833582_zh-CN.png)

-   配置Flume

    创建配置文件flume.properties，添加如下配置。其中，配置项a1.sources.source1.kafka.bootstrap.servers为Kafka集群broker的host和端口号，a1.sinks.k1.table为HBase表名，a1.sinks.k1.columnFamily为列簇名：

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

-   启动服务

    ```
    flume-ng agent --name a1 --conf /etc/ecm/flume-conf  --conf-file flume.properties
    ```

-   测试

    在Kafka集群使用kafka-console-producer.sh生成数据后，就可以在HBase查到数据

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/67072/154442413833583_zh-CN.png)


## Kafka-\>OSS {#section_cys_5kr_zfb .section}

-   创建OSS路径

    创建OSS Bucket及目录，如oss://flume-test/result

-   配置Flume

    Flume向OSS写入数据时，需要占用较大的JVM内存，可以改小OSS缓存或者增大Flume Agent的Xmx：

    -   修改OSS缓存大小

        将hdfs-site.xml配置文件从/etc/ecm/hadoop-conf拷贝至/etc/ecm/flume-conf，改小配置项smartdata.cache.buffer.size的值，如修改为1048576。

    -   修改Xmx

        在Flume的配置路径/etc/ecm/flume-conf下，复制配置文件flume-env.sh.template并重命名为flume-env.sh，设置Xmx，如设置为1g ：

        ```
        export JAVA_OPTS="-Xmx1g"
        ```

    创建配置文件flume.properties，添加如下配置。其中，配置项a1.sources.source1.kafka.bootstrap.servers填写kafka集群broker的host和端口号，a1.sinks.k1.hdfs.path为OSS路径：

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

-   启动Flume

    如果配置Flume时修改了OSS缓存大小，使用classpath参数传入OSS相关依赖和配置：

    ```
    flume-ng agent --name a1 --conf /etc/ecm/flume-conf  --conf-file flume.properties  --classpath "/opt/apps/extra-jars/*:/etc/ecm/flume-conf/hdfs-site.xml"
    ```

    如果修改了Flume Agent的Xmx，只需传入OSS相关依赖：

    ```
    flume-ng agent --name a1 --conf /etc/ecm/flume-conf  --conf-file flume.properties  --classpath "/opt/apps/extra-jars/*"
    ```

-   测试

    在Kafka集群使用kafka-console-producer.sh生成数据后，在OSS的oss://flume-test/result路径下会以当前时间的\(毫秒\)时间戳为后缀生成文件FlumeData.xxxx


## Kerberos Kafka source {#section_l1j_3fs_zfb .section}

消费高安全Kafka集群的数据时，需要做额外的配置：

-   参照[兼容MIT Kerberos认证](intl.zh-CN/用户指南/Kerberos认证/兼容MIT Kerberos认证.md#)在Kafka集群配置Kerberos认证，将生成的keytab文件test.keytab拷贝至Hadoop集群的/etc/ecm/flume-conf路径下；将Kafka集群的/etc/ecm/has-conf/krb5.conf文件拷贝至Hadoop集群的/etc/ecm/flume-conf路径下。
-   配置flume.properties

    在flume.properties中添加如下配置：

    ```
    a1.sources.source1.kafka.consumer.security.protocol = SASL_PLAINTEXT
    a1.sources.source1.kafka.consumer.sasl.mechanism = GSSAPI
    a1.sources.source1.kafka.consumer.sasl.kerberos.service.name = kafka
    ```

-   配置Kafka client
    -   在/etc/ecm/flume-conf下创建文件flume\\\_jaas.conf，内容如下：

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

        其中，$\{realm\}替换为Kafka集群的Kerberos realm。获取方式为，在Kafka集群执行命令hostname，得到形式为emr-header-1.cluster-xxx的主机名ame，如emr-header-1.cluster-123456，最后的数字串123456即为realm

    -   修改/etc/ecm/flume-conf/flume-env.sh

        初始情况下，/etc/ecm/flume-conf/下没有flume-env.sh文件，需要拷贝flume-env.sh.template并重命名为flume-env.sh。添加如下内容：

        ```
        export JAVA_OPTS="$JAVA_OPTS -Djava.security.krb5.conf=/etc/ecm/flume-conf/krb5.conf"
        export JAVA_OPTS="$JAVA_OPTS -Djava.security.auth.login.config=/etc/ecm/flume-conf/flume_jaas.conf"
        ```

-   设置域名

    将Kafka集群各节点的长域名和IP的绑定信息添加到Hadoop集群的/etc/hosts。长域名的形式例如emr-header-1.cluster-123456

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/67072/154442413833590_zh-CN.png)


## 跨域互信使用Flume {#section_oft_fjs_zfb .section}

在配置了跨域互信后，其他配置如下：

-   参照[兼容MIT Kerberos认证](intl.zh-CN/用户指南/Kerberos认证/兼容MIT Kerberos认证.md#)在Kafka集群配置Kerberos认证，将生成的keytab文件test.keytab拷贝至Hadoop集群的/etc/ecm/flume-conf路径下。
-   配置flume.properties

    在flume.properties中添加如下配置：

    ```
    a1.sources.source1.kafka.consumer.security.protocol = SASL_PLAINTEXT
    a1.sources.source1.kafka.consumer.sasl.mechanism = GSSAPI
    a1.sources.source1.kafka.consumer.sasl.kerberos.service.name = kafka
    ```

-   配置Kafka client
    -   在/etc/ecm/flume-conf下创建文件flume\\\_jaas.conf，内容如下：

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

        其中，$\{realm\}替换为Kafka集群的Kerberos realm。获取方式为，在Kafka集群执行命令hostname，得到形式为emr-header-1.cluster-xxx的主机名，如emr-header-1.cluster-123456，最后的数字串123456即为realm

    -   修改/etc/ecm/flume-conf/flume-env.sh

        初始情况下，/etc/ecm/flume-conf/下没有flume-env.sh文件，需要拷贝flume-env.sh.template并重命名为flume-env.sh。添加如下内容：

        ```
        export JAVA_OPTS="$JAVA_OPTS -Djava.security.auth.login.config=/etc/ecm/flume-conf/flume_jaas.conf""
        ```


